## ARTS

### Review

# Laravel-从百草园到三味书屋

## 第二章
## The IoC Container 控制反转容器
## Basic Binding 基础绑定
Now that we've learned about dependency injection, let's explore inversion of control containers. IoC containers make managing your class dependencies much more convenient, and Laravel ships with a very powerful container. The IoC container is the certral piece of the Laravel framework, and it is what allows all of the framework's jcomponents jto work together. In fact, the Laravel Application class extends the Container class!

我们已经学习了依赖注入，接下来咱们一起来探索“控制反转容器”（IoC）。 IoC容器可以使你更容易管理依赖注入，Laravel框架拥有一个很强大的IoC容器。Laravel的核心就是这个IoC容器，这个IoC容器使得框架各个组件能很好的在一起工作。事实上Laravel的Application类就是继承自Container类！
>IoC Container 控制反转容器

>Inversion of control containers make dependency injection more convenient. How to resolve a given class or interface is defined once in the container, which manages resolving and injecting those objects throughout your application.

>控制反转容器使得依赖注入更方便。当一个类或接口在容器里定义以后，如何处理它们——如何在应用中管理、注入这些对象？

In a Laravel application, the IoC container can be accessed via the App facade. The container has a variety of methods, but we'll start with the most basic. Let's continue to use our BillerInterface and BillingNotifierInterface from the previous chapter, and assume that our application is using [Stripe](https://stripe.com/) to process payments. We can bind the Stripe implementation of the interface to the container like this:

在Laravel应用里，你可以通过App来访问控制反转容器。容器有很多方法，不过我们从最基础的开始。让我们继续使用上一章写的BillerInterface和BillingNotifierInterface，且假设我们使用了[Stripe](https://github.com/fabpot/pimple)来进行支付操作。我们可以将Stripe的支付实现绑定到容器里，就像这样：
```
<!-- lang: php -->
App::bind('BillerInterface', function()
{
    return new StripeBiller(App::make('BillingNotifierInterface'));
});
```
Notice that within our BillingInterface resolver, we also resolve a BillingNotifierInterface implementation. Let's define that binding as well:

注意在我们处理BillingInterface时，我们额外需要一个BillingNotifierInterface的实现，也就是再来一个bind：
```
<!-- lang: php -->
App::bind('BillingNotifierInterface', function()
{
    return new EmailBillingNotifier;
});
```
So, as you can see, the container is a place to store Closures that resolve various classes. Once a class has been registered with the container, we can easily resolve it from anywhere in our application. We can even resolve other container bindings within a resolver.

如你所见， 这个容器就是个用来存储各种绑定的地方（译者注：这么理解简单点。再扯匿名函数、闭包就扯远了。）。一旦一个类在容器里绑定了以后，我们可以很容易的在应用的任何位置调用它。我们甚至可以在bind函数内写另外的bind。
>Have Acne?

>The Laravel IoC container is a drop-in replacement for the [Pimple](https://github.com/fabpot/pimple) IoC container by Fabien Potencier. So, if you're already using Pimple on a project, feel free to upgrade to the [Illuminate Container](https://github.com/jilluminate/container) component for a few more features!

>Laravel框架的Illuminate容器和另一个名为[Pimple](https://github.com/fabpot/pimple)的IoC容器是可替换的。所以如果你之前用的是Pimple，你尽可以大胆的升级为[Illuminate Container](https://github.com/jilluminate/container)，后者还有更多新功能！

Once we're using the container, we can switch interface implementations with a single line change. For example, consider the following:

一旦我们使用了容器，切换接口的实现就是一行代码的事儿。 比如考虑以下代码：
```
<!-- lang: php -->
class UserController extends BaseController{
    public function __construct(BillerInterface $biller)
    {
        $this->biller = $biller;
    }
}
```
When this controller is instantiated via the IoC container, the StripeBiller, which includes the EmailBillingNotifier, will be injected into the instance. Now, if we want to change our notifier implementation, we can simply change the binding to this:

当这个控制器通被容器实例化后，包含着EmailBillingNotifier的StripeBiller会被注入到这个控制器中（译者注：见上文的两个bind）。如果我们现在想要换一种提示方式，我们可以简单的将代码改为这样：

```
<!-- lang: php -->
App::bind('BillingNotifierInterface', function()
{
    return new SmsBillingNotifier;
});
```
Now, it doesn't matter where the notifier is resolved in our application, we will now always get the SmsBillingNotifier implementation. Utilizing this architecture, our application can be rapidly shifted to new implementations of various services.

现在不管在应用的哪里需要一个提示器，我们总会得到SmsBillingNotifier的对象。利用这种结构，我们的应用可以在不同的实现方式之间快速切换。

Being able to change implementations of an interface with a single line is amazingly powerful. For example, imagine we want to change our SMS service from a legacy provider to Twilio. We can develop a new Twilio implementation of the notifier and swap our binding. If we have problems with the transition to Twilio, we can quickly change back to the legacy provider by making a single IoC binding change. As you can see, the benefits of using dependency injection go beyond what is immediately obvious. Can you think of more benefits for using dependency injection and an IoC container?

只改一行就能切换代码实现，这可是很厉害的能力。比如我们想把短信服务从原来的提供商替换为Twilio。我们可以开发一个新的Twilio的提示器类（译者注：当然要继承自BillingNotifierInterface）然后修改绑定语句。如果Twilio有任何闪失，我们只需修改一行代码就可以快速的切换回原来的短信提供商。看到了吧，依赖注入的好处多得很呢。你能再想出几个使用依赖注入和控制反转容器的好处么？

Sometimes you may wish to resolve only one instance of a given class throughout your entire application. This can be achieved via the singleton method on the container class:

想在应用中只实例化某类一次？没问题，使用singleton方法吧：
```
<!-- lang: php -->
App::singleton('BillingNotifierInterface', function()
{
    return new SmsBillingNotifier;
});
```
Now, once the container has resolved the billing notifier once, it will continue to use that same instance for all subsequent requests for that interface.

这样只要这个容器生成了这个提示器对象一次， 在接下来的生成请求中容器都只会提供这同样的一个对象。

The instance method on the container is similar to singleton, however, you are able to pass an already existing object instance. The instance you give to the container will be used each time the container needs an instance of that class:

容器的instance方法和singleton方法很类似，区别是instance可以绑定一个已经存在的对象。然后容器每次返回的都是这个对象了。

```
<!-- lang: php -->
$notifier = new SmsBillingNotifier;
App::instance('BillingNotifierInterface', $notifier);
```

Now that we're familiar with basic container resolution using Closures, let's dig into its most powerful feature: the ability to resolve class via reflection.

现在我们熟悉了容器的基础用法，让我们深入发掘它更强大的功能：依靠反射来处理类和接口。

>Stand Alone Container 容器独立运行
>Working on a project that isn't built on Laravel? You can still utilize Laravel's IoC container by installing the illuminate/container package via Composer!
>你的项目没有使用Laravel？但你依然可以使用Laravel的IoC容器！只要用Composer安装了illuminate/container包就可以了。


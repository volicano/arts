## ARTS

### Review

# Laravel-从百草园到三味书屋

## 第一章

## **Dependency Injection** 依赖注入
### **The Problem** 遇到的问题

The foundation of the Laravel framework is its powerful IoC container. To truly understand the framework, a strong grasp of the container is necessary. However, we shold note that an IoC container is simply a convenience mechanism for achieving a software design pattern: dependency injectionl. A container is not necessary to perform dependency injection, it simply makes the task easier.

Laravel框架的基 础是一个功能强大的控制反转容器（IoC container）。 为了真正理解本框架，需要好好掌握该容器。但我们要搞清楚，控制反转容器只是一种用于方便实现“依赖注入”的工具。要实现依赖注入并不一定需要控制反 转容器，只是用容器会更方便和容易一点儿。

First, let's explore why dependency injection is beneficial. Consider the following class and method:

首先来看看我们为何要使用依赖注入，它能带来什么好处。 考虑下列代码：

```
class UserController extends BaseController{
    public function getIndex()
    {
        $users= User::all();
        return View::make('users.index', compact('users'));

    }

} 
```

While this code is concise, we are unable to test it without hitting an actual database. In other words, the Eloquent ORM is tightly coupled to out controller. We have no way to use or test this controller without also using the entire Eloquent ORM, including hitting a live database. This code also violates a software design principle commonly called separation of concerns. Simple put: our controller knows too much. Controllers do not need to know where data comes from, but only how to access it. The controller doesn't need to know that the data is available in MySQL, but only that it is available somewhere.

这段代码很 简短，但我们要想测试这段代码的话就一定会和实际的数据库发生联系。也就是说， Eloquent ORM（译者注：Laravel的数据库对象模型库）和该控制器有着紧耦合。如果不使用Eloquent ORM，不连接到实际数据库，我们就没办法运行或者测试这段代码。这段代码同时也违背了“关注分离”这个软件设计原则。简单讲：这个控制器知道的太多了。 控制器不需要去了解数据是从哪儿来的，只要知道如何访问就行。控制器也不需要知道这数据是从MySQL或哪儿来的，只需要知道这数据目前是可用的。

>**Separation Of Concerns关注分离**

>Every class should have a single responsibility, and that responsibility should be entirely encapsulated by the class.

>每一个类都应该有单独的职责，并且该职责应完全被这个类封装。（译者注：我认为就是不要让多个类负责同样的职责）

So, it will be beneficial for us to decouple our web layer (controller) from our data access layer completely. This will allow us to migrate storage implementations easily, as well as make our code easier to test. Think of the "Web" as just a transport layer into your "real" application.

关注分离的好处就是能让Web控制器和数据访问解耦。这会使得实现存储迁移更容易，测试也会更容易。“Web”就仅仅是为你真正的应用做数据的传输了。

Imagine that your application is like a monitor with a variety of cable ports. You can access the monitor's functionality via HDMI, VGA, or DVI. Think of the Internet as just a cable into your application. The bulk of a monitor's functionality is independent of the cable. The cable is just a transport mechanism just like HTTP is a transport mechanism for your application. So, we don't want to clutter up our transport mechanism (the controller) with application logic. This will allow any transport layer, such as an API or mobile application, to access our application logic.

想象一下你有一个类似于监视器的程序，有着很多线缆接口（HDMI，VGA，DVI等等）。 你可以通过不同的接口访问不同的监视器。把Internet想象成另一个插进你程序线缆接口。大部分显示器的功能是与线缆接口互相独立的。线缆接口只是一 种传输机制就像HTTP是你程序的一种传输机制一样。所以我们不想把传输机制（控制器）和业务逻辑混在一起。这样的好处是很多其他的传输机制比如API调 用、移动应用等都可以访问我们的业务逻辑。

So, instead of coupling our controller to the Eloquent ORM, let's inject a repository class.

那么我们就别再将控制器和Eloquent ORM耦合在一起了。 咱们注入一个资料库类。

### Build A Contract 建立约定

First, we'll define an interface and a corresponding implementation:

首先我们定义一个接口，然后实现该接口。

```
interface UserRepositoryInterface
{
    public function all();
}
class DbUserRepository implements UserRepositoryInterface
{
    public function all()
    {
        return User::all()->toArray();
    }
}
```
Next, we'll inject an implementation of this interface into our controller:

然后我们将该接口的实现注入我们的控制器。

```
class UserController extends BaseController
{
  public function __construct(UserRepositoryInterface $users)
   {
    $this->users = $users;
  }
  public function getIndex()
 {
   $users=$this->users->all();
   return View::make('users.index', compact('users'));
 }
}

```

Now our controller is completely ignorant of where our user data is being stored. In this case, ignorance is bless! Our data could be coming from MySQL, MongoDB, or Redis. Our controller doesn't know the difference, nor should it care. Just by making this small change, we can test our web layer independent of our data layer, as well as easily switch our storage implementation.

现在我们的控制器就完全和数据层面无关了。在这里无知是福！我们的数据可能来自MySQL，MongoDB或者Redis。我们的控制器不知道也不需要知道他们的区别。仅仅做出了这么小小的改变，我们就可以独立于数据层来测试Web层了，将来切换存储实现也会很容易。

>**Respect Boundaries 严守边界**

>Remember to respect responsibility boundaries. Controllers and routes serve as a mediator between HTTP and your application. When writing large applications, don't clutter them up with your domain logic.

>记得要保持清晰的责任边界。 控制器和路由是作为HTTP和你的应用程序之间的中间件来用的。当编写大型应用程序时，不要将你的领域逻辑混杂在其中（控制器、路由）。

To solidify our understanding, let's write a quick test. First, we'll mock the repository and bind it to the application IoC container. Then, we'll ensure that the controller properly calls the repository:

为了巩固学到的知识，咱们来写一个测试案例。首先，我们要模拟一个资料库然后绑定到应用的IoC容器里。 然后，我们要保证控制器正确的调用了这个资料库：
```
public function testIndexActionBindsUsersFromRepository()
{    
   // Arrange...
   $repository = Mockery::mock('UserRepositoryInterface');
   $repository->shouldReceive('all')->once()->andReturn(array('foo'));
   App::instance('UserRepositoryInterface', $repository);
   // Act...
  $response  = $this->action('GET', 'UserController@getIndex');
  // Assert...
    $this->assertResponseOk();
  $this->assertViewHas('users', array('foo'));
}
```

>**Are you Mocking Me 你在模仿我么？**

>In this example, we used the `Mockery` mocking library. This library provides a clean, expressive interface for mocking your classes. Mockery can be easily insalled via Composer.

>在上面的例子里， 我们使用了名为`Mockery`的模仿库。 这个库提供了一套整洁且富有表达力的方法，用来模仿你写的类。 Mockery可以通过Composer安装。

### Taking It Further 更进一步

Let's consider another example to solidify our understanding. Perhaps we want to notify customers of charges to their account. We'll define two interfaces, or contracts. These contracts will gie us the flexibility to change out their implementations later.

让我们考虑另一个例子来巩固理解。 可能我们想要去提醒用户该交钱了。 我们会定义两个接口， 或者约定。这些约定使我们在更改实际实现时更加灵活。

<!-- lang: php -->
interface BillerInterface {
    public function bill(array $user, $amount);
}
interface BillingNotifierInterface {
    public function notify(array $user, $amount);
}

Next we'll build an implementation of our BillerInterface contract:

接下来我们要写一个BillerInterface的实现：

<!-- lang:php -->
```
class StripeBiller implements BillerInterface{
    public function __construct(BillingNotifierInterface $notifier)
    {
        $this->notifier = $notifier;
    }
    public function bill(array $user, $amount)
    {
        // Bill the user via Stripe...
        $this->notifier->notify($user, $amount);
    }

```

By separating the responsibilities of each class, we're now able to easily inject various notifier implementations into our billing class. For example, we could inject a SmsNotifier or an EmailNotifier. Our biller is no longer concerned with the implementation of notifyingg, but only the contract. As long as a class abides by its contract (interface), the biller will gladly accept it. Furthermore, not only do we get added flexibility, we can now test our biller in isolation from our notifiers by injecting a mock BillingNotifierInterface.

只要遵守了每个类的责任划分，我们很容易将不同的提示器（notifier）注入到账单类里面。 比如，我们可以注入一个SmsNotifier或者EmailNotifier。账单类只要遵守了约定，就不用再考虑如何实现提示功能。只要是遵守约定（接口）的类， 账单类都能用。这不仅仅是方便了我们的开发，而且我们还可以通过模拟BillingNotifierInterface来进行无痛测试。

>Be The Interface 使用接口

>While writing interfaces might seem to a lot of extra work, they can actually make your development more rapid. Use interfaces to mock and test the entire back-end of your application before writing a single line of implementation!

>写接口可能看上去挺麻烦，但实际上能加速你的开发。你不用实现任何接口，就能使用模拟库来模拟你的接口，进而测试整个后台逻辑！

So, how do we *do* dependency injection? It's simple:

那我们如何*做*依赖注入呢？很简单：

<!-- lang:php -->
$biller = new StripeBiller(new SmsNotifier);

That's dependency injection. Instead of the biller being concerned with notifying users, we simply pass it a notifier. A change this simple can do amazing things for your applications. Your code immediately becomes more maintainable since class responsibilities are clearly delineated. Also, testability will skyrocket as you can easily inject mock dependencies to isolate the code under test.

这就是依赖注入。 biller不需再考虑提醒用户的事儿，我们直接传给他一个提示器（notifier）。 这种微小的改动能使你的应用焕然一新。 你的代码马上就变得更容易维护， 因为明确指定了类的职责边界。 并且更容易测试， 你只需使用模拟依赖即可。

But what about IoC containers? Aren't they necessary to do dependency injection? Absolutely not! As we'll see in the following chapters, containers make dependency injection easier to manage, but they are not a requirement. By following the principles in this chapter, you can practice dependency injection in any of your projects, regardless of whether a container is avaliable to you.

那IoC容器呢？ 难道依赖注入不需要IoC容器么？当然不需要！在接下来的章节里面你会了解到，容器使得依赖注入更易于管理，但是容器不是依赖注入所必须的。只要遵循本章提出的原则， 你可以在你任何的项目里面实施依赖注入，而不必管该项目是否使用了容器。

### Too Much Java? 太像Java了？

A common criticism of use of interfaces in PHP is that it makes your code too much like "Java". What people mean is that it makes the code very verbose. You must define an interface and an implementation, which leads to a few extra key-strokes.

有人会说使用接口让PHP代码看上去太像Java了——即代码太罗嗦了——你必须定义接口然后实现它，要多按好多下键盘。

For small, simple applications, this criticism is probably valid. Interfaces are often unnecessary in these applications, and it is "OK" to just couple yourself to an implementation you know won't change. There is no need to use interfaces if you are certain your implementation will not change. Architecture astronauts will tell you that you can "never be certain". But, let's face it, sometimes you are.

对于小而简单的应用来说，以上说法也对。 接口通常是不必要的。将代码耦合到那些你认为不会改变的地方也是可以的。在你确信不会改变的地方就没有必要使用接口了。架构师说“不会改变的地方是不存在的”。不过话说回来，有时候的确不会改。

Interfaces are very helpful in large applications, and the extra key-strokes pale in comparison to the flexibility and testability you will gain. The ability to quickly swap implementations of a contract will "wow" your manager, and allow you to write code that easily adapts to change.

在大型应用中接口是很有帮助的。和提升的代码灵活性、可测试性比起来，多敲键盘费的功夫就微不足道了。当你迅速的切换了代码实现的时候，你的经理一定会被你的神速吓一跳的。你也可以写出更适应变化的代码。

So, in conclusion, keep in mind that this book presents a very "pure" architecture. If you need to scale it back for a small application, don't feel guilty. Remember, we're all trying to "code happy". If you're not enjoying what you're doing or you are programming out of guilt. step back and re-evaluate.

总而言之， 记住本书提倡“简单”架构。如果你在写小程序的时候无法遵守接口原则， 别觉得不好意思。 要记住做码农呢，最重要就是开心。如果你不喜欢写接口，那就先简单的写代码吧。日后再精进即可。

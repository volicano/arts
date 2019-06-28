## ARTS

### Review

# Laravel-从百草园到三味书屋

## 第三章
## Interface As Contract 接口约定
## Strong Typing & Water Fowl 强类型和小鸭子


In the previous chapters, we covered the basics of dependency injection: what it is; how it is accomplished; and several of benefits. The examples in the previous chapters also demonstrated the injection of *interface* into our *classes*, so before proceeding further, it will be beneficial to talk about interfaces in depth, as many PHP developers are not familiay with them.

在之前的章节里，涵盖了依赖注入的基础知识：什么是依赖注入；如何实现依赖注入；依赖注入有什么好处。 之前章节里面的例子也模拟了将*interface*注入到*classes*里面的过程。在我们继续学习之前，有必要深入讲解一下接口，而这正是很多PHP开发者所不熟悉的。

Before I was a PHP programmer, I was a .NET programmer. Do I love pain or what? In .NET, interfaces are everywhere. In fact, many interfaces are defined as part of the core .NET framework itself, and for good reason: many of the .NET languages, such as C# and VB.NET, are *strongly typed*. In general, you must define the *type* of object or primitive you will be passing into a method. For example, consider the following C# method:

在我成为PHP程序员之前，我是写.NET的。 你觉得我是M么？在.NET里可到处都是接口。 事实上很多接口是定义在.NET框架核心中了，一个好的理由是：很多.NET语言比如C#和VB.NET都是*强类型的*。 也就是说，你在给一个函数传值，要么传原生类型对象，要么就必须给这个对象一个明确的*类型*定义。比如考虑以下C#方法：

```
<!-- lang: c# -->
public int BillUser(User user)
{
    this.biller.bill(user.GetId(), this.amount)
}
```

Note that we were forced to define not only what *type* of arguments we will be passing to the method, but also what the method itself will be returning. C# is encouraging *type safety*. We will not be allowed to pass anything other than an User object into out BillUser method

注意在这里， 我们不仅要定义传进去的参数是什么类型的，还要定义这个方法返回值是什么类型的。 C#鼓励类型安全。除了指定的User对象，它不允许我们传递其他类型的对象到BillUser方法中。

However, PHP is generally a *duck typed* language. In a duck typed language, an object's avaliable methods determine the way it may be used, rather than its inheritance from a class or implementation of an interface. Let's look at an example:

然而PHP是一种*鸭子类型*的语言。 所谓鸭子类型的语言， 一个对象可用的方法取决于使用方式， 而非这个方法从哪儿继承或实现。来看个例子：
```
<!-- lang:php -->
public function billUser($user)
{
    $this->biller->bill($user->getId(), $this->amount);
}
```

In PHP, we did not have to tell the method what type of argument to expect. In fact, we can pass any type, so long as the object responds to the getId method. This is an example of duck typing. If the object looks like a duck, quacks like a duck, it must be a duck. Or, in this case, if the object looks like a user, and acts like a user, it must be one.

在PHP里面，我们不必告诉一个方法需要什么类型的参数。 实际上我们传递任何类型的对象都可以，只要这个对象能响应getId的调用。这里有个关于鸭子类型（下文译作：弱类型）的解释：如果一个东西看起来像个鸭子，叫声也像鸭子叫，那他就是个鸭子。 换言之在程序里，一个对象看上去是个User，方法响应也像个User，那他就是个User。

But, does PHP have *any* strongly typed style features? Of course! PHP has a mixture of both strong and duck typed constructs. To illustrate this, let's re-write our billUser method:

不过PHP到底有没有*任何*强类型功能呢？当然有！PHP混合了强类型和弱类型的结构。为了说明这点，咱们来重写一下billUser方法：

```
<!-- lang:php -->
public function billUser(User $user)
{
    $this->biller->bill($user->getId(), $amount);
}
```

After adding the User type-hint to our method signature, we can now gurantee that all objects passed to billUsereither are a User instance, or extend the User class.

给方法加上了加上了User类型提示后， 我们可以确信的说所有传入billUser方法的参数，都是User类或是继承自User类的一个实例。

There are advangates and disadvantages to both typing systems. In strongly typed languages, the compiler can often provide you with through compile-time error checking, which is extremely helpful. Method inputs and outputs are also much more explicit in a strongly typed language.

强类型和弱类型各有优劣。 在强类型语言中， 编译器通常能提供编译时错误检查的功能，这功能可是非常有用的。方法的输入和输出也更加明确。

As the same time, the explicit nature of strong typing can make it rigid. For example, dynamic methods such as whereEmailOrName that the Eloquent ORM offers would be impossible to implement in a strongly typed language like C#. Try to avoid heated discussions on which paradigm is better, and remember the advantages and disadvantages of each. It is not "wrong" to use the strong typing avaliable in PHP, nor is it wrong to use duck typing. What is wrong is exclusively using one or the other without considering the particular problem you are trying to solve.

与此同时，强类型的特性也使得程序僵化。比如Eloquent ORM中，类似whereEmailOrName的动态方法就不可能在C#这样的强类型语言里实现。我们不讨论强类型弱类型哪种更好，而是要记住他们分别的优劣之处。在PHP里面使用强类型标记不是错误，使用弱类型特性也不是错误。但是不加思索，不管实际情况去使用一种模式，这么固执的使用就是错的。

## A Contract Example 约定的范例

Interfaces are contracts. Interfaces do not contain any code, but simply define a set of methods that an object must implement. If an object implements an interface, we are guranteed that every method defined by the interface is valid and callable on that object. Since the contract gurantees the implementation of certain methods, type safety becomes more flexible via *polymorphism*.

接口就是约定。接口不包含任何代码实现，只是定义了一个对象应该实现的一系列方法。如果一个对象实现了一个接口，那么我们就能确信这个接口所定义的一系列方法都能在这个对象上使用。因为有约定保证了特定方法的实现标准，通过*多态*也能使类型安全的语言变得更灵活。

>Poly what？ 多什么肽？

>Polymorphism is a big word that essentially means an entity can have multiple forms. In the context of this book, we mean that an interface can have multiple implementations. For example, a UserRepositoryInterface could have a MySQL and a Redis implementation, and both implementations would qualify as a UserRepositoryInterface instance.

>多态含义很广，其本质上是说一个实体拥有多种形式。在本书中，我们讲多态是一个接口有着多种实现。比如UserRepositoryInterface可以有MySQL和Redis两种实现，每一种实现都是UserRepositoryInterface的一个实例。



To illustrate the flexibility that interfaces introduce into strongly typed languages, let's write some simple code that books hotel rooms. Consider the following interface:

为了说明在强类型语言中接口的灵活性，咱们来写一个酒店客房预订的代码。考虑以下接口：

```
<!-- lang:php -->
interface ProviderInterface{
    public function getLowestPrice($location);
    public function book($location);
}
```

When our user books a room, we'll want to log that in our system, so let's add a few methods to our User class:

当用户订房间时，我们需要将此事记录在系统里。所以在User类里面写点方法：

```
<!-- lang:php -->
class User{
    public function bookLocation(ProviderInterface $provider, $location)
    {
        $amountCharged = $provider->book($location);
        $this->logBookedLocation($location, $amountCharged);
    }
```

Since we are type hinting the ProviderInterface, our User can safely assume that the book method will be available. This gives us the flexibility to re-use our bookLocation method regardless of the hotel provider the user prefers. Finally, let's write some code that harnesses this flexibility:

因为我们写出了ProviderInterface的类型提示，该User类的就可以放心大胆的认为book方法是可以调用的。这使得bookLocation方法有了重用性。当用户想要换一家酒店提供商时也就更灵活。最后咱们来写点代码来强化他的灵活性。

```
<!-- lang:php -->
$location = 'Hilton, Dallas';

$cheapestProvider = $this->findCheapest($location, array(
    new PricelineProvider,
    new OrbitzProvider,
));
```

$user->bookLocation($cheapestProvider, $location);

Wonderful! No matter what provider is the cheapest, we are able to simply pass it along to our User instance for booking. Since our User is simply asking for an object instances that abides by the ProviderInterface contract, our code will continue to work even if we add new provider implementations.

太棒了！不管哪家是最便宜的，我们都能够将他传入User对象来预订房间了。由于User对象只需要要有一个符合ProviderInterface约定的实例就可以预订房间，所以未来有更多的酒店供应商我们的代码也可以很好的工作。

>Forget The Details 忘掉细节

>Remember, interfaces don't actually *do* anything, They simply define a set of methods that an implementing class **must** have.

>记住，接口实际上不真正做任何事情。它只是简单的定义了类们**必须**实现的一系列方法。

## Interfaces & Team Development 接口与团队开发

When you team is building a large application, pieces of the application progress at different speeds. For example, imagine one developer is working on the data layer, while another developer is working on the front-end and web/controller layer. The front-end developer wants to test his controllers, but the back-end is progressing slowly. However, what if the two developers can agree on an interface, or contract, that the back-end classes will abide by, such as following:

当你的团队在开发大型应用时，不同的部分有着不同的开发速度。比如一个开发人员在制作数据层，另一个开发人员在做前端和网站控制器层。前端开发者想测试他的控制器，不过后端开发较慢没法同步测试。那如果两个开发者能以接口的方式达成协议，后台开发的各种类都遵循这种协议，就像这样：

```
<!-- lang:php -->
interface OrderRepositoryInterface {
    public function getMostRecent(User $user);
}
```
Once the contract has been agreed upon, the front-end developer can test his controller, even if no real implementation of the contract has been written! This allows components of the application to be built at different speeds, while still allowing for proper unit tests to be written. Further, this approach allows entire implementations to change without breaking other, unrelated components. Remember, ignorance is bliss. We don't want our classes to know *how* a dependency does something, but only that it *can*. So, now that we have a contract defined, let's write our controller:

一旦建立了约定，就算约定还没实现，前端开发者也可以测试他的控制器了！这样应用中的不同组件就可以按不同的速度开发，并且单元测试也可以做。而且这种处理方法还可以使组件内部的改动不会影响到其他不相关组件。要记着无知是福。我们写的那些类们不用知道别的类*如何*实现的，只要知道它们*能*实现什么。这下咱们有了定义好的约定，再来写控制器：

```
<!-- lang:php -->
class OrderController {
    public function __construct(OrderRepositoryInterface $orders)
    {
        $this->orders = $orders;
    }
    public function getRecent()
    {
        $recent = $this->orders->getMostRecent(Auth::user());
        return View::make('orders.recent', compact('recent'));
    }
}
```

The front-end developer could even write a "dummy" implementation of the interface, so the application's views can be populated with some fake data:

前端开发者甚至可以为这接口写个“假”实现，然后这个应用的视图就可以用假数据填充了：

```
<!-- lang:php -->
class DummyOrderRepository implements OrderRepositoryInterface {
    public function getMostRecent(User $user)
    {
        return array('Order 1', 'Order 2', 'Order 3');
    }
}
```

Once the dummy implementation has been written, it can be bound in the IoC container so it is used throughout the application:

一旦假实现写好了，就可以被绑定到IoC容器里，然后整个程序都可以调用他了：

```
<!-- lang:php -->
App::bind('OrderRepositoryInterface', 'DummyOrderRepository');
```

Then, once a "real" implementaion, such as RedisOrderRepository, has been written by the back-end developer, the IoC binding can simply be switched to the new implementation, and the entire application will begin using orders that are stored in Redis.

接下来一旦后台开发者写完了真正的实现代码，比如叫RedisOrderRepository。那么IoC容器就可以轻易的切换到真正的实现上。整个应用就会使用从Redis读出来的数据。

>Interface As Schematic 接口就是大纲

>Interfaces are helpful for developing a "skeleton" of defined functionality provided by your application. Use them during the design phase of a component to facilicate design discussion amongst your team. For example, define a BillingNotifierInterface and discuss its methods with your team. Use the interface to agree on a good API before you actually write any implementation code!

>接口在开发程序的“骨架”时非常有用。 在设计组件时，使用接口进行设计和讨论都是对你的团队有益处的。比如定义一个BillingNotifierInterface然后讨论他有什么方法。在写任何实现代码前先用接口讨论好一套好的API！



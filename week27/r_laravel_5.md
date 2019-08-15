## ARTS

### Review

# Laravel-从百草园到三味书屋

## 第五章
## Application Structure 应用结构
## Introduction 介绍


Where does this class belong? This question is extremely common when building applications on a framework. Many developers ask this question because they have been told that "Model" means "Database". So, developers have their controllers that interact with HTTP, models which do *something* with the database, and views which contain their HTML. But, what about classes that send e-mail? What about classes that validate data? What about classes that call an API to gather information? In this chapter, we'll cover good application structure in the Laravel framework and break down some of the common mental roadblocks that hold developers back from good design.

这个类要写到哪儿？这是一个在用框架写应用程序时十分常见的问题。大量的开发人员都有这个疑问。他们被灌输“Model”就是“Database”，在控制器里面处理HTTP请求，在模型里操作数据库，视图里包含了要显示的HTML。不过，发送电子邮件的类要写到哪儿？数据验证的类要写到哪儿？调用外部API的类要写到哪儿？在这一章节，我们将学习如何写结构优美的Laravel应用，打破长久以来掣肘开发人员的普遍思维惯性这个拦路虎，最终做出好的设计。

## MVC Is Killing You MVC是慢性谋杀

The biggest roadblock towards developers achieving good design is a simple acronym: M-V-C. Models, views, and controllers have dominated web framework thinking for years, in part because of the popularity of Ruby on Rails. However, ask a developer to define "model". Usually, you'll hear a few mutters and the word "database". Supposedly, the model *is* the database. It's where all your database *stuff* goes, whatever that means. But, as you quickly learn, your application needs a lot more logic than just a simple database access class. It needs to do validation, call external services, send e-mails, and more.

为了做出好的程序设计，最大的拦路虎就是一个简单的缩写词：M-V-C。模型、视图、控制器主宰了Web框架的思想已经好多年了。这种思想的流行某种程度上是托了Ruby on Rails愈加流行的福。然而，如果你问一个开发人员“模型”的定义是什么。通常你会听到他嘟哝着什么“数据库”之类的东西。这么说，模型*就是*数据库了。不管这意味着什么，模型里包含了关于数据库的*一切*。但是，你很快就会知道，你的应用程序需要的不仅仅是一个简单的数据库访问类。他需要更多的逻辑如：数据验证、调用外部服务、发送电子邮件，等等更多。

>What Is A Model? 模型是啥？

>The word "model" has become so ambiguous that it has no meaning. By developing with a more specific vocabulary, it will be easier to separate our application into smaller, cleaner classes with a clearly defined responsiblity.

>单词"model"的含义太模糊了，很难说明白准确的含义。更具体来讲，模型是用来将我们的应用划分成更小、更清晰的类，使得各代码部分有着明确的权责。

So, what is the solution to this dilemma? Many developers start packing logic into their controllers. Once the controllers get large enough, they need to re-use business logic that is in other controllers. Instead of extracting the logic into another class, most developers mistakenly assume they *need* to call controllers from within other controllers. This pattern is typically called "HMVC". Unfortunately, this pattern often indicates poor application design, and controllers that are much too complicated.

所以怎么解决这个问题（译者注：上文中“更多的业务逻辑”）呢？很多开发者开始将业务逻辑包装到控制器里面。当控制器庞大到一定规模，他们将会需要重用业务逻辑。大部分开发人员没有将这些业务逻辑提取到别的类里面，而是错误的臆想他们*需要*在控制器里面调用别的控制器。这种模式通常被称为“HMVC”。不幸的是，这种模式通常也预示着糟糕的程序设计，并且控制器已经太复杂了。

>HMVC (Usually) Indicates Poor Design

>HMVC（通常）预示着糟糕的设计。

>Feel the need to call controllers from other controllers? This is often indicative of poor application design and too much business logic in your controllers. Extract the logic into a third class that can be injected into any controller.

>你觉得需要在控制器里面调用其他的控制器？这通常预示着糟糕的程序设计并且你的控制器里面业务逻辑太多了。把业务逻辑抽出来放到一个新的类里面，这样你就可以在其他任何控制器里面调用了。

There is a better way to structure applications. We need to wash our minds clean of all we have been taught about *models*. In fact, let's just delete the model directory and start fresh!

有一种更好的程序结构。但首先我们要忘掉以往我们被灌输的关于“模型”的一切。干脆点，让我们直接删掉model目录，重新开始吧！

## Bye, Bye Models 再见，模型

Is your models directory deleted yet? If not, get it out of there! Let's create a folder within our app directory that is simply named after our application. For this discussion, let's call our application QuickBill, and we'll continue to use some of the interfaces and classes we've discussed before.

删掉你的models目录了么？还没删就赶紧删了！我们将要在app目录下创建个新的目录，目录名就以我们这个应用的名字来命名，这次我们就叫QuickBill吧。在后续的讨论中，我们在前面写的那些接口和类都会出现。

>Remember The Context 注意使用场景

>Remember, if you are building a very small Laravel application, throwing a few Eloquent models in the modelsdirectory perfectly fine. In this chapter, we're primarily concerned with discovering more "layered" architecture suitable to large and complex projects.

>记住，如果你在写一个很小的Laravel应用，那在models目录下写几个Eloquent模型其实挺合适的。但在本章节，我们主要关注如何开发更有合适“层次”架构的大型复杂项目。

So, we should have an app/QuickBill directory, which is at the same level in the application directory structure as controllers and views. We can create several more directories within QuickBill. Let's create a Repositoriesdirectory and a Billing directory. Once these directories are established, remember to register them for PSR-0 auto-loading in your composer.json file:

这样我们现在有了个app/QuickBill目录，它和应用目录下的其他目录如controllers还有views都是平级的。在QuickBill目录下我们还可以创建几个其他的目录。我们来在里面创建个Repositories和Billing目录。目录都创建好以后，别忘了在composer.json文件里加入 PSR-0 的自动载入机制：

```
<!-- lang:javascript -->
"autoload": {
    "psr-0":    {
        "QuickBill":    "app/"
    }
}
```

>译者注：psr-0 也可以改成 psr-4， "psr-4": { "QuickBill\": "app/QuickBill" } psr-4 是比较新的建议标准，和 psr-0 具体有什么区别请自行检索。

For now, let's put our Eloquent classes at the root of the QuickBill directory. This will allow us to conveniently access them as QuickBill\User, QuickBill\Payment, etc. In the Repositories folder would belongs classes such as PaymentRepository and UserRepository, which would contain all of our data access functions such as getRecentPayments, and getRichestUser. The Billing directory would contain the classes and interfaces that work with third-party billing services like Stripe and Balanced. The folder structure would look something like this:

现在我们把继承自 Eloquent 的模型类都放到QuickBill目录下面。这样我们就能很方便的以QuickBill\User, QuickBill\Payment的方式来使用它们。Repositories目录属于PaymentRepository 和UserRepository这种类，里面包含了所有对数据的访问功能比如getRecentPayments和getRichestUser。Billing目录应当包含调用第三方支付服务（如Stripe和Balanced）的类。整个目录结构应该类似这样：

```
<!-- lang:php -->
// app
    // QuickBill
        // Repositories
            -> UserRepository.php
            -> PaymentRepository.php
        // Billing
            -> BillerInterface.php
            -> StripeBiller.php
        // Notifications
            -> BillingNotifierInterface.php
            -> SmsBillingNotifier.php
        User.php
        Payment.php
```

>What About Validation 数据验证怎么办？

>Where to perform validation often stumps developers. Consider placing validation methods on your "entity" classes (like User.php and Payment.php). Possible method name might be: validForCreation or hasValidDomain. Alternatively, you could create a UserValidator class within a Validation namespace and inject that validator into your repository. Experiment with both approaches and see what you like best!

>在哪儿进行数据验证常常困扰着开发人员。可以考虑将数据验证方法写进你的“实体”类里面（好比User.php和Payment.php）。方法名可以设为validForCreation或hasValidDomain。或者你也可以专门创建个验证器类UserValidator，放到Validation命名空间下，然后将这个验证器类注入到你的repository类里面。两种方式你都可以试试，看哪个你更喜欢！


By just getting rid of the models directory, you can often break down mental roadblocks to good design, allowing you to create a directory structure that is more suitable for your application. Of course, each application you build will have some similarities, since each complex application will need a data access (repository) layer, several external service layers, etc.

摆脱了models目录后，你通常就能克服心理障碍，实现好的设计。使得你能创建一个更合适的目录结构来为你的应用服务。当然，你建立的每一个应用程序都会有一定的相似之处，因为每个复杂的应用程序都需要一个数据访问（repository）层，一些外部服务层等等。

>Don't Fear Directories 别害怕目录

>Don't be afraid to create more directories to organize your application. Always break your application into small components, each having a very focused responsibility. Thinking outside of the "model" box will help. For example, as we previously discussed, you could create a Repositories directory to hold all of your data access classes.

>不要惧怕建立目录来管理应用。要常常将你的应用切割成小组件，每一个组件都要有十分专注的职责。跳出“模型”的框框来思考。比如我们之前就说过，你可以创建个Repositories目录来存放你所有的数据访问类。


## It's All About The Layers 核心思想就是分层

As you may have noticed, a key to solid application design is simply separating responsibilities, or creating layers of responsibility. Controllers are responsible for receiving an HTTP request and calling the proper business layer classes. Your business / domain layer *is* your application. It contains the classes that retrieve data, validate data, process payments, send e-mail, and any other function of your application. In fact, your domain layer doesn't need to know about "the web" at all! The web is simply a transport mechanism to access your application, and knowledge of the web and HTTP need not go beyond the routing and controller layers. Good architecture can be challenging, but will yield large profits of sustainable, clear code.

你可能注意到，优化应用的设计结构的关键就是责任划分，或者说是创建不同的责任层次。控制器只负责接收和响应HTTP请求然后调用合适的业务逻辑层的类。你的业务逻辑/领域逻辑层*才是*你真正的程序。你的程序包含了读取数据，验证数据，执行支付，发送电子邮件，还有你程序里任何其他的功能。事实上你的领域逻辑层不需要知道任何关于“网络”的事情！网络仅仅是个访问你程序的传输机制，关于网络和HTTP请求的一切不应该超出路由和控制器层。做出好的设计的确很有挑战性，但好的设计也会带来可持续发展的清晰的好代码。

For example, instead of accessing the web request instance in a class, you could simply pass the web input from the controller. This simple change alone decouples your class from "the web", and the class can easily be tested without worrying about mocking a web request:

举个例子。与其在你业务逻辑类里面直接获取网络请求，不如你直接把网络请求从控制器传给你的业务逻辑类。这个简单的改动将你的业务逻辑类和“网络”分离开了，并且不必担心怎么去模拟网络请求，你的业务逻辑类就可以简单的测试了：

```
<!-- lang:php -->
class BillingController extends BaseController{
    public function __construct(BillerInterface $biller)
    {
        $this->biller = $biller;
    }
    public function postCharge()
    {
        $this->biller->chargeAccount(Auth::user(), Input::get('amount'));
        return View::make('charge.success');
    }
}
```

Our chargeAccount method is now much easier to test, since we no longer have to use the Request or Input class inside of our BillingInterface implementation as we are simply passing the charged amount into the method as an integer.

现在chargeAccount 方法更容易测试了。 我们把Request和Input从BillingInterface里提出来，然后在控制器里把方法需要的支付金额直接传过去。

Separation of responsibilities is one of the keys to writing maintainable applications. Always be asking if a given class knows more than it should. You should frequently ask yourself: "Should this class care about X?" If answer is "no", extract the logic into another class that can be injected as a dependency.

编写拥有高可维护性应用程序的关键之一，就是责任分割。要时常检查一个类是否管得太宽。你要常常问自己“这个类需不需要关心XXX呢？”如果答案是否定的，那么把这块逻辑抽出来放到另一个类里面，然后用依赖注入的方式进行处理。（译者注：依赖注入的不同方式还记得么？调用方法传参、构造函数传参、从IoC容器获取等等。）


>Single Reason To Change

>A helpful method of determining whether a class has too many responsibilities is to examine your reason for changing code within that class. For example, should we need to change code within a Biller implementation when tweaking our notification logic? Of course not. The Biller implementations are concerned with billing, and should only work with notification logic via a contract. Maintaining this mindset as you are working on your code will help you quickly identify areas of an application that can be improved.

>如何判断一个类是否管得太宽，有一个有用的方法就是检查你为什么要改这块儿代码。举个例子：当我们想调整通知逻辑的时候，我们需要修改Biller的实现代码么？当然不需要，Biller的实现仅仅需要考虑支付，它与通知逻辑应当仅通过约定来进行交互。使用这种思路过一遍代码，会让你很快找出应用中需要改进的地方。


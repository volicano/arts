## ARTS

### Review

# Laravel-从百草园到三味书屋

第六章
## Applied Architecture: Decoupling Handlers 实用做法：解耦处理函数
### Introduction 介绍

Now that we have discussed various aspects of sound application architecture using Laravel 4, Let's dig into some more specifics. In this chapter, we'll discuss tips for decoupling various handlers like queue and event handlers, as well as other "event-like" structures such as route filters.

我们已经讨论了用 Laravel4 制作优美的程序架构的各个方面，让我们再深入一些细节。在本章，我们将讨论如何解耦各种处理函数：队列处理函数、事件处理函数，甚至其他“事件型”的结构如路由过滤器。

>Don't Clog Your Transport Layer 不要堵塞传输层

>Most "handlers" can be considered *transport layer* components. In other words, they receive calls through something like queue workers, a dispatched event, or an incoming request. Treat these handlers like controllers, and avoid clogging them up with the implementation details of your application.

>大部分的“处理函数”可以被当作*传输层*组件。也就是说，队列触发器、被触发的事件、或者外部发来的请求等都可能调用处理函数。可以把处理函数理解为控制器，避免在里面堆积太多具体业务逻辑实现。

## Decoupling Handlers 解耦处理函数

To get started, let's jump right into an example. Consider a queue handler that sends an SMS message to a user. After sending the message, the handler logs that message so we can keep a history of all SMS messages we have sent to that user. The code might look something like this:

接下来我们看一个例子。考虑有一个队列处理函数用来给用户发送手机短信。信息发送后，处理函数还要记录消息日志来保存给用户发送的消息历史。代码应该看起来是这样：

```
<!-- lang:php -->
class SendSMS{
    public function fire($job, $data)
    {
        $twilio = new Twilio_SMS($apiKey);
        $twilio->sendTextMessage(array(
            'to'=> $data['user']['phone_number'],
            'message'=> $data['message'],
        ));
        $user = User::find($data['user']['id']);
        $user->messages()->create(array(
            'to'=> $data['user']['phone_number'],
            'message'=> $data['message'],
        ));
        $job->delete();
    }
}
```

Just by examining this class, you can probably spot several problems. First, it is hard to test. The Twilio_SMS class is instantiated inside of the *fire* method, meaning we will not be able to inject a mock service. Secondly, we are using Eloquent directly in the handler, thus creating a second testing problem as we will have to hit a real database to test this code. Finally, we are unable to send SMS messages outside of the queue. All of our SMS sending logic is tightly coupled to the Laravel queue.

简单审查下这个类，你可能会发现一些问题。首先，它难以测试。在fire方法里直接使用了Twilio_SMS类，意味着我们没法注入一个模拟的服务（译者注：即一旦测试则必须发送一条真实的短信）。第二，我们直接使用了Eloquent，导致在测试时肯定会对数据库造成影响。第三，我们没法在队列外面发送短信，想在队列外面发还要重写一遍代码。也就是说我们的短信发送逻辑和Laravel的队列耦合太多了。

By extracting this logic into a separate "service" class, we can decouple our application's SMS sending logic from Laravel's queue. This will allow us to send SMS messages from anywhere in our application. While we are decoupling this process from the queue, we will also refactor it to be more testable.

将里面的逻辑抽出成为一个单独的“服务”类，我们即可将短信发送逻辑和Laravel的队列解耦。这样我们就可以在应用的任何位置发送短信了。我们将其解耦的过程，也令其变得更易于测试。

So, let's examine an alternative:

那么我们来稍微改一改：

```
<!-- lang:php -->
class User extends Eloquent {
    /**
     * Send the User an SMS message
     *
     * [@param](https://my.oschina.net/u/2303379) SmsCourierInterface $courier
     * [@param](https://my.oschina.net/u/2303379) string $message
     * [@return](https://my.oschina.net/u/556800) SmsMessage
     */
    public function sendSmsMessage(SmsCourierInterface $courier, $message)
    {
        $courier->sendMessage($this->phone_number, $message);
        return $this->sms()->create(array(
            'to'=> $this->phone_number,
            'message'=> $message,
        ));
    }
}
```

In this refactored example, we have extracted the SMS sending logic into the User model. We are also injecting a SmsCourierInterface implementation into the method, allowing us to better test that aspect of the process. Now that we have refactored this logic, let's re-write our queue handler:

在本重构的例子中，我们将短信发送逻辑抽出到User模型里。同时我们将SmsCourierInterface的实现注入到该方法里，这样我们可以更容易对该方法进行测试。现在我们已经重构了短信发送逻辑，让我们再重写队列处理函数：

```
<!-- lang:php -->
class SendSMS {
    public function __construct(UserRepository $users, SmsCourierInterface $courier)
    {
        $this->users = $users;
        $this->courier = $courier;
    }
    public function fire($job, $data)
    {
        $user = $this->users->find($data['user']['id']);
        $user->sendSmsMessage($this->courier, $data['message']);
        $job->delete();
    }
}
```

As you can see in this refactored example, our queue handler is now much lighter. It essentially serves as a *translation layer* between the queue and your *real* application logic. That is great! It means that we can easily send SMS message s outside of the queue context. Finally, let's write some tests for our SMS sending logic:

你可以看到我们重构了代码，使得队列处理函数更轻量化了。它本质上变成了队列系统和你*真正*的业务逻辑之间的*转换层*。这可是很了不起！这意味着我们可以很轻松的脱离队列系统来发送短信息。最后，让我们为短信发送逻辑写一些测试代码：

```
<!-- lang:php -->
class SmsTest extends PHPUnit_Framework_TestCase {
    public function testUserCanBeSentSmsMessages()
    {
        /**
         * Arrage ...
         */
        $user = Mockery::mock('User[sms]');
        $relation = Mockery::mock('StdClass');
        $courier = Mockery::mock('SmsCourierInterface');
    
        $user->shouldReceive('sms')->once()->andReturn($relation);

        $relation->shouldReceive('create')->once()->with(array(
            'to' => '555-555-5555',
            'message' => 'Test',
        ));

        $courier->shouldReceive('sendMessage')->once()->with(
            '555-555-5555', 'Test'
        );

        /**
         * Act ...
         */
        $user->sms_number = '555-555-5555'; //译者注： 应当为 phone_number
        $user->sendMessage($courier, 'Test');
    }
}
```

## Other Handlers 其他处理函数

We can improve many other types of "handlers" using this same approach to decoupling. By restricting all handlers to being simple *translation layers*, you can keep your heavy business logic neatly organized and decoupled from the rest of the framework. To drive the point home further, let's examine a route filter that verifies that the current user of our application is subscribed to our "premium" pricing tier.

使用类似的方式，我们可以改进和解耦很多其他类型的“处理函数”。将这些处理函数限制在*转换层*的状态，你可以将你庞大的业务逻辑和框架解耦，并保持整洁的代码结构。为了巩固这种思想，我们来看看一个路由过滤器。该过滤器用来验证当前用户是否是交过钱的*高级*用户套餐。

```
<!-- lang:php -->
Route::filter('premium', function()
{
    return Auth::user() && Auth::user()->plan == 'premium';
});
```

On first glance, this route filter looks very innocent. What could possibly be wrong with a filter that is so small? However, even in this small filter, we are leaking implementation details of our application into the code. Notice that we are manually checking the value of the plan variable. We have tightly coupled the representation of "plans" in our business layer into our routing / transport layer. Now, if we change how the "premium" plan is represented in our database or user model, we will need to change this route filter!

猛一看这路由过滤器没什么问题啊。这么简单的过滤器能有什么错误？然而就是是这么小的过滤器，我们却将我们应用实现的细节暴露了出来。要注意我们在该过滤器里是写明了要检查plan变量。这使得将“套餐方案”在我们应用中的代表值（译者注：即plan变量的值）暴露在了路由/传输层里面。现在我们若想调整“高级套餐”在数据库或用户模型的代表值，我们竟然就需要改这个路由过滤器！

Instead, let's make a very simple change:

让我们简单改一点儿：

```
<!-- lang:php -->
Route::filter('premium', function()
{
    return Auth::user() && Auth::user()->isPremium();
});
```

A small change like this has great benefits and very little cost. By deferring the determination of whether a user is on the premium plan to the model, we have removed all implementation details from our route filter. Our filter is no longer responsible for knowing how to determine if a user is on the premium plan. Instead, it simply asks the User model. Now, if the representation of premium plans changes in the database, there is no need to update the route filter!

小小的改变就带来巨大的效果，并且代价也很小。我们将判断用户是否使用高级套餐的逻辑放在了用户模型里，这样就从路由过滤器里去掉了对套餐判断的实现细节。我们的过滤器不再需要知道具体怎么判断用户是不是高级套餐了，它只要简单的把这个问题交给用户模型。现在如果我们想调整高级套餐在数据库里的细节，也不必再去改动路由过滤器了！

>Who Is Responsible? 谁负责？

>Again we find ourselves exploring the concept of *responsibility*. Remember, always be considering a class' responsibility and knowledge. Avoid making your transport layer, such as handler, responsible for knowledge about your application and business logic.

>在这里我们又一次讨论了*责任*的概念。记住，始终保持一个类应该有什么样的责任，应该知道什么。避免在处理函数这种传输层直接编写太多你应用的业务逻辑。


译者注：本文多次出现*transport layer*, *translation layer*，分别译作传输层和转换层。其实他们应当指代的同一种东西。



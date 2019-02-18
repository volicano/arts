## ARTS

##### Review

### Passwordless SMS Authentication: The Basics
    
I’ve been desiring to implement passwordless SMS authentication in my own apps for a long time now. I’m always a little bit desirous of other apps that simply ask for your mobile phone number, you receive a one-time passcode, and you are authenticated. Bam! No username, password, email, or “remember me” checkboxes. Of all the authentication methods mentioned in my previous articles for [Facebook Login](https://itnext.io/facebook-login-using-aws-amplify-and-amazon-cognito-4acf74875a04), [Google Sign-In](https://itnext.io/google-sign-in-using-aws-amplify-and-amazon-cognito-69cc3bf219ad), and [Login with Amazon](https://itnext.io/add-login-with-amazon-to-your-ios-swift-app-d1b07655d31a), **this approach is by far the simplest way to onboard your mobile users**. So, why don’t you see this option in more apps? Every mobile device has a phone number, right? Is it difficult to implement as a developer? Is it too costly and can this scale globally to millions of users? Are the users comfortable providing their phone number? And, the BIG question…, is this secure? Let’s dive in to answer these questions and then in my next article, I’ll propose a solution for passwordless SMS authentication for iOS using Amazon Cognito and Amazon Pinpoint.

First, let’s go over the **three factors of authentication** so it’s clear when we attempt to authenticate a user via SMS. Even with the advancements of Touch and Face ID, the core three factors of authentication remain.

### Three factors of authentication

1. Something you know — e.g. password, keycode, or pin code


2. Something you have or possess — e.g. cell phone, YubiKey, or even a physical key


3. Something you are (think Touch ID, Face ID)


Up until now, my previous articles covering Facebook, Google, and Login with Amazon, have all focused on #1, what the user knows. In those scenarios, the user knew their social login credentials, or simply linked to a previously authenticated session. Also, worth mentioning here is MFA (multi-factor authentication) or even 3FA (three-factor authentication). When you hear about these factors being implemented in mobile and web applications, these are just more secure ways of authenticating a user as they are using **two factors of authentication for MFA** and all **three factors for 3FA**. More factors involved means less chance that all forms can be faked or impersonated but more difficult to implement as a developer, not to mention, cumbersome for the user.

Now that we are familiar with all three factors of authentication, you’ll see that passwordless SMS authentication will fall into #2 (something the user has or possess) as the phone displaying the one-time SMS passcode is something the user has possession of. Note: The SMS capable phone does not have to be the same device that has your app installed.

So, let’s recap for a minute on this subject and then we’ll move on. Adding basic auth (username/password) to your app is applying #1 for something the user knows and is just one-factor authentication. For SMS passwordless, we are also applying one-factor authentication but using factor #2, something the user possesses, such as a cell phone. If we were to apply both basic auth AND an SMS one-time passcode, this would be considered MFA because we are applying two or more factors of authentication. Hopefully, I’m not too redundant but it wasn’t crystal clear to me at first, so I want to make sure you run this and apply these concepts to any application in the future.

### Why passwordless SMS authentication?

Providing passwordless SMS authentication in your app is the **simplest way to onboard a user**. It’s **convenient**, **secure** (see below), and **easy to implement**. It removes the barrier for your users, much like adding Facebook and Google federated identities eases the basic authentication because the user doesn’t have to create additional creds. Even with a plethora of password managers like [1Password](https://1password.com/) and [LastPass](https://www.lastpass.com/), users still don’t want to set up another user/pass combo, and most of the time, they can’t even remember what username they used. Or, even worse, they use the same username/password for all their accounts.

### How does passwordless SMS work?

Passwordless SMS authentication is just another factor of authentication against a secure platform in possession of the user, in this case, the user’s cell phone. When a mobile user authenticates in your app for the first time, the user is prompted for their phone number and in return, they receive a one-time passcode from the developer’s server-side app on the physical device in the form of an SMS text message. The user enters the SMS passcode into the app and they are authenticated. If this is the first time (sign up), an account is created for the user, if not, they are authenticated and will use the existing unique identity ID provided by the identity provider. In my next article, we’ll be using Amazon Cognito User Pools as the identity provider and sending SMS via Amazon Pinpoint. For this method to work, the user must provide a correct phone number capable of receiving SMS messages AND have physical access to the device receiving the SMS passcode.

### Here’s what a passwordless SMS authentication flow looks like:

![图片](https://cdn-images-1.medium.com/max/1600/1*oCoFBqjpDrZvYlMadiYjqA.gif)

### Is passwordless SMS secure?

Yes. Compared to basic auth, most users provide very weak passwords and often re-use the same password across applications. A combination of these practices increases the risk of someone or some application guessing a user’s credentials and impersonating them. The only obvious risk here is if someone gains access to the physical phone and bypasses the phone’s security to read SMS messages.

### If passwordless SMS is so great, why do we see little adoption from developers?

There are several reasons why developers may not adopt passwordless SMS into their app. One main reason is infrastructure. I mentioned earlier that this is easy to implement, and it is, but you need to use a cloud provider or 3rd party service to manage the SMS with carriers. [Auth0](https://auth0.com/), [Twilio](https://www.twilio.com/), [okta](https://www.okta.com/), and [Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) are just a few services that make this solution secure and easy to get started.

One other reason for lower adoption is the additional cost of SMS, especially global SMS and variable pricing. Even though these are one-time passcodes and not MFA (a passcode sent every time a user attempts to access the app), the cost per SMS message is more expensive than the FREE options of federating Facebook and Google.

One last barrier to SMS passwordless adoption is your app will most likely be running on mobile devices that do not have a phone number or cellular plan such as an iPad wifi edition. In this case, the user would need to have their cell phone nearby when they wanted to authenticate on a non- cellular device. Again, most of the time this is not a problem, however, in my family, my son, has a wifi iPad and no mobile SMS capable phone. So, in this case, the developer would need to offer additional authentication factors such as a basic username/password. Not too radical but does add an additional barrier and this is why most mobile developers will offer several ways for users to authenticate.

### Final Thoughts

Passwordless SMS authentication for your app can improve user experience, and get users quickly authenticated and using your app. There is a considerable amount of drop off of users during the first critical days after installing your app. Getting the user in and using the app needs to be seamless. Simply offering to send a one-time passcode via SMS can be that one killer feature that can dramatically increase user retention. I can’t wait to show you the implementation details and sample iOS app in my next article!

### References:

How Passwordless SMS Authentication Can Improve Your App:


[https://auth0.com/blog/how-passwordless-sms-authentication-can-improve-your-app/](https://auth0.com/blog/how-passwordless-sms-authentication-can-improve-your-app/)

Three-Factor Authentication by Gemalto:

[https://blog.gemalto.com/security/2011/09/05/three-factor-authentication-something-you-know-something-you-have-something-you-are/](https://blog.gemalto.com/security/2011/09/05/three-factor-authentication-something-you-know-something-you-have-something-you-are/)



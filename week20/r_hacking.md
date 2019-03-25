## ARTS

### Share

## “攻击”麻省理工技术评论网站获得无限制访问

DISCLAIMER: All data and information provided in this article are for informational purposes only. The main goal is to increase security awareness, teach about information security, countermeasures and give readers information on how to implement a safe and functional system (in our case, a website). If you plan to use the information for illegal purposes, please leave this website now. I cannot be held responsible for any misuse of the given information. I do not use this method to avoid paying and neither should you.

The word “hacking” usually has a strong meaning and most people would think of gaining root access to the website I am referring to in the title or insight into its database. In this case, however, I want to demonstrate how you can exploit a specific error in its design and **gain access to a feature that was initially blocked**.

Starting from the beginning, I enjoy reading scientific articles on the [www.technologyreview.com](http://www.technologyreview.com/) website. There is however a certain aspect that bothered me. **You can only read three free articles per month**, for more (along with other perks) you need to subscribe to a paid for “all-access digital program”.

![图片](https://uploader.shimo.im/f/NMZ6gm25uUsmrUNn.png!thumbnail)

Every time I read a story, a banner would appear telling me that I have read 1/3, 2/3 or 3/3 of the stories I was entitled to for that month and when I tried to open a new article, after I’d read 3 stories in a month, it prompted me to subscribe in order to have access to it. So I started thinking of how this functionality worked and how it could be bypassed. One could circumvent that perhaps by **making a lot of accounts **(or **constantly chan****gi****n****g**** I****P**** a****d****dres****ses**)** **and reading three articles from each one, but** I don’t consider that to be a reasonable solution.**

I started wondering how the stories I was reading were being counted. My **original hypothesis **was that *every time I clicked on a URL (I was signed in to my account), a request was made to the server, the number of articles I had read in a month was checked for being less than 3, t**he article ID was kept (so I could revisit that article) an**d then I would get the corresponding response from the server (the article itself or a message that I needed** to subscribe for more).*

![图片](https://uploader.shimo.im/f/jSd4bWsKoZIWxwYZ.png!thumbnail)



This seemed pretty logical, but I decided to **take a look at the source code (Ctrl-U) **as well. I started thinking like a programmer and with my beloved **Ctrl-F** searched for certain variable names and bingo! **Here is what I found with some reverse engineering logic.**

* First I spotted the following snippet of HTML code that indicated the message that I was to be shown according to the **number ****of articles I’d read**:

```
<!-- Reading Meter -->
<div class="meter">
<span show="meter.loggedIn.1">
<span class="meter__line"> You've read
<span text="meter.usageWord"></span> of three free articles this month.
</span>
```
* After that, I searched for the word “meter” in the source code, since it is everywhere in the above snippet. What I came up with was a script assigning a JSON object type to a variable named “serverData” (!!!). Here is what a part of it looked like:

```
<script> serverData = "user":{"id":1111111,"username":"John D","email":"myemail@gmail.com","status":"active","rss_token":"st45pew5rX2UQmFafadsgajtP92a","livefyre_id":"uid_11111111","notifications_unread":0,"profile":{"status":"public","display_name_from":"username","name":"John D","url":"/profile/john-d/","first_name":"John","last_name":"D"},"meter":{"month":"2018-01","count":1,"ids":[600000],"hits":[{"id":600000,"date":"2018-01-01T15:11:32-04:00"}]},"email_notifications":....
</script>
```
* Below that, there were some more scripts with a link to JavaScript files, so I started opening them too:

```
<script src="https://cdn.technologyreview.com/js/example.js?v=22222222" defer></script>
```
* From searching, in what seemed to be a lot of lines of JavaScript, I managed to isolate some “revealing” lines of code (again using Ctrl-F and searching for keywords). Here are some of my findings:

```
function P(e,t){e.func.call(e.context,t,e.count++)}
function w(){if(!y())return!1;o.meter={usage:i.length>h.allowed?h.allowed:i.length,allowed:h.allowed,allowedMax:h.allowedMax,...,loggedIn:{1:!1,2:!1,3:!1,"3b":!1},loggedOut:{"1a":!1,"1b":!1}},o.meter.loggedIn[o.meter.usage.toString()]=!0,null==e&&(b=11==(S=new Date).getMonth()...t(window).trigger("mittr:meterBlock",[{reason:"User is above the allowed view limit",...{if(o.meter={usage:e.ids.length>h.allowed?h.allowed:e.ids.length,allowed:h.allowed,allowedMax:h.allowedMax,visible:!0,open:!0,paywall:!1,usageWord:"",allowedWord:"",loggedIn:{1:!1,2:!1,3:!1,"3b":!1},....loggedOut:e.ids.length+1>o.meter.allowedMax?]}
```

**It was obvious that bot****h the increment of the counter ****a****nd the inspection of whether the read articles were less tha****n ****three, were made at**** client-side in JavaScript!! (it actually turned out yo****u didn’t even need t****o be logged in)**

The way someone could overcome the 3 articles per month restriction was now in front of me. **I just had to block JavaScript on the website!**

![图片](https://uploader.shimo.im/f/1gQHDl8eI3UEmoqE.png!thumbnail)

*It may be possibl**e to bene**fit** **fr**o**m **e**ven more mist**a**kes in the** **we**b**site's desi**g**n from this k**in**d of **se**rve**r-s**ide valid**a**tion, but after** disco**vering this basic flaw**,** I sent an** **email to **MIT Techn**ology Review **r**eporting my** **findings.*

Summing up, there are **t****wo**** bas****ic**** mis****ta****ke****s ****in t****his ****imple****m****entation:**

**a****)**** T****h****e v****alid****atio****n ****of th****e n****umber o****f**** articles that the user has read in one m****on****th is**** ****ma****de at**** cli****e****nt-****s****ide, so by**** ****disabling JS, ****o****ne has unl****im****ited on****l****ine acc****e****ss.**

**b) Th****e ****counter**** ****is incremen****te****d at client****-s****ide, s****o by disabling JS, the number of r****ead articles remai****n****s the same. ****So eve****n if the validation was ma****d****e at serve****r**** side we coul****d still h****ave access to**** ****more than t****h****ree articles.**

*I also ha**ve to** mentio**n th**at**** by bloc******king J******a******vaScript**** ****n******ew c******ookies were not ******crea******ted****. If you **d**ele**te** the **ol**d one**s **the**re** is **no **way t**o** access data co**n**cerning the user’s access, with the cu**r**rent** implementation at least. So another way to bypass the paywall is to delete or block cookies.*



Someone might think that this kind of vulnerability is not that serious, but that is not quite true in my opinion.

First of all, it shows **a lack of basic “best p****ractices”** and indicates that **more mistakes may be found in**** the webs****ite's implementation.**

Another important aspect is **the feature’s importance**. In this case, for example, to gain access to the feature of *“unlimited online access to articles”***you need to purchase a subscription**. Depending on the website’s **business plan** this might be an **important source of revenue** (that is acquired by offering unlimited online access as a service).



>*The lesson to be taug**ht here, as simple and **fundamen**tal as it might sound to people with a lot of experience on web **developing, is:**** avoi******d blocking only by client-side!***



*A great hacking challenge that also taught me this, is ****“HackGame3, by ChauRocks”****, *[https](https://hackgame.chaurocks.com/)[://hackgame.chaurocks.com](https://hackgame.chaurocks.com/)* (solution**s/hints at *[https://github.com/KAUT](https://github.com/KAUTH/HackGame3-solutions)[H/HackGame3-so](https://github.com/KAUTH/HackGame3-solutions)[lutions](https://github.com/KAUTH/HackGame3-solutions)*). This article is a real-life example of one of the t**ricks you need to t**hink of to proceed to the next levels of the game.*

### UPDATE:

After writing this article** I came across many other websites whose paid prime program can be c****ircumvented with the same method** **(blocking Jav****aScript)**. Moreover, in some cases, you can **browse having your**** add****-blocker on**, something that was not allowed before.

The sites affected that I have found so far are:

* [MIT Technology Review](https://www.technologyreview.com/)**,**
* [Business Insider](https://www.businessinsider.com/)**,**
* [The New York Times](https://www.nytimes.com/)**,**
* [The Washington Post](https://www.washingtonpost.com/)**,**
* [The Economist](https://www.economist.com/)**,**
* [The Boston Globe](https://www.bostonglobe.com/)

*All the** above websites have been informed about the flaw.*


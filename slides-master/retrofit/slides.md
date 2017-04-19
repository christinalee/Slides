<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
# Retrofitting Retrofit

Christina Lee |  Pinterest 
<br>

!!!

# Networking

!!!

> "If you see something [on your phone], it involved the network"

note: telling Boo about networking layer

!!!

![Kind of a big deal gif](img/kindofabigdeal.gif)

!!!

![I'm not ready for this gif](img/imnotready.gif)

note: hashtag not qualified

!!!

![Can we panic now gif](img/canwepanicnow.gif)

note: hashtag help

!!!

# Retrofit expert?

![no gif](img/no.gif)

!!!

![Holdiay inn express meme](img/holidayinnexpress.jpg)

!!!

## We have a networking layer that mostly works but is hard to modify and is based on technologies from 5 years ago expert?

!!!

<img src="img/hellyeah.gif" alt="Giphy meme" style="width: 480px;"/>
<div align="center" style="font-size:12px">
<a href="https://giphy.com/gifs/excited-yes-agree-jErnybNlfE1lm">via GIPHY</a>
</div>

!!!

<img class="plain"  src="img/applesandoranges.jpg"/> 

note: it's important to realize those are two different things

!!!

![pouring foundation gif](img/new_foundation.gif)

!!!

<table>
  <tr>
    <td> <img class="plain"  src="img/existing_foundation.gif"/> </td>
    <td> <img class="plain"  src="img/existing_foundation_2.gif"/> </td>
  </tr>
</table>

!!!

<img class="plain"  src="img/retrofitdocs.png"/> 

note: because if you don't already have a code base...

!!!

### <div style="text-align: center">Interface,<br>Builder,<br>Call,<br>Annotations<br></div>

# <div style="text-align: center"> BOOOM </div>

note:
- which makes it all look easy

!!!

# Let's do a trial!

note: 
- not going to wholesale swap willy nilly
- need test case and validation to get approval

!!!

![searching gif](img/searching.gif)

note: 
- but what?
- if you're like us, you have several modules that separate network calls based on features

!!!

![package apis screenshot](img/package_apis.png)

note: this begs the question which one do you choose?

!!!

<blockquote class="twitter-tweet" data-conversation="none" data-lang="en"><p lang="en" dir="ltr">My favorite bit of any talk I give is talking about the dumb shit I did so the audience doesn’t do it. Makes the hair pulled out worth it.</p>&mdash; Ellen Shapiro (@designatednerd) <a href="https://twitter.com/designatednerd/status/841800315003379712">March 14, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

note: now's a good time to mention

!!!

# Account API

note: 
- I chose the account API because AUTH AND UNAUTH!
- I was thinking about this from a technical perspective, not a practical one

!!!

# Feed API

note: 
- but what this ignored was that our feed was the most important litmus test
- once I completed the account api there were still a lot of unknowns
- spend more time on this, choose wisely for maximum learning

!!!

## Lesson 1: Choose your test case with care

!!!

## Lesson 2: There will be startup costs
todo: maybe you aren't as dumb as me, but I forgot I'd need to impl oauth
todo: budget time for everyone to offer opinions about volley, OkHTTP on its own, and retrofit

!!!

some bridge

!!!

# Lesson 3: Don't hate the player, hate the switch

!!!

```
public static void login(/*params*/, /*handler*/) {
	// some logic
	switch (params.getType()) {
		case SignupParams.TYPE_FACEBOOK:
			paramsMap.put("fb_id", params.fbId);
			paramsMap.put("fb_token", params.fbToken);
			path = "facebook/";
			break;
		case SignupParams.TYPE_EMAIL:
			path = "email/";
			break;
		...
	}
	// more logic
}
```

note: 
- because building out products happens iteratively, you most likely have something like this
- common form of networking tech debt
- write a login, then add more ways to login and expand the original func
- there are two main issues with this

!!!

```
if (params.emailAddress != null) {
	paramsMap.put("email", params.emailAddress);
}
if (params.username != null) {
	paramsMap.put("username", params.username);
}
if (params.password != null) {
	paramsMap.put("password", params.password);
}
```

note:
- issue 1: you get code like this
- different login types require different args so can't make them all mandatory

!!!

# "We're a package deal!"

note: 
- issue 2: login types are closely coupled for no good reason
- modifying facebook login can very possibly break email login

!!!

```
@FormUrlEncoded
@POST("something/facebook/")
fun facebookSignup(
  @Field("facebook_id") facebookId: String,
  @Field("facebook_token") facebookToken: String,
) : Observable<T>

@FormUrlEncoded
@POST("something/twitter/")
fun twitterSignup(
  @Field("twitter_id") twitterId: String,
  @Field("twitter_token") twitterToken: String,
  @Field("twitter_token_secret") twitterTokenSecret: String,
) : Observable<T>
```

note: 
- we're good citizens, so let's fix this/break these up

!!!

# Hurray! We win!

note: 
- cool, this seems objectively better

!!!

# But...

note: 
- shared logic across login events

!!!

# Where do we put shared behavior now?

note:
- not for me to say, developer dependent
- but you'll need to think about this

!!!

## When adding Retrofit retroactively, switches will be among your most time intensive migrations.

note: this is not a problem when you build with it from the start

!!!

# Lesson 3 (soon to be obsolete): Check your Rx

note: 
- one of the benefits of Retrofit is exposing streams
- this was a big reason we wanted to adopt it

!!!

## Are you an Rx1 holdout?

!!!

```
packagingOptions {
  exclude 'META-INF/rxjava.properties'
}
```

note:
- not retrofit explicit
- rx1 and rx2 include the same file

!!!

# Phew!

note: but theres more

!!!

```
java.lang.IllegalArgumentException: Unable to create call 
  adapter for io.reactivex.Observable<T<Map<U, V>>>
```

!!!

``` 
compile 'com.squareup.retrofit2:adapter-rxjava2:2.2.0'
```

```
Retrofit.Builder()
                .addCallAdapterFactory(RxJava2CallAdapterFactory.create())
```

!!!

# Lesson 4: With great interceptor power, comes great interceptor responsibility

!!!

## Background: We already used OkHttp

note:
- but like most things, it didn't start that way and was added later
- focus on going from Apache to OkHttp
- didn't take advantage of interceptors even though we could have

!!!

## Interceptors: 

- usually used to add/remove/change headers
- can also be used for crazy shenanigans like <a href="https://publicobject.com/2016/01/17/sneaking-data-into-an-okhttp-interceptor/">this</a>













!!!

# That's all folks!

Slides posted: <a href="http://bit.ly/2d6WxQl">http://bit.ly/2d6WxQl</a>  

Find me on 

- Twitter: <a href="twitter.com/runchristinarun">@RunChristinaRun</a>
- Pinterest: <a href="https://www.pinterest.com/clehrlee/">pinterest.com/clehrlee</a>

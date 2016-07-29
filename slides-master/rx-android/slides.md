<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
# Intro to Rx
						
Christina Lee / @RunChristinaRun

note: <a href="http://twitter.com/RunChristinaRun">@RunChristinaRun</a>
!!!


##<div align="center">Fasten</div>
##<div align="center">Your</div>
##<div align="center">Seatbelts</div>

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->

## Background

!!!

## Background

* Why do we need asynchronous work? <!-- .element: class="fragment" data-fragment-index="1" -->
* What is the state of async on Android? <!-- .element: class="fragment" data-fragment-index="2" --> 

!!!

#<div align="center"> Users are our friends</div>

note: Users are our friends

!!!

## Tools for Async Work 

* AsyncTask
* Future
* Event bus
* Observable

!!

## More info

For a complete run down of async primitives in both iOS and Android, check out the blog post on [Concurrency Categorization](https://medium.com/math-camp-engineering/concurrency-categorization-eea7b871e0ac#.mknu37jlw "Concurrency Categorization") written by my coworker Bkase 

!!!

## Tools for Async Work 

* AsyncTask
* <s>Future</s> ListenableFuture
* Event bus
* Observable

!!!

## Ways to Evaluate

* When they run
* How they run
* Whom they impact

!!!

## Example: AsyncTask

* Must be explicitly started
* Work cannot be modified or composed
* Data is output via side effects 

Note: Point out that a future runs upon creation
!!!

## Example: AsyncTask

```java
private class SomeTask extends 
	AsyncTask<ParamsType, ProgressType, ResultType> {

  //some implementation here
  protected ResultType doInBackground(ParamsType params) { ... }

  protected void onPostExecute(ResultType result) { ... }
}

new SomeTask().execute(params)
```

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->

## Asynchronous Ideals

!!!

## Asynchronous Ideals

* Explicit execution 
* Easy thread management
* Easily transformable
* As few side effects as possible

!!!

## Benefits

Explicit execution:
* Create async tasks without needing to execute them immediately

!!!

## Benefits

Easy thread management: 
* Easily assign code to proper thread
* Clearly understand which thread code is running on (readability)

!!!

## Benefits

Easily composable:
* Async tasks are often interdependent
* Direct chaining means less room for error

!!!

## Benefits

Minimized side effects:
* Code is easy to trace, and easy to reason about

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->

## Rx

!!!

## Rx
* is explicitly started
* makes it easy to determine which thread work is performed on
* is incredibly easy to transform and combine
* has a lesser degree of sideeffects than many alternatives

!!!

## Fast facts

1. Library for composing asynchronous events  <!-- .element: class="fragment" data-fragment-index="1" -->
2. Rx = "Reactive Extensions" <!-- .element: class="fragment" data-fragment-index="2" -->
3. Many flavors (RxJava, RxSwift, RxJs...) <!-- .element: class="fragment" data-fragment-index="3" -->

!!! 

## Rx = Observables + LINQ + Schedulers


!!! 

## Rx = <b><font color="#32607D">Observables</font></b> + LINQ + Schedulers

represent asynchronous data streams

!!!  

## Rx = Observables + <b><font color="#32607D">LINQ</font></b> + Schedulers

query & combine asynchronous data streams with operators

!!!  

## Rx = Observables + LINQ + <b><font color="#32607D">Schedulers</font></b>

manage concurrency

!!!

### 1) Represent Asynchronous data streams
### 2) Query and Combine streams with operators
### 3) Manage concurrency

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->

## Observables

!!!

## Observables

* Streams of data
* Pull based <i>(Caveat: Subjects)</i>
* Create, store, pass around
* <b>Abstract away threading, syncronization, concurrency, etc</b>

!!!

## Factory Analogy

* Raw Material == creation
* Conveyor Belts == operators/transforms
* End Product == output

!!!

## Observables

1. Put data in
2. Get data out

!!!

## Observables

<font color="#32607D"><b>1. Put data in</b></font><br>
<span>2. Get data out</span>

!!!  

## Observable Creation

```java 
Observable.just("Hello World!")
``` 
note: Single value

!!!

## Observable Creation

```java
val names: Array<String> = 
	arrayOf("Christina", "Nicole", "Alison")

//Will output Christina --> Nicole --> Alison --> X
Observable.from(names)
```

note: from iterables

!!
## Java

```java
String[] names = {"Christina", "Nicole", "Alison"};

//Will output: -> Christina -> Nicole -> Alison -> x
Observable.from(names)
```
!!!  

## Observable Creation

```java
Observable.create<String> { s -> 
	s.onNext("I created an observable!")
	s.onCompleted()
}
```

!!

## Java

```java
Observable.create(new OnSubscribe<String>() {
  @Override
  public void call(Subscriber<? super String> subscriber) {
    subscriber.onNext("I created an Observable!");
    subscriber.onCompleted();
  }
})
```

!!!

![](http://media1.giphy.com/media/ToMjGpnXBTw7vnokxhu/giphy.gif "Woah Gif")

note: In order to understand how to put data in, we need to know what the user expects on the other end

!!

Image from Giphy: <br>
http://media1.giphy.com/media/ToMjGpnXBTw7vnokxhu/giphy.gif

!!!

## Observables

<span>1. Put data in</span><br>
<font color="#32607D"><b>2. Get data out</b></font>

note: <span class="fadedfont">1. Put data in</span><br>
<b><font color="red">2. Get data out </font><b>

!!!

<br>
## <div align="center">Streams</div>
# <div align="center">Streams</div>
## <div align="center">Streams</div>

!!!

## Observables

What do we need?
1. Give me the next piece of data! <!-- .element: class="fragment" data-fragment-index="1" -->
2. Is there any more data left to process?  <!-- .element: class="fragment" data-fragment-index="2" -->
3. Did any errors happen that I should know about? <!-- .element: class="fragment" data-fragment-index="3" -->

!!!

## Observables

How do we get those?
1. onNext <!-- .element: class="fragment" data-fragment-index="1" -->
2. onComplete  <!-- .element: class="fragment" data-fragment-index="2" -->
3. onError <!-- .element: class="fragment" data-fragment-index="3" -->

!!!

## On Next

```java
Observable.create<String> { s ->
	val expensiveThing = doExpensiveComputationHere()
	s.onNext(expensiveThing)

	val otherExpensiveThing = doOtherExpensiveComputation()
	s.onNext(otherExpensiveThing)
	
	//all done
	s.onComplete()
}
```

!!

## Java

```java
Observable.create(new OnSubscribe<Object>() {
  @Override
  public void call(Subscriber<? super Object> subscriber) {
    Object expensiveObject = doExpensiveCompHere();
    subscriber.onNext(expensiveObject);

    Object otherExpensiveObj = doOtherExpensiveCompHere();
    subscriber.onNext(otherExpensiveObj);

    subscriber.onCompleted();
  }
})
```

!!!

## On Next

Call as little or often as you want\*
<br>
<br> 
<i>*Not an entirely truthful statement</i><!-- .element: class="fragment" data-fragment-index="1" -->

!!

Advanced Rx: 
It is not hard to create an observeable that fires more quickly than a consumer can process events

But no fear, Shakespeare! Read up on `Backpressure`, `Sample`, `Throttle`, and their ilk for ways to cope

!!!

##  Pop Quiz!

What does this stream look like?
```java
s.onNext(2)
s.onNext(3)
s.onComplete()
```

!!!

--> `2` --> `3` --> X

!!!

##  Pop Quiz!

What does this stream look like?
```java
s.onNext("Apple")
s.onNext("Bannana")
s.onNext("Orange")
s.onNext("Pineapple")
s.onComplete()
```

!!!

--> `Apple` --> `Bannana` --> 
	`Orange` --> `Pineapple` --> X

!!!

##  Pop Quiz!

What does this stream look like?
```java
s.onNext(1)
s.onNext(10)
s.onNext(101)
s.onNext(1010)
s.onNext(10101)
...
s.onNext(101010101010)
s.onComplete()
```

!!!

### Unsurprisingly 
`1` --> `10` --> `101` --> `1010` --> `10101` --> `101010` --> ... `101010101010` --> X

!!!

## On Error

* If an error occurs in a stream, it will cease to output any more data.

* Errors are handled in one place

note: other async primitives  must each know how to handle errors or how to propagate them through. With Rx, all errors are surfaced in onError, which is the end of the line

!!!

## Output if function succeeds

```java
Observable.create<String> { s -> 
  try { 
    val result = functionThatMightError()
    s.onNext(result)
  } catch (e: Error) {
    s.onError(e) 
  }
  s.onCompleted() 
}
```

!!

### Calls `onNext()` then `onCompleted()`

!!!  

## Output if function fails

```java
Observable.create<String> { s -> 
  try { 
	val result = functionThatMightError()
	s.onNext(result)
  } catch (e: Error) {
	s.onError(e)
  }
  s.onCompleted() //<--NOPE
}
```

!!

### Only calls `onError()`

!!!

## On Complete

* no more items will be emitted from stream
* safe for cleanup!

!!!

## Observable Review

1. onNext
2. onComplete
3. onError

note: both onComplete and onError are "stopping" events, but they give you different reasons why you stopped (i.e because of success or because of failure)

!!!

![](http://media3.giphy.com/media/zG6MKhlBxIloc/giphy.gif "Too Easy!")

!!

Image from Giphy: <br>
http://media3.giphy.com/media/zG6MKhlBxIloc/giphy.gif 

!!!

## Remember LINQ?

!!!

### Observables really shine when you need to compose several streams of asynchronous data 

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
##Operators

!!! 

## Operators

Observables have a plethora of operators for, among other things:
* combining data
* filtering/reducing data
* transforming data

!!!

## Operators

```java
Observable.from([1,2,3,4]).map { num ->
	num + 1	
}
```
Output:
`2` --> `3` --> `4` --> `5`

!!

##Java

```java
Integer[] numbers = {1, 2, 3, 4};
Observable<Integer> obs = Observable.from(numbers).map(
  new Func1<Integer, Integer>() {
    @Override
    public Integer call(Integer integer) {
      return integer + 1;
    }
  }
);

//Outputs: 2 --> 3 --> 4 --> 5 --> x
```
!!!

## Operators

```java
Observable.from([1,2,3,4]).filter { num ->
	num % 2 == 0
}
```
Output:
`2` --> `4`

!!

##Java

```java
Integer[] numbers = {1, 2, 3, 4};
Observable<Integer> obs = Observable.from(numbers).filter(
  new Func1<Integer, Boolean>() {
    @Override
    public Boolean call(Integer integer) {
      return integer % 2 == 0;
    }
  }
);

//Output: 2 --> 4 --> x
``` 

!!!

## Operators

```java
val schoolFriendsObs = 
	Observable.from(arrayOf("Mo", "Dave"))
val workFriendsObs = 
	Observable.from(arrayOf("Nicole", "Alison"))

val allFriendsObs = Observable.merge(
	schoolFriendsObs,
	workFriendsObs
)
```

!!

```java
//Note: Output is only in the same 
//order because input was synchronous
I/RxKotlinHelper: onNext(Mo)
I/RxKotlinHelper: onNext(Dave)
I/RxKotlinHelper: onNext(Nicole)
I/RxKotlinHelper: onNext(Alison)
I/RxKotlinHelper: onComplete()
```

!!!

### Best resource: <a href="http://rxmarbles.com/">RxMarbles</a>

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
## Lastly, Schedulers

!!!

## Start Listening

* without `Subscribe`, nothing* happens 

!!

###*Advanced Rx caveat: code execution can depend on Observable creation type

<a href="http://blog.danlew.net/2015/07/23/deferring-observable-code-until-subscription-in-rxjava/">See this blog post for a brief explanantion of this</a>

!!!

# Subscribe Basics

* Subscribers can take different numbers of functions
* Best practice: always pass an onError func 
* Use `subscribeOn` and `observeOn` to assign to threads

!!!  

## Subscribe Basics 

```java

val names = arrayOf("Christina", "Nicole", "Alison")

Observable.from(names).subscribe(
	{ next ->
	  Log.i(TAG, "onNext($next)")
	}
)
```

!!

## Output: 

```java
I/RxKotlinHelper: onNext(Christina)
I/RxKotlinHelper: onNext(Nicole)
I/RxKotlinHelper: onNext(Alison)
```

!! 

## Subscribe Basics

```
Observable.just("Hello World!")
	.subscribe(new Action1<String>() {
		@Override
		public void call(String s) {
			Log.i(TAG, "onNext(" + s + ")");
		}
	});
```

note: in Java, if you don't want to supply all functions, you can use Action1 

!!

## Output

```java
I/RxJavaHelper: onNext(Hello World!)
```

!!!

## Subscribe Basics 

```java 
val names = arrayOf("Christina", "Nicole", "Alison")

Observable.from(names)
	.subscribe(
	  { next ->
	    Log.i(TAG, "onNext($next)")
	  },
	  { error ->
	    Log.i(TAG, "onError($error)")
	  },
	  {
	    Log.i(TAG, "onCompleted()")
	  }
	)
```
note: can call observable's subscribe with different number of funcs... best practice is always to add onError

!!

## Output: 

```java
I/RxKotlinHelper: onNext(Christina)
I/RxKotlinHelper: onNext(Nicole)
I/RxKotlinHelper: onNext(Alison)
I/RxKotlinHelper: onCompleted()
```

!!

## Java Translation

```java
String[] names = {"Christina", "Nicole", "Alison"}; 
Observable.from(names).subscribe(new Subscriber<String>() {
	@Override public void onCompleted() {
	  Log.i(TAG, "onCompleted()");
	}

	@Override public void onError(Throwable e) {
	  Log.i(TAG, "onError()", e);
	}

	@Override public void onNext(String string) {
	  Log.i(TAG, "onNext(" + string + ")");
	}
}); 
```

!!

## Output

```java
I/RxJavaHelper: onNext(Christina)
I/RxJavaHelper: onNext(Nicole)
I/RxJavaHelper: onNext(Alison)
I/RxJavaHelper: onCompleted() 
```

!!!

## Subscribe Basics

* `.subscribe` will return a Subscription
* Unsurprisingly, you can unsubscribe from it

!!!

## Subscribe Basics

```java
val sub: Subscription = SomeObs.subscribe{ next ->
	Log.i(TAG, "onNext($next)")
}
sub.unsubscribe()
``` 

!!!

## What about threads?

1. `subscribeOn`
2. `observeOn`

!!!

## Subscribe On

* Declare only once
	* If set multiple times, declaration farthest upstream wins 
* Defaults to thread on which observable is created
* Obs will always kick off execution on this thread, no matter where it's declared

!!!

## Observe On

* Declare as many times as needed*
* Affects all operators downstream

!!

It is possible to create `backpressure` issues if using observeOn with fast emitting streams.

For a gif that explains why, checkout the end of <a href="http://tomstechnicalblog.blogspot.com/2016/02/rxjava-understanding-observeon-and.html">this</a> blog post

!!!

## Let's build!

!!!

<!-- .slide: data-transition="none" --> 
## Subscribe/Observe On

<br>
<br>
<br>
<br>
<br> 

!!!

<!-- .slide: data-transition="none" --> 
## Subscribe/Observe On

```java
Observable.from(arrayOf("Red", "Orange", "Blue")) 
```
<br>
<br>
<br>
<br>

!!!  

<!-- .slide: data-transition="none" --> 
## Subscribe/Observe On

```java
Observable.from(arrayOf("Red", "Orange", "Blue")) 
  .doOnNext { color ->
	Log.i(TAG, "Color $color pushed through 
		  on ${Thread.currentThread()}")
  }
```
<br>
<br>

!!!

<!-- .slide: data-transition="none" --> 
## Subscribe/Observe On

```java
Observable.from(arrayOf("Red", "Orange", "Blue")) 
  .doOnNext { color ->
	Log.i(TAG, "Color $color pushed through 
		  on ${Thread.currentThread()}")
  }.observeOn(Schedulers.io())
```
<br>
<br>


!!!

<!-- .slide: data-transition="none" --> 
## Subscribe/Observe On

```java
Observable.from(arrayOf("Red", "Orange", "Blue")) 
  .doOnNext { color ->
	Log.i(TAG, "Color $color pushed through 
		  on ${Thread.currentThread()}")
  }.observeOn(Schedulers.io()).map { color ->
	color.length
  }
```
<br>

!!!

<!-- .slide: data-transition="none" --> 
## Subscribe/Observe On

```java
Observable.from(arrayOf("Red", "Orange", "Blue"))
  .doOnNext { color ->
	Log.i(TAG, "Color $color pushed through 
  		  on ${Thread.currentThread()}")
  }.observeOn(Schedulers.io()).map { color ->
	color.length
  }.subscribe { length ->
	Log.i(TAG, "Length $length being recieved 
		  on ${Thread.currentThread()}")
  }
```

!!

```
I/Rx: Color Red pushed through on Thread[main,5,main]
I/Rx: Color Orange pushed through on Thread[main,5,main]
I/Rx: Color Blue pushed through on Thread[main,5,main]

I/Rx: Length 3 being 
	recieved on Thread[RxIoScheduler-2,5,main]
I/Rx: Length 6 being 
	recieved on Thread[RxIoScheduler-2,5,main]
I/Rx: Length 4 being 
	recieved on Thread[RxIoScheduler-2,5,main]
```
!!

### Subscribe on Worker, Observe on UI is a common threading pattern

For more info about using `compose` to reduce boilerplate with `ObserveOn` and `SubscribeOn`, checkout <a href="http://blog.danlew.net/2015/03/02/dont-break-the-chain/">this</a> blog post.

!!!

## Subscribe/Observe On

```
Observable.from(arrayOf("Red", "Orange", "Blue"))
	.doOnNext { color ->
		Log.i(TAG, "Color $color pushed through 
			on ${Thread.currentThread()}")
	}.observeOn(Schedulers.io())
	.map { color -> color.length }
	.subscribeOn(Schedulers.computation())
	.subscribe { length ->
		Log.i(TAG, "Length $length being recieved 
			on ${Thread.currentThread()}")
	}
```

!!

```
//Output trimmed to fit

I/Rx: Red pushed on Thread[RxComputationScheduler]
I/Rx: Length 3 recieved on Thread[RxIoScheduler]

I/Rx: Orange pushed on Thread[RxComputationScheduler]
I/Rx: Length 6 recieved on Thread[RxIoScheduler]

I/Rx: Blue pushed on Thread[RxComputationScheduler]
I/Rx: Length 4 recieved on Thread[RxIoScheduler]
``` 

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
## Rx & Android

!!!

## Rx & Android

A few things you can do
1. Bind to clicks and filter clicks for a given area
2. Flatmap cache hit with network call
3. Handle auth flow with a single stream

!!!

##Ex: Debounce a button

```java
fun Button.debounce(length: Long, unit: TimeUnit) {
  setEnabled(false)

  Observable.timer(length, unit)
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe {
	  setEnabled(true)
    }
}
```

!!!

##Ex: Get size of 24 most recent db photos

```java
fun size(): Observable<Int> = Database.with(ctx)
      .load(DBType.photoDetails)
      .orderByTs(Database.SORT_ORDER.DESC)
      .limit(24)
      .map { it.size() }
```

!!!

##Ex: Download things
```java
return Observable.create<Unit> { s ->
  val outputFile = writeOutputFile(mediaFile)

  when (type) {
    Type.Photo -> addPicToGallery(ctx, outputFile)
    Type.Video -> addVideoToGallery(ctx, outputFile)
    else -> {
      s.onError(Error("Unexpected download type!"))
    }
  } 
  s.onNext(Unit)
  s.onCompleted()
}.subscribeOn(Schedulers.io())
```

!!!  

## Ex: Lift data from LocalBroadcasts

```java
fun codeObservable(): Observable<String?> {
  val filter = IntentFilter(SmsUtility.INTENT_VERIF_CODE)
  return ContentObservable.fromLocalBroadcast(this, filter)
	.map { intent ->
		intent.getStringExtra(SmsUtility.KEY_VERIF_CODE)
	}
} 
``` 

!!!

## Ex: Onboarding flow

```java
timedAuthObservable
  .observeOn(Schedulers.io())
  .flatMap { code ->
        userModel.sendVerifyResponse(code)
  }.flatMap {
        userModel.getSuggUsername().onErrorReturn { "" }
  }.observeOn(AndroidSchedulers.mainThread())
  .subscribe({ suggestedUsername -->
		//update UI with suggested username
  })
```

!!!

##Ex: Fetch  once verified

```java
//Verify with backend, then prepare data for UI
override fun getVerifiedData(code: String): 
									Observable<Unit> {
	return UserService.noAuthClient
		.verifyUser(authToken, code)
		.flatMap { 
			UserService.authClient.fetchUserDetails()
		}.map{ data ->
			loadableUserState.loadFromData(data)
		}.observeOn(AndroidSchedulers.mainThread())
}
```

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
## In Summary 

!!!

## Rx is not the only game in town...

!!!

## ...but Rx is one of the most powerful games in town

!!!

## There is a steep learning curve...

!!!  

## ...but once you can get the hang of it, Rx can help you reduce errors and bugs

!!!

## Streams can be a big pardigm shift...

!!!

## ...but they will make it easy to understand how data flows through your app.

!!!

## In Summary

<table>
<td><img width="400" src="https://upload.wikimedia.org/wikipedia/commons/7/79/2009-03-11_Beat_up_car_driving_in_Durham.jpg"></td>
<td><img width="500" src="https://upload.wikimedia.org/wikipedia/commons/d/d5/Tesla_Roadster_2.5_%28front_quarter%29.jpg"></td>
</table>

note: if you have a honda, no need for a Tesla roadster to get to work. You can get to work just fine with a honda. But it'll be slower, and it'll probably be less safe
learning can take time, but that's not a reason not to do it
on city streets, a honda will do fine. You won't be able to flex the tesla's muscle anyways... but on highways, you'll soon notice the difference

!!!

## In Summary

* More Resources
	* RxMarbles & RxDocumentation
	* Dan Lew's blog post series
* Slides are live
* Find me online: 
	* @RunChristinaRun

note: just scratched the surface, there is much more out there, etc
!!!

![](http://media3.giphy.com/media/upg0i1m4DLe5q/giphy.gif "That's all")

!!

Image from Giphy: <br>
http://media3.giphy.com/media/upg0i1m4DLe5q/giphy.gif

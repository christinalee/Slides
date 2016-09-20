<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
# Intro to Kotlin

<div align="center">
Christina Lee |  Pinterest 
<br>
<a href="https://www.pinterest.com/clehrlee/">pinterest.com/clehrlee</a> | <a href="twitter.com/runchristinarun">@RunChristinaRun</a>
!!!

# What is Kotlin?

- JVM language
- from Jetbrains

note: breaking that down:
!!!

<div align="center">
*JVM*: what can be done in Kotlin can be done in Java* 
<br>
<br>
<br>
</div>
  
  
<div align="right">
*mostly
</div>

!!!

<div align="center">
*Jetbrains*: tooling is top notch
</div>

!!!

### Kotlin is tooling on top of Java

note: this does not mean Google can't black list it, but it does mean it's harder
!!!

# What does it add?

1. Static typing + Smart Casts
2. Null safety
3. Conciseness
4. Higher order functions
5. Mutability protection
6. Lambdas
7. Better generics

!!v

## Find more at:
<a href"https://kotlinlang.org/docs/reference/comparison-to-java.html"> https://kotlinlang.org/docs/reference/comparison-to-java.html </a>

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->

# How does it affect Android?

!!!

# 1. Static Typing + Smart Casts

*Shared*: static + strong typing
  
*Different*: compiler tracked smart casting

!!!

# Smart Casting

In Java:
![java explicit cast example](img/java_explicit_casting.png)

!!!

# Smart Casting

In Kotlin:
![kotlin smart cast example](img/kotlin_smart_casting_example.png)

!!!

# Smart Casting

In Kotlin:
![kotlin smart cast example with dialogue](img/kotlin_smart_cast_with_dialogue.png)

!!!

# Smart Casting

- Compiler tracks `is`-checks
- Inserts safe casts* on your behalf
- Includes negative checks & short circuiting
- Sidenote: global vars

!!v

# More info: Safe Casts

Kotlin allows for both safe and unsafe type casting:  
```
/* UNSAFE */
val x: String = y as String //throws exception if cast fails

/* SAFE */
val x: String? = y as? String //null if cast fails
```

!!v

# More info: Vars + Smart Casting

Smart casting only applies if the compiler can guarantee that the field has not been mutated since the `is` check. 
From the <a href="https://kotlinlang.org/docs/reference/typecasts.html">Kotlin Docs</a>:

![Smart cast table](insert table here.jpg)

!!!

# 2. Null safety

//Todo: insert happy meme here

note: Billion dollar problem, cannot over state what a big deal this is
!!!

## Nulls are part of the type system

!!!

# 2. Null safety

- Types default to non-nullable
- Mark nullable with the addition of a `?`

!!!

# 2. Null safety

```
var nonNullType: Type
nonNullType = null // compilation error

var nullableType: Type? 
nullableType = null // ok
```

!!!

# Null safety helpers

- Safe accessor: `?`
- Elvis opertator: `?:` 

!!!

# Working with nulls

```
// if messageFromServer is null, we'll display a default msg
val toDisplay: String = messageFromServer ?: defaultMessage

// cascading null safety
val maybeNullThing: Thing? = /* something */
val maybeName: String? = maybeNullThing?.someProperty?.name

// safe scoping
maybeNullThing?.let {
  // code will only be executed if not null
}
```

note: toy examples
!!!

# Working with nulls
```
val playerControl: PinterestPlayerControl? = /* some init */
var position: Int
        get() {
            return playerControl?.currentPosition ?: 0
        }
        set(position) {
            playerControl?.setPosition(position)
        }
```

note: more robust example
!!!

# 3. Conciseness

//todo: insert gif here

!!!

## Java files consistently shrink

!!!

# 3. Conciseness

Java:
```
view.setOnClickListener(new OnClickListener() {
	@Override
	public void onClick(View v) {
		// do something
	}
});
```

!!!

# 3. Conciseness

Kotlin:
```
view.setOnClickListener { 
	//do something 
}
```

!!!

# 3. Conciseness

Kotlin:
```
view.setOnClickListener { view: View ->
	//do something with view
}
```

!!!

# 4. Higher order functions

> Higher order functions are functions which can take as arguments, or return as output, other functions

!!!

# 4. Higher order functions

```
public inline fun <T> Iterable<T>.forEach(
	action: (T) -> Unit // <-- input is a function
): Unit {
    for (element in this) action(element)
}
```

!!!

# 4. Higher order functions

```
fun <T, R> List<T>.map(transform: (T) -> R): List<R> {
  val result = arrayListOf<R>()
  for (item in this)
    result.add(transform(item))
  return result
}
```

!!!

# 4. Higher order functions

```
for (i in 0 .. icons.childCount - 1) {
	var icon : View = icons.getChildAt(i)
	if (icon is IconView) {
		resizeIconView(icon)
	} else {
		resizeActionView(icon)
	}
}
```
note: can be as simple as replacing a for loop so no manual indexing, might not seem like a big deal, but no more index out of bounds exceptions

!!!

# 4. Higher order functions

```
// no more indexing
icons.childrenSequence().forEach { view: View ->
	when(view) {
		is IconView -> resizeIconView(view)
		else -> resizeActionView(view)
	}
}
``` 

!!!

# 4. Higher order functions

```
allItems.map { item -> item.icon }
	.filterNotNull()
	.forEach { icon: Drawable ->
            calculateOptimalIconSize(icon)
        }
```

note: can also be more complicated

!!!

# 5. Mutability protection

Mutability notation is a requirement.  
You must choose `val` or `var`.

!!!

## Use `var` for something that will vary with time.  
## Use `val` for a value that won't change.

!!!

# 5. Mutability protection

The compiler is your friend!

!!!

# 5. Mutability protection

Java:
```
//no compiler warning:
if (globalVar != null) {
  globalVar.someField() // this can NPE
}
```

!!!

# 5. Mutability protection
Kotlin:
![mutability protection](img/kotlin_mutability_protection.png)

!!!

> Smart cast to 'GlobalVarType' is impossible, because 'globalVar' is a mutable property that could have been changed by this time

!!!

# Mutability Best Practice

- Always default to `val` until something needs to be made into `var`
- Compiler will flag non-mutating vars

!!!

# 6. Lambdas

Already seen lambdas
- <a href="#/22">click listeners</a>
- <a href="#/29">higher order functions</a>

!!!

# 6. Lambdas

Perks
- can be inlined: <a href="https://kotlinlang.org/docs/reference/inline-functions.html">"performant custom control structures"</a>
- closures

!!v

# More info: Java 8 lambdas

With limited support for Java 8 being rolled out on Android, you might wonder about J8 lambdas.
Here is a breakdown of some of the <a href"https://blog.jetbrains.com/kotlin/2016/03/kotlins-android-roadmap/">Kotlin/Java lambda differences</a>.
Also note <a href="http://bruceeckel.github.io/2015/10/17/are-java-8-lambdas-closures/">Java 8's closure scope</a> is more restrictive than Kotlin's.

!!!

# Inlined functions

- use `inline` keyword
- avoid object instantiation
- `return` behavior is `fun` level

note: non-local return

!!!

# 7. Better generics

TL;DR: Use site variance  
Read more: <a href="https://kotlinlang.org/docs/reference/generics.html">Kotlin docs</a>

!!!  

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->

# How can you adopt it?

!!!

## Use IntelliJ
<div align="center">
Support for Eclipse is not competitive 
</div>

!!!

## Include the dependency
<div align="center">
736KB (as of 1.0.3)
</div>

note: runtime is relatively small at 736KB
!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
# Is it right for big companies?

!!!

# Context: 
- Highlight
- Pinterest

!!!

## Syntax is very easily picked up by existing Java developers 
- readable by Java devs
- code reviews require only a small startup cost
- detail work handled by compiler

note: 
have polled devs to make sure lines are readable, and they always say yes 
only really need mutable, null, and (lesser degree) lambdas to get started
Java developers continue to code review me
intracacies of Kotlin are caught by compiler -- nulls, mutability, etc, so are not on CR's shoulders

!!!

## Piecemeal file introduction
- no additional framework
- Kotlin exports Java interfaces
- a few excited employees can lead the charge

note: compare to swift/obj c where you need to maintain bridging headers

!!!

## Conversion
- right click conversion
- look for `!!`

note:
naive conversion from java compiles down to the same code, so you're exactly where you started
but this can be made better, so go through and clean up
this process will make it very clear how kotlin makes your code better

!!!

## Compile times
- historically slow
- on the Kotlin roadmap 

!!!

## The Elephant in the Room: Google 

- JVM is our friend
- JVM is not necessarily a savior

!!!


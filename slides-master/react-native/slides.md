<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
# Move fast with React Native

Christina Lee |  Pinterest 
<br>

!!!

# Background

note: before pinterest, worked at a startup of ~10

!!!

<img src="img/highlight.jpg" alt="Highlight Team" style="width: 700px;"/>

note: this was us

!!!

# We were iterating quickly!

!!!

- Highlight
- Roll
- View
- Shorts

note: to name a few

!!!

# We could not move fast enough with our current tools

!!!

## Why?

- Android:
	- no limit on apk uploads
	- slow adoption

!!!

## Why?

- iOS:
	- adoption is good 
	- review times can be long
	- possibility of rejection

!!!

<img src="img/app_review_times.png" alt="App Review Times" style="width: 700px;"/>

<div align="center" style="font-size:12px">
source: http://appreviewtimes.com/
</div>

note: app times have come down drammatically, but in the past, when we were developing, they could be up to two weeks
can also have updates be rejected, and we often did, so things are further of your control

!!!

# We needed a new tool!

!!!

<img src="img/react-native.png" alt="React Native Logo" style="width: 500px;"/>

!!!

# Timeline

- First version of RN emerged from an internal FB hackathoon in 2013
- Initially launched May 2015, iOS only
- Android support came in September of 2015

note: about this time, RN was gaining popularity

!!!

<img src="img/RN-search-frequency.png" alt="Google Trends Analysis" style="width: 1000px;"/>

<div align="center" style="font-size:12px">
Data source: Google Trends (www.google.com/trends)
</div>

note: ascended pretty darn quickly
yellow: RN
red: Android
blue: iOS

!!! 

# Claims to fame:

- Native apps written in JavaScript
- Use React --> "data flows down"
- No waiting for compilation

note: what did it have to offer?

!!!

# Solved Problems

### 1) Code push

### 2) Shared codebase

!!!

<img src="img/sounds-good-meme.jpg" alt="Sounds good meme" style="width: 700px;"/>

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->

# React In Practice

!!!

# Caveat:
### Theory != Practice

note: going to try to differentiate the theory of react from what it's like in practice
React will continue to get better and approach it's theoretical optimum, but isn't there yet

!!!

# Pros

!!!

# Code Push

> CodePush is a cloud service that enables Cordova and 
React Native developers to deploy mobile app updates directly 
to their users’ devices.

!!!

# Code Push

- Works as a central repo
- Can push updates to JS, HTML, CSS, etc
- App will query for updates using SDK

!!!

# Code Push Benefits

Allows for on the fly:
- fixing of bugs
- feature tweaks
- A/B testing

note: Pinterest deploys react 2x a day, iOS only once every two weeks

!!!

# Code Push Limitations:
- data usage
- needs to be downloaded
	-- tricky sequencing
- might run afoul of Apple
- more complicated build processes

!!! 

# Important Note on Hot Code

- Google: ¯\\_(ツ)_/¯
- Apple: Adheres to TOS, but fuzzy

!!!

"3.3.2 An Application may not download or install executable code. Interpreted code may only be used in an Application if all scripts, code and interpreters are packaged in the Application and not downloaded. The only exception to the foregoing is scripts and code downloaded and run by Apple's built-in WebKit framework, provided that such scripts and code do not change the primary purpose of the Application by providing features or functionality that are inconsistent with the intended and advertised purpose of the Application as submitted to the App Store."

!!! 

> "... provided that such scripts and code do not change the primary purpose of the Application..."

!!!

- Use your best judgement
- Hope that it matches Apple's

!!!

# Summary:

### Using React Native gives you Code Push.

### Code Push gives you design velocity and hot fixes.

!!!

Other Benefits of React Native:


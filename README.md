# threex.gamepadtools.js
It contains various tools to handle gamepads e.g. button touchStart/touchEnd event on gamepad buttons
or gesture recognition on gamepad trackpad.
In virtual reality, controllers tends to have trackpads e.g. HTC Vive or Daydream controllers.
threex.gamepadtool.js is able to recognize swipe gesture on trackpad 
and even the cute [1$ unistroke recognizer](http://depts.washington.edu/aimgroup/proj/dollar).

Thus you will be able to easily recognize a 'swipe left' on your daydream controller,
or a 'delete gesture' on your HTC Vive.

# Start with the Example - [source](https://github.com/jeromeetienne/threex.gamepadtools.js/blob/master/examples/basic.html)
Checkout the [basic.html example](https://github.com/jeromeetienne/threex.gamepadtools.js/blob/master/examples/basic.html). 
It is a simple, hopefully educational, example
on how to use those three.js extensions. btw nothing here is three.js specific 
you can easily use those extensions with another 3d engine.

# threex.gamepadsignals.js - [source](https://github.com/jeromeetienne/threex.gamepadtools.js/blob/master/src/threex.gamepadsignals.js)
It monitors the gamepad and notify signals.
It will notifies a **touchStart event** when a button of the gamepad is just started to be pressed,
and notifies a **touchEnd event** when the button is released.
Gamepads are exposed to the browser via the [gamepad API](https://developer.mozilla.org/en-US/docs/Web/API/Gamepad_API).


```javascript
// create gamepadSignals
var gamepadSignals = new THREEx.GamepadSignals()

// periodically update gamepadSignals with the gamepad states
onRenderFcts.push(function(){
	// get current gamepad state
	var gamepad = navigator.getGamepads()[0]
	if( gamepad === null )	return
	// update gamepadSignals with the current gamepad
	gamepadSignals.update(gamepad)		
})

// listen to gamepadSignals touchStart
gamepadSignals.signals.touchStart.add(function(buttonIndex, gamepad){	
	console.log('button', buttonIndex, 'just received touchStart')
})

// listen to gamepadSignals touchEnd
gamepadSignals.signals.touchEnd.add(function(buttonIndex, gamepad){	
	console.log('button', buttonIndex, 'just received touchEnd')
})
```



# threex.gamepadonedollar.js - [source](https://github.com/jeromeetienne/threex.gamepadtools.js/blob/master/src/threex.gamepadonedollar.js)
It monitors the gamepad trackpad and recognize gestures on it. The recognize gestures
are the one from the famous [$1 Unistroke Recognizer](http://depts.washington.edu/aimgroup/proj/dollar/).
It relies on the javascript implementation [onedollar-unistroke-coffee](https://github.com/nok/onedollar-unistroke-coffee)
by [nok](http://nok.onl)


```javascript
// create gamepadOneDollar
var gamepadOneDollar = new THREEx.GamepadOneDollar(gamepadSignals)

// periodically update gamepadOneDollar with the gamepad states
onRenderFcts.push(function(){
	// get current gamepad state
	var gamepad = navigator.getGamepads()[0]
	if( gamepad === null )	return
	// update gamepadOneDollar with the current gamepad
	gamepadOneDollar.update(gamepad)		
})	

// listen to recognizer result
gamepadOneDollar.signals.result.add(function(result){
	console.log('recognize gesture', result.name)
})
```

# threex.gamepadswipe.js - [source](https://github.com/jeromeetienne/threex.gamepadtools.js/blob/master/src/threex.gamepadswipe.js)

It monitor the trackpads of a gamepad and recognize swipes on it.

```javascript
// create gamepadSwipe
var gamepadSwipe = new THREEx.GamepadSwipe(gamepadSignals)

// listen to gamepadSwipe signals 
gamepadSwipe.signals.swipe.add(function(direction){
	console.log('notified signals swipe direction', direction)
})
```

# swipedetector.js - [source](https://github.com/jeromeetienne/threex.gamepadtools.js/blob/master/src/swipedetector.js):
it is a swipe detector using a similar API as the 1$ Unistroke Recognizer.
It is used by threex.gamepadswipe.js to detect swipes on a gamepad trackpad (e.g. the one on HTCVive controller, or on Daydream controller).

```javascript
// create the swipe detector
var swipeDetector = new SwipeDetector()

// then you should update the detector with the touch of your trackpad.
// - swipeDetector.start(x,y) when you start touching the trackpad - touchStart
// - swipeDetector.update(x,y) when you move the touch while remaining touched - touchMove
// - swipeDetector.end(x,y) when you stop touching the trackpad - touchEnd
```

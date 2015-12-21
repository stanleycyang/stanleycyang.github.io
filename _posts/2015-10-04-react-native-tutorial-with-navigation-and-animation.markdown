---
layout: post
title: "Tutorial: Handcrafting an iOS Application with React Native (and lots of love)"
redirect_to:
  - https://www.stanleycyang.com/tutorials/handcrafting-an-ios-application-in-react-native-with-love
date: 2015-10-04 10:02:30
categories: technology reactjs native ios
comments: true
---

###Tutorial Objectives:

- Provide an overview of **React Native** and why it is amazing
- Teach the fundamentals of **Navigator** in a React Native iOS application
- Break down the **Animated API** and perform amazing animations
- Tap into **gesture detection** with PanResponder in iOS

<br />

> **Note**: If you want to skip the tutorial and jump straight to the code, the completed [source code](https://github.com/stanleycyang/rn_animation_tutorial) is available on my [GitHub](https://github.com/stanleycyang) [here](https://github.com/stanleycyang/rn_animation_tutorial).

<br />

##React Native

[React Native](https://facebook.github.io/react-native/) is a framework created by [Facebook](https://www.facebook.com) which allows programmers build **native** iOS application with JavaScript. 

If you take a quick peek at the source code of React Native, you will notice that a lot of it is actually written in Objective-C! This is great because:

- With React Native, your application business logic is written and run in JavaScript while keeping the UI (User Interface) completely native. Therefore, there is no compromises typically associated with HTML5 UI

- React is a completely new approach to building extremely performant web applications with emphasis on the **V** in [**MVC**](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)

React Native aims to bring the power of React to mobile app development. Once you learn the basics, you can create magnificient web / mobile applications that users will love.

<br />

##Tutorial Scope

We will be building an iOS application in this tutorial. However, once you learn the concepts you can transfer it into making an Android application very Swift-ly (ha-ha).

<br />

##Getting Started

React Native is completely [open-source](https://github.com/facebook/react-native). Feel free to clone it or fork it!

You will be using your CLI (Command Line Interface) to create your project. 

<br />

####Basic requirements
<hr />
<br />

- [NodeJS](https://nodejs.org/en/)
- [Homebrew](http://brew.sh/)
- XCode

*If you don't have the requirements already installed, make sure you go to their homepage and install them before going on.*

<br />

####Installation
<hr />
<br />

If you do not have node yet, let's use Homebrew to install Node.js.

	$ brew install node

Then, use Homebrew to install [watchman](https://facebook.github.io/watchman/), a file watcher from Facebook. It will watch your code for changes and rebuild accordingly.

	$ brew install watchman

Once you have those dependencies installed, let's use npm (node package manager) to install the React Native CLI tool.

	$ npm install -g react-native-cli

This tool will help us generate the iOS /Android application and much more!

<br />

####Creating the basic iOS app
<hr />
<br />

Now, lets navigate to the folder you want to store your React Native iOS application and use the React Native CLI tool to generate the new project!

	$ react-native init ReactNativeTutorial

This command will give you the starter code (in a folder called ReactNativeTutorial) you will need to build and run a basic iOS application.

<br />

####Running the application
<hr />
<br />

When you look at your file structure, you should have something that looks like the following:

- **android/** (*everything android-related*)
- **index.android.js** (*the basis of the React Native code for your android application*)
- **index.ios.js** (*the basis of the React Native code for your iOS application*)
- **ios/** (*everything iOS-related*)
- **node_modules/** (*all your dependencies, installed locally*)
- **package.json** (*a JSON file with basic information about your application*)

Open up the file **ReactNativeTutorial.xcodeproj** within the **ios** folder then build and run. Your simulator will start up and display the following:

<br />

![iOS Simulator]({{ site.url }}/assets/react-native-tutorial-with-navigation-and-animation/React-Starter.png)

A separate terminal window will also pop up, displaying:

	 ===============================================================
	 |  Running packager on port 8081.       
	 |  Keep this packager running while developing on any JS         
	 |  projects. Feel free to close this tab and run your own      
	 |  packager instance if you prefer.                              
	 |                                                              
	 |     https://github.com/facebook/react-native                 
	 |                                                              
	 ===============================================================
	 
	Looking for JS files in
	   /YOUR-JS-FILE
	 
	React packager ready.


The packager lives on a Node server on 8081, and will run in the background and host your JS bundle to be used within the application.

<br />

###Going Native, React Native
<br />

####Starting point:
<hr />
<br />

Let's begin writing some JavaScript code! We'll start in `index.ios.js` and lay the groundwork for our application. Remember, `index.ios.js` is essential the entry point for our iOS React Native application.

Remove the boilerplate code in `index.ios.js` and add this to the beginning of the file:

<br />

{% highlight js %}
// React Native module
const React = require('react-native');

// Dimensions module
const Dimensions = require('Dimensions');
{% endhighlight %}

This will load the `react-native` module and assign it to React. React uses the same loading technology as Node.js (with the` require`).

Beneath those lines, add the following code snippet:

<br />

{% highlight js %}
// Using the Dimensions module, grab the current screen's width and height

const {
  width,
  height
} = Dimensions.get('window');

// Grab the React properties we will be using in this tutorial 

const {
  AppRegistry,
  StyleSheet,
  Component,
  Text,
  View,
  Navigator,
  TouchableHighlight
} = React;
{% endhighlight %}
<br />

This is syntax is called the **destructuring syntax**, which lets us extract multiple object properties in a single line. Now we no longer have to prefix them with `React` (ie. `React.AppRegistry` or `React.Component`). 

Pretty sweet, right?

>**Note:** Notice how I am using the `const` keyword? This means I am creating a **read-only** reference to a value. It means that the variable identifier now cannot be reassigned. Also, it results in [better performance](http://stackoverflow.com/questions/21237105/const-in-javascript-when-to-use-it-and-is-it-necessary).

<br />

####Styling basics
<hr />
<br />

Now, let's add the following lines into the code:

<br />

{% highlight js %}
// Use the StyleSheet property to add styling to our code
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
    color: 'white'
  },
});
{% endhighlight %}
<br />

This looks like regular CSS, with camel-casing! To use the styles, you simply call it like this: `styles.container` or `styles.welcome`.

> **Note**: React Native uses [css-layout](https://github.com/facebook/css-layout) library, which implements the flexbox standard transpiled into C for iOS and Java for Android. For more about flexbox, check out [flexbox.io](www.flexbox.io).

<br />

####In comes the Navigator
<hr />
<br />

**Navigator** is an awesome API to transition between different scenes in your React Native application. 

To make it work, we have to 1) provide the route objects to identify each scene, and 2) a renderScene method that the navigator uses to render the scene for the given route.

> **Note**: Think of the Navigator as a stack. It follows **LIFO** (Last in, First out). All we need to navigate around the app later is simply the `push` and `pop` methods.

Lets add this code after the styling for our current project:

<br />

{% highlight js %}
// Navigator configuration
const BaseConfig = Navigator.SceneConfigs.FloatFromRight

const CustomLeftToRightGesture = Object.assign({}, BaseConfig.gestures.pop, {
  // Make it snap back really quickly after canceling pop
  snapVelocity: 8,
  // Make it so we can drag anywhere on the screen
  edgeHitWidth: width
});

const CustomSceneConfig = Object.assign({}, BaseConfig, {
  // A tightly wound spring will make this transition fast
  springTension: 100,
  springFriction: 1,
  // Use our custom gesture defined above
  gestures: {
    pop: CustomLeftToRightGesture
  }
});
{% endhighlight %}
<br />

This bit of code may seem very confusing, but it's actually pretty simple. 

Essentially, we tell the Navigator API that we want to configure the scene to come in from the right side with `Navigator.SceneConfigs.FloatFromRight`.

Then, we can customize additional properties such as where a user is allowed to drag from (`edgeHitWidth`, which we set to the whole screen) and how quickly it snaps (`snapVelocity`).

Then, we plug it all into `CustomSceneConfig` which we will use later into our code.

>**Note**: What is **Object.assign**? It is a new method in ES2015 which allows us to copy the values of all enumerable properties from one or more objects into a target object. For more, here is the [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign).

<br />

####Writing the first React component
<hr />
<br />

Let's write the main component of this application! Write the following code beneath the animation configuration.

<br />

{% highlight js %}

// Use an ES2015 class to create components

class ReactNativeTutorial extends Component {

  // A simple switch statement to allow our router to work
  _renderScene(route, navigator) {
    
  }

	// Plug in the animations
  _configureScene(route) {
    return CustomSceneConfig;
  }

  render() {
    return (
      <Navigator
        initialRoute={{id: 1}}
        renderScene={this._renderScene}
        configureScene={this._configureScene} />
    )
  }
}
{% endhighlight %}
<br />

We will be using the ES2015 `class` to write React components. This is going to be the root component. All the other components will flow through this one. 

We will inherit from Components with the help of the keyword `extends` and let React know that this will be a React Native component.

At minimum, all React component must have the render method which returns [JSX](https://facebook.github.io/react/docs/jsx-in-depth.html) for what the component will look like.

Our Navigator component will set the initial route to the first page (`id 1`). Then we will use **_renderScene** to determine which page to show. And lastly, we will use **_configureScene** to apply the configurations we set previously to our application.

>**Note**: We currently do not have any scenes yet, so our app will remain blank for now.

<br />

####AppRegistry
<hr />
<br />
Every React Native app needs an AppRegistry. The root components should register themselves so the native system can load the bundle for the app and run it. Let's write the following code to the end of `index.ios.js`:

<br />
{% highlight js %}
AppRegistry.registerComponent('ReactNativeTutorial', () => ReactNativeTutorial);
{% endhighlight %}
<br />

Our root component is now registered! The next step is building out the 3 pages we will build the animations on.

<br />

####Scenes
<hr />
<br />

Next, in your terminal, run the following commands:
<br />

	$ mkdir components
	$ touch components/PageOne.js components/PageTwo.js components/PageThree.js

<br />
At the top of `index.ios.js`, beneath the import of React Native and Dimensions, add these lines of code:

<br />
{% highlight js %}

// Bring in components
const PageOne = require('./components/PageOne')
const PageTwo = require('./components/PageTwo')
const PageThree = require('./components/PageThree')

{% endhighlight %}
<br />

Now that we have connected the pages, lets complete the `_renderScene` method:

<br />
{% highlight js %}

_renderScene(route, navigator) {
	// Add in a switch statement
    switch (route.id) {
      case 1:
        return <PageOne navigator={navigator} />
      case 2:
        return <PageTwo navigator={navigator} />
      case 3:
        return <PageThree navigator={navigator} />
    }
  }

{% endhighlight %}
<br />

We now check what the route id is, then redirect the user to the correct page.

At this point in time, we can now build out our three navigation pages and dive head-first into React Native Animations.

###Layout.js

In your terminal, write the following commands:

	$ mkdir stylesheets
	$ touch stylesheets/layout.js

The `layout.js` file will be the styling you will share throughout this application. 

<br />

Within the `layout.js`, add the following lines of code:

{% highlight js %}
const React = require('react-native')
const { StyleSheet } = React;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
    color: 'white'
  },
  button: {
    paddingVertical: 10,
    paddingHorizontal: 20,
    backgroundColor: 'black'
  },
  backButton: {
    paddingVertical: 10,
    paddingHorizontal: 65,
    backgroundColor: 'red'
  }
})

module.exports = styles
{% endhighlight %}

<br />

We can now import this file throughout our children components.

<br />

####PageOne
<hr />
<br />

Add the following code snippet into `components/PageOne.js`:

{% highlight js %}
// Bring in React Native
const React = require('react-native');
// Bring in our layout styling
const styles = require('../stylesheets/layout');


// Destructured components
const {
  Component,
  Text,
  View,
  TouchableHighlight
} = React;

class PageOne extends Component {
  constructor(props) {
    super(props);
  }
  _handlePress() {
    // Go to page 2, simply by pushing!
    this.props.navigator.push({id: 2});
  }

  _goBack() {
    // Go to the previous page, by simply popping!
    this.props.navigator.pop();
  }

  render() {
    return (
      <View style={[styles.container, {backgroundColor: 'green'}]}>
        <Text style={styles.welcome}>Page one</Text>
        <TouchableHighlight onPress={this._handlePress.bind(this)} style={styles.button}>
          <Text style={styles.welcome}>Go to page two</Text>
        </TouchableHighlight>

        <TouchableHighlight onPress={this._goBack.bind(this)} style={styles.backButton}>
          <Text style={styles.welcome}>Back</Text>
        </TouchableHighlight>
      </View>
    )
  }
}

// Export this component to be used by the parent component
module.exports = PageOne;
{% endhighlight %}
<br />

- Again, we bring in all the parts we will need with the `require` and `destructuring` syntax.

- Within the PageOne class, we now have a `constructor` method. The constructor method simply runs when the class is initialized. It takes in `props` as an argument, which we will use in the `super(props)` call. What the `super(props)` call will do is simply allow us to reference the props as `this.props`. Since we flowed `navigator` into this component from our parent, we can now reference the `navigator` as `this.props.navigator`.

- Now onto the the `_handlePress` and `_goBack` methods. They simply move forward (push) and go to the previous page (pop). Simple, right?

- We will now tie the `_handlePress` method to the first `TouchableHighlight` component, so whenever a user clicks it we will go to page two.

- We will also tie the `_goBack` method to the second `TouchableHighlight` component, so it will take a user to the previous page whenever its clicked.

>**Note**: In ES2015, the context is not automatically bound to the class instance methods. Therefore, we have to manually bind it (ie. `this._handlePress.bind(this)`).

Page two will follow a similar format. You can figure it out, right? ;-)

<br />

####PageTwo
<hr />
<br />

Add the following code snippet into `components/PageTwo.js`:

{% highlight js %}

const React = require('react-native')
const styles = require('../stylesheets/layout')


// Destructured components
const {
  Component,
  Text,
  View,
  TouchableHighlight
} = React;

class PageTwo extends Component {
  constructor(props) {
    super(props);
  }
  _handlePress() {
    this.props.navigator.push({id :3})
  }
  _goBack() {
    this.props.navigator.pop()
  }

  render() {
    return (
      <View style={[styles.container, {backgroundColor: 'blue'}]}>
        <Text style={styles.welcome}>Page two</Text>
        {% raw %}      
        <TouchableHighlight onPress={this._handlePress.bind(this)} style={{paddingVertical: 10, paddingHorizontal: 20, backgroundColor: 'black'}}>
        {% endraw %}
            <Text style={styles.welcome}>Go to last page</Text>
        </TouchableHighlight>
        <TouchableHighlight onPress={this._goBack.bind(this)} style={styles.backButton}>
          <Text style={styles.welcome}>Back</Text>
        </TouchableHighlight>
      </View>
    )
  }
}

// export the component to be used by the parent
module.exports = PageTwo


{% endhighlight %}

<br />

####PageThree
<hr />
<br />

Add the following code snippet into `components/PageThree.js`:

{% highlight js %}

const React = require('react-native')
const styles = require('../stylesheets/layout')


// Destructured components
const {
  Component,
  Text,
  View,
  TouchableHighlight
} = React;

class PageThree extends Component {
  constructor(props) {
    super(props);
  }
  _handlePress() {
    this.props.navigator.push({id :1})
  }
  _goBack() {
    this.props.navigator.pop()
  }

  render() {
    return (
      <View style={[styles.container, {backgroundColor: 'purple'}]}>
        <Text style={styles.welcome}>Page three</Text>
        {% raw %}
        <TouchableHighlight onPress={this._handlePress.bind(this)} style={{paddingVertical: 10, paddingHorizontal: 20, backgroundColor: 'black'}}>{% endraw %}
            <Text style={styles.welcome}>Go to page one</Text>
          </View>
        </TouchableHighlight>
        <TouchableHighlight onPress={this._goBack.bind(this)} style={styles.backButton}>
          <Text style={styles.welcome}>Back</Text>
        </TouchableHighlight>
      </View>
    )
  }
}

// export this component to be used by the parent
module.exports = PageThree;


{% endhighlight %}

<br />

At this point in time, you will now have three pages interconnected by Navigator. Also, notice that you can also swipe right to go to the previous page. All that is thanks to the `CustomSceneConfig` we did early on!

With the `Navigator` portion now complete, let's dive into the part you have been waiting for -- the `Animated` API!

<br />

###Animated API
<hr />
<br />

Our goal will be to fill out each page with a different type of animation. Since we have 3 pages, we will be creating 3 different animations.

Let's create the files for the animation components with the help of our terminal:

	$ touch components/PlayGround.js 
	$ touch components/Gestures.js
	$ touch components/PanResponder.js

The animations we will be creating are as follows:

1. In `PlayGround.js`, we will create an expanding circle effect that will cover the entire screen.
2. In `Gestures.js`, we will create a button that allows the user to press and hold until it is filled.
3. In `PanResponder.js`, we will create a square that is draggable aroud the page, but will return to its initial value once the user lets go.

<br />

####PlayGround.js
<hr />
<br />

Write this code into `PlayGround.js`:

{% highlight js %}

const React = require('react-native')
const Dimensions = require('Dimensions')

// Destructured syntax, make sure you include the Animated and Easing API
const {
  Component,
  StyleSheet,
  Animated,
  Text,
  Easing,
  TouchableOpacity,
  View
} = React;

// Again, get the width and height of the page
const {
  width,
  height
} = Dimensions.get('window')

// Create the dimension for the circle (width: 40, height: 40, border radius: 20)
const CIRCLE_DIMENSIONS = 40

// Set the configuration for our animation timing
const TIMING_CONFIG = {
  duration: 300, // Last for 300 ms
  delay: 0, // 0 ms delay
  easing: Easing.in(Easing.ease) //  Use the Easing API here
}

// Create the PlayGround component
class Playground extends Component {
  constructor (props) {
    // Flow down the props
    super(props)
    
    // Set the initial state of the PlayGround
    this.state = {
      previewOpen: false,
      w: new Animated.Value(CIRCLE_DIMENSIONS),
      h: new Animated.Value(CIRCLE_DIMENSIONS),
      br: new Animated.Value(CIRCLE_DIMENSIONS/2)
    }
	
	 // Again, we have to bind to provide the context (this) to the _triggerAnimation function
    this._triggerAnimation = this._triggerAnimation.bind(this)

  }

	// @params: w1, h1, br1, w2, h2, br2 : Integer
  _triggerAnimation (w1, h1, br1, w2, h2, br2) {
    // Set preview state
    this.setState({
      previewOpen: !this.state.previewOpen
    })

	 // Start the sequence. Notice how it takes an array of animations. Everything in here will happen sequentially (FIFO)
    Animated.sequence([
      // Parallel also takes an array of animations. These animations will happen in parallel
      Animated.parallel([
        // From the initial width (40), we will move it to the new width
        Animated.timing(this.state.w, {
          ...TIMING_CONFIG,
          toValue: w1
        }),
        // From the initial height (40), we will move it to the new height
        Animated.timing(this.state.h, {
          ...TIMING_CONFIG,
          toValue: h1
        }),
        // From the initial borderRadius (20), we will move it to the new borderRadius
        Animated.timing(this.state.br, {
          ...TIMING_CONFIG,
          toValue: br1
        })
      ]),
      Animated.parallel([
        Animated.timing(this.state.w, {
          ...TIMING_CONFIG,
          toValue: w2
        }),
        Animated.timing(this.state.h, {
          ...TIMING_CONFIG,
          toValue: h2
        }),
        Animated.timing(this.state.br, {
          ...TIMING_CONFIG,
          toValue: br2
        })
      ])
    ]).start()
  }
  // This is a part of the component lifecycle. When the component mounts onto the page, it'll run this algorithm
  componentDidMount () {
    this._triggerAnimation(width, width, width/2, width, height, 0)
  }

  // When the close button is clicked, run this closing animation
  _closePreview () {
    this._triggerAnimation(width, width, width/2, CIRCLE_DIMENSIONS, CIRCLE_DIMENSIONS, CIRCLE_DIMENSIONS/2)
  }

  // Returns the current style of the element. We will plug this method directly into the element
  getStyle () {
    return [
      styles.circle,
      {
        width: this.state.w,
        height: this.state.h,
        borderRadius: this.state.br
      }
    ]
  }
  // The one essential function. This will render the view
  render () {
    let CloseButton

    if (this.state.previewOpen) {
       CloseButton =
          <TouchableOpacity onPress={this._closePreview.bind(this)} style={styles.closeButton}>
            <Text>Close me</Text>
          </TouchableOpacity>
    }

    return (
      <View style={styles.container}>
        <Animated.View
          style={this.getStyle()} />
          { CloseButton }
      </View>
    )
  }
}

// Nothing new, provide styles for the component
const styles = {
  container: {
    flex: 1
  },
  circle: {
    backgroundColor: 'darkgreen'
  },
  closeButton: {
    paddingVertical: 10,
    paddingHorizontal: 65,
    backgroundColor: 'red',
    position: 'absolute',
    top: 20,
    left: 0,
  }
}

// Export it for use in the PageOne Component
module.exports = Playground;


{% endhighlight %}

<br />

React Native provides a great API for animations call [`Animated`](https://facebook.github.io/react-native/docs/animations.html). 

This allows us to run a simple animation (`Animated.timing`, `Animated.decay`, `Animated.spring`). It also lets us chain animations in sequence (`Animated.sequence`) or in parallel (`Animated.parallel`). All we have to do is provide an `Animated.Value`, then tie that `Animated.Value` into the component we want to animate! Easy, right?

To see it in action, all we have to do is import it into the `PageOne` component, as follows (Beneath the react and styles import in `PageOne`:

{% highlight js %}
...
// Require component here
const Playground = require('./PlayGround');
...
class PageOne extends Component {
  ...

  render() {
    return (
      <View style={[styles.container, {backgroundColor: 'green'}]}>
        <Playground /> // Add component here
		 ...	
      </View>
    )
  }
}
...
{% endhighlight %}

Now run the app, and on `PageOne`, you will see our screen being turn to a dark shade of green!:

![Green Screen]({{ site.url }}/assets/react-native-tutorial-with-navigation-and-animation/Green-Screen.png)

Awesome! Let's go on to build the animated button.

<br />

####Gestures.js
<hr />
<br />

Write this code into `Gestures.js`:

{% highlight js %}
const React = require('react-native')

// Bring in the Animated API
const {
  StyleSheet,
  Animated,
  Component,
  View,
  Text,
  TouchableWithoutFeedback
}  = React

// Set a timing for the button press
const ACTION_TIMER = 400
// Set the colors you want the button to turn into
const COLORS = ['rgb(0,0,255)', 'rgb(111,235,62)']

// Create the Gestures component
class Gestures extends Component {
  constructor (props) {
    super(props)
    this.state = {
      pressAction: new Animated.Value(0), // Default the value to 0
      textComplete: '',
      buttonWidth: 0,
      buttonHeight: 0
    }
    // Bind the context to animationActionComplete
    this.animationActionComplete = this.animationActionComplete.bind(this)
  }

  // When the button is pressed, run this code
  handlePressIn () {
    // We will run the animation until it turns to 1
    Animated.timing(this.state.pressAction, {
      duration: ACTION_TIMER,
      toValue: 1
    }).start(this.animationActionComplete)
    // After the animation finishes, run the animationActionComplete method
  }

  // When the user doesn't hold on to the button, turn the value back towards 0
  handlePressOut () {
    Animated.timing(this.state.pressAction, {
      duration: this.state.pressAction.__getAnimatedValue() * ACTION_TIMER,
      toValue: 0
    }).start(this.animationActionComplete)
  }

  // Shows a customized message
  animationActionComplete () {
    let message = ''
    if (this.state.pressAction.__getAnimatedValue() === 1) {
      message = "Thank you for holding :-)"
    } else {
      message = 'Press and hold the button!'
    }

    this.setState({
      textComplete: message
    })
  }
  // Get the width & height of the button
  getButtonWidthLayout (e) {
    this.setState({
      buttonWidth: e.nativeEvent.layout.width - 6,
      buttonHeight: e.nativeEvent.layout.height - 6
    })
  }

  // Like the getStyle method in the previous example, we will return the style of the button
  getProgressStyles () {
    // Map the range of our inputs to the width
    let width = this.state.pressAction.interpolate({
      inputRange: [0, 1],
      outputRange: [0, this.state.buttonWidth]
    })
    // Map the range of inputs to the colors
    let bgColor = this.state.pressAction.interpolate({
      inputRange: [0, 1],
      outputRange: COLORS
    })

    return {
      width: width,
      height: this.state.buttonHeight,
      backgroundColor: bgColor
    }
  }

  render () {
    return (
      <View style={styles.container}>
        // When users press or unpress this, run the methods
        <TouchableWithoutFeedback
          onPressIn={this.handlePressIn.bind(this)}
          onPressOut={this.handlePressOut.bind(this)}
          >
          // A static button, which will provide the width when rendered
          <View style={styles.button} onLayout={this.getButtonWidthLayout.bind(this)}>
            <Animated.View style={[styles.bgFill, this.getProgressStyles()]} />
            <Text style={styles.text}>Press me!</Text>
          </View>
        </TouchableWithoutFeedback>
        <View>
          // Show the customized message
          <Text>{this.state.textComplete}</Text>
        </View>
      </View>
    )
  }

}

// Provide basic styles
const styles = StyleSheet.create({
  container: {
    flex: 1,
    flexDirection: 'column',
    alignItems: 'center',
    justifyContent: 'center'
  },
  button: {
    padding: 10,
    borderWidth: 3,
    borderColor: '#111'
  },
  text: {
    backgroundColor: 'transparent',
    color: '#111'
  },
  bgFill: {
    position: 'absolute',
    top: 0,
    left: 0
  }
})

module.exports = Gestures;
{% endhighlight %}

<br />

In this example, we are also tying the Animated library into play. We basically animate a button within a static container so it grows until it hits 1. Also, we got to use the `Animated.interpolate` method, which allows us to map a range to an output range. At this point, we can now bring this component into `PageTwo`.

Write the following code into `PageTwo.js`:

{% highlight js %}

const React = require('react-native')
const styles = require('../stylesheets/layout')

// Import components
const Gestures = require('./Gestures')

...
class PageTwo extends Component {
  ...
  render() {
    return (
      <View style={[styles.container, {backgroundColor: 'blue'}]}>
        <Gestures />
        ...
      </View>
    )
  }
}

module.exports = PageTwo


{% endhighlight %}

Save the code and run your application. You should now have a button that you can press and it will load in a color!

![Loading Button]({{ site.url }}/assets/react-native-tutorial-with-navigation-and-animation/Button.png)

Excellent! Now we will build our last component, the `PanResponder`.

<br />

####PanResponder.js
<hr />
<br />

Let's take a minute and explain what the [PanResponder](https://facebook.github.io/react-native/docs/panresponder.html) API is all about.

It is a library in React Native that recognizes simple touch gestures, and allows us to track how a user is interacting with the application. 

Today, we will use this library to allow us to build a draggable square within this application. If you aren't excited now, you should be.

Write this code into `PanResponder.js`:

{% highlight js %}
const React = require('react-native')

// Remember to load in the PanResponder Library!
const {
  Component,
  StyleSheet,
  Animated,
  Text,
  View,
  PanResponder,
  TouchableWithoutFeedback
} = React;

// Set the square dimensions to be 40 by 40
const SQUARE_DIMENSIONS = 40

// Create the PanResponderAPI Component
class PanResponderAPI extends Component {
  constructor (props) {
    super(props)

	 // We will be dealing with the X and Y coordinate of the square, so set the value to Animated.ValueXY
    this.state = {
      pan: new Animated.ValueXY()
    }

    // Bind functions
    this.getStyle = this.getStyle.bind(this)
  }

  // Before the component is mounted to the page, lets work with our PanResponder Library
  componentWillMount () {
    // Ask to be the responder for our app. Make sure we say true
    this._panResponder = PanResponder.create({
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponderCapture: (evt, gestureState) => true,
      // The gesture has now started. Show the offset on the page!
      onPanResponderGrant: (evt, gestureState) => {
        // The guesture has started. Grab the values of where the square is moving to
        // what is happening!
        this.state.pan.setOffset({
          x: this.state.pan.x.__getAnimatedValue(),
          y: this.state.pan.y.__getAnimatedValue()
        })
        // Default value of the square
        this.state.pan.setValue({
          x: 0,
          y: 0
        })

        // gestureState.{x,y}0 will be set to zero now
      },
      // Input the coordinates
      onPanResponderMove: Animated.event([
        null,
        {
          dx: this.state.pan.x,
          dy: this.state.pan.y
        }
      ]),
      onPanResponderTerminationRequest: (evt, gestureState) => true,
      onPanResponderRelease: (evt, gestureState) => {
        // The user has released all touches while this view is the
        // responder. This typically means a gesture has succeeded
        Animated.spring(this.state.pan, {
          toValue: 0
        }).start()
      }
    })
  }
  // This method will go into the animated component
  getStyle () {
    return [
      styles.square,
      {
        transform: [
          {
            translateX: this.state.pan.x
          },
          {
            translateY: this.state.pan.y
          }
        ]
      }
    ]
  }

  render () {
    return (
      <View style={styles.container}>
        // Use a spread operator to put in our panHandlers and tie it with the component
        <Animated.View style={this.getStyle()} {...this._panResponder.panHandlers} />
      </View>
    )
  }
}

// Basic styling
const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center'
  },
  square: {
    width: SQUARE_DIMENSIONS,
    height: SQUARE_DIMENSIONS,
    backgroundColor: 'yellow'
  }
})

// Export the component to be used in PageThree
module.exports = PanResponderAPI
{% endhighlight %}

<br />

We will set `this._panResponder` before the component has loaded, and this will allow us to grab the user interaction with the component. We grab the coordinates of where the user is moving to and then set our square to those coodinates.

At this point, we can now load this code into `PageThree`! Let's do the following:

{% highlight js %}

const React = require('react-native')
const styles = require('../stylesheets/layout')

// Import component
const PanResponder = require('./PanResponder')

...

  render() {
    return (
      <View style={[styles.container, {backgroundColor: 'purple'}]}>
        <PanResponder />
        ...
      </View>
    )
  }
}

...
{% endhighlight %}

Now when we save and run the code, we should see the following: 

![Draggable Square]({{ site.url }}/assets/react-native-tutorial-with-navigation-and-animation/Square.png)

... And at this point, our application is complete! Voila, a fully navigatable and animated iOS application!


##What have we learned?

<br />

>- React Native is an awesome tool to craft native applications.
- The Navigator is simply a stack that allows us to `push` and `pop` to the different pages.
- Animating components in React Native using the `Animated` and the `PanResponder` APIs.

<br />

I hope this tutorial got you excited about building native application with React Native. If you have any problems or suggestions, give me a buzz either here or on [Twitter](https://twitter.com/stanleycyang).

You can check out the finished version on [GitHub](https://github.com/stanleycyang/rn_animation_tutorial), and find out more about React Native [here](https://facebook.github.io/react-native/).

<br />

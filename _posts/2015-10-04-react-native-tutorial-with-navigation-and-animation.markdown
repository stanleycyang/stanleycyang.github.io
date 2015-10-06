---
layout: post
title: "React Native Tutorial with Navigation and Animation"
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

At this point in time, we can now build out our three pages, and dive head first into React Native Animations.

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
        <TouchableHighlight onPress={this._handlePress.bind(this)}>
          <View style={{paddingVertical: 10, paddingHorizontal: 20, backgroundColor: 'black'}}>
            <Text style={styles.welcome}>Go to last page</Text>
          </View>
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
        <TouchableHighlight onPress={this._handlePress.bind(this)}>
          <View style={{paddingVertical: 10, paddingHorizontal: 20, backgroundColor: 'black'}}>
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
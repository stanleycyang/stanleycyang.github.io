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

##React Native

[React Native](https://facebook.github.io/react-native/) is a framework created by [Facebook](https://www.facebook.com) that allows programmers build **native** iOS application with JavaScript. 

If you take a quick peek at the source code of React Native, you will notice that a lot of it is actually written in Objective-C! This is great because:

- With React Native, your application business logic is written and run in JavaScript while keeping the UI (User Interface) completely native. Therefore, there is no compromises typically associated with HTML5 UI

- React is a completely new approach to building extremely performant web applications with emphasis on the **V** in [**MVC**](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)

React Native aims to bring the power of React to mobile app development. Once you learn the basics, you can create magnificient web / mobile applications that users will love.

##Tutorial Scope

We will be building an iOS application in this tutorial. However, once you learn the concepts you can transfer it into making an Android application very Swift-ly (ha-ha).

##Getting Started

React Native is completely [open-source](https://github.com/facebook/react-native). Feel free to clone it or fork it!

You will be using your CLI (Command Line Interface) to create your project. 

####Basic requirements

- [NodeJS](https://nodejs.org/en/)
- [Homebrew](http://brew.sh/)
- XCode

*If you don't have the requirements already installed, make sure you go to their homepage and install them before going on.*

####Installation

If you do not have node yet, let's use Homebrew to install Node.js.

	$ brew install node

Then, use Homebrew to install [watchman](https://facebook.github.io/watchman/), a file watcher from Facebook. It will watch your code for changes and rebuild accordingly.

	$ brew install watchman

Once you have those dependencies installed, let's use npm (node package manager) to install the React Native CLI tool.

	$ npm install -g react-native-cli

This tool will help us generate the iOS /Android application and much more!

####Creating the basic iOS app

Now, lets navigate to the folder you want to store your React Native iOS application and use the React Native CLI tool to generate the new project!

	$ react-native init ReactNativeTutorial

This command will give you the starter code (in a folder called ReactNativeTutorial) you will need to build and run a basic iOS application.

####Running the application

When you look at your file structure, you should have something that looks like the following:

- **android/** (*everything android-related*)
- **index.android.js** (*the basis of the React Native code for your android application*)
- **index.ios.js** (*the basis of the React Native code for your iOS application*)
- **ios/** (*everything iOS-related*)
- **node_modules/** (*all your dependencies, installed locally*)
- **package.json** (*a JSON file with basic information about your application*)

Open up the file **ReactNativeTutorial.xcodeproj** within the **ios** folder then build and run. Your simulator will start up and display the following:

![iOS Simulator]({{ site.url }}/assets/react-native-tutorial-with-navigation-and-animation/React-Starter.png)
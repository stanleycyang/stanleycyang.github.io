---
layout: post
title: "How to Start Programming"
date: 2015-08-30 11:33:40
categories: technology life
---

Basic `Person` class in JavaScript (**ES2015 syntax**)

{% highlight js %}
class Person {
	constructor(name) {
		this.name = name;
	}
	helloWorld() {
		console.log(`Hello world! My name is ${this.name}`);
	}
}

var Stanley = new Person('Stanley');

Stanley.helloWorld();
// Prints out "Hello world! My name is Stanley" in browser console	
{% endhighlight %}

*Note: If this doesn't make sense to you yet, don't worry.*

<br>

##Programming is challenging

It takes years to become really comfortable with code and to jump from language to language. It's a matter of practice and confidence. 

Luckily, it is easier to start learning today than any other time in human history, #BecausetheInternet (*[GitHub](https://github.com), [StackOverflow](https://stackoverflow.com), [blogs](http://stanleycyang.github.io)*).

Also, the rise and growth of the career-changing boot camps have made it even easier to learn applicable technical abilities and jump into software development.

***Disclaimer**: I used to teach at **[General Assembly](https://generalassemb.ly/)**, a popular New York based educational startup*.

<br>


##Programming is rewarding

I'm sure we've all heard stories about **[college students with 6-figure salaries out of college](http://www.nytimes.com/2015/07/29/technology/code-academy-as-career-game-changer.html?_r=0)**.

These are not myths. These are the going market rates for software engineers today. 

Besides the money, coding is **addicting**. It is better than the best video game you have ever played. And the best part is that it is applicable in all parts of your daily life.

<br>

##Getting started

JavaScript is the language of the web today (thus making it **[the most popular language in the world](http://githut.info/)**).

![JS Logo]({{ site.url }}/assets/how-to-start-programming/js_logo.png)

Today, I will show you how to manipulate the appearance of the webpage with a bit of JavaScript magic.

`Note: this demo will require Google Chrome`

In your browser, right-click in the current tutorial window and choose the `Inspect Element` option.

This will open up your **Chrome Developer Tools**, a powerful tool for web development.

![Chrome Console]({{ site.url }}/assets/how-to-start-programming/console.png)

Next, click the `Console` tab on the right side.

<br>

Once the console opens up, type this line of JavaScript into the console:

{% highlight js %}
document.body.style.backgroundColor = 'red';
{% endhighlight %}

Once you press enter, notice how the **page color changed**!

![Chrome Console]({{ site.url }}/assets/how-to-start-programming/redbackcolor.png)

####Congratulations! You have written your first line of JavaScript!

<br>

###Continuing your personal development

Here are some great resources for getting started with JavaScript:

- [MDN Guide](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/JavaScript_basics)
- [Code Academy](https://www.codecademy.com/tracks/javascript/resume)
- [Code School](https://www.codeschool.com/paths/javascript)

<br>

###If you enjoyed this blog..

Let me know at <mailto:stanley@stanleycyang.com>

I would love to get you started coding with tips, advices, references. 


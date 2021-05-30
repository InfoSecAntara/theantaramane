---
layout: 
title: PHPObjectInjection
subtitle: LFI to RCE
# gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [WebAppVulnerabilities, Pentesting]
comments: true
---

Hello bug-hunters, today we are gonna talk about PHPObjectInjection and leveraging the power of Reflection to modify the serialized objects and access any arbitrary files from the server, later we will learn how to convert it into a RCE (Remote Code Execution). let the hunt begin;

## What is PHPObjectInjection?

Before we talk about the attack, let's undertsand what is Serialization? I hope you must be havinga an idea around it, lets have a revision,  

Serialization is basically converting a set of data structure or the objects into a stream of bytes to transmit it to a memory, a database, or a file. The main advantage of this technology is to save the state of an object in order to be able to recreate it whenever needed, eg. video games, etc.

**Areas of Risks**

Along with the advantage this technology also has its disadvantages. This vulnerability could occur when a user-supplied input is not appropriately sanitized before being passed to the unserialize() PHP function. Since PHP allows object serialization, a malacious actor could pass ad-hoc serialized strings to a vulnerable unserialize() call, resulting in an arbitrary PHP object(s) injection into the application scope.

In order to successfully exploit a PHP Object Injection vulnerability the following two conditions must be met:

  1. The application must have a class which implements a PHP magic method (such as __wakeup or __destruct) that can be used to carry out malicious attacks, or to start a “POP chain”.
  2. All of the classes used during the attack must be declared when the vulnerable unserialize() is being called, otherwise object autoloading must be supported for such classes.

## Here is a secondary heading

Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .mx-auto.d-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.

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

## Attack Story

During a recent Pen Test I came across an interesting piece of PHP code that could lead to RCE, but the exploitation was bit tricky. After spending some sleepless nights trying to break this code, I identified that both application and system level code execution was possible using the vulnerability.

The vulnerable Code:

    /*<?php
  class HTMLFile
  {
    public $filename='help.html';
    public $bla =  1;
    public function __toString()
    {
        return '<iframe src=' . $this->filename ."></iframe>";
    }
  }
  class IncludeFile
  {
    public $filename = '';
    public function __toString()
    {
        include $this->filename;
        return "<br /><br />";
    }
  }
    if (isset($_GET['file'])){
    $htmlobject = unserialize(base64_decode($_GET['file']));
  } else {
    $htmlobject = new HTMLFile();
  }
  ?> 
    <?php echo $htmlobject; ?>*/

In the above code, a user controlled value can be passed on to a PHP un-serialization function. The application accepts a "filename" which gets called, and the output is then fed to php unserialize module. With the above bug both application level and system level code executions is possible. For exploiting this vulenarability both the conditions are satisfied. 

**Serialization example:**

Serializing a 3 char string array

![] (https://github.com/InfoSecAntara/theantaramane/tree/main/assets/img/sample_serializled_code.png)

**Understanding the serialized string**

![] (https://github.com/InfoSecAntara/theantaramane/tree/main/assets/img/understanding_serialzed.png)


In our case, I found an instance where the "file" parameter was carrying a base64 encoded payload, later identified to be a serialized object as following;

![] (https://github.com/InfoSecAntara/theantaramane/tree/main/assets/img/instance.png)

Deserialized form:

"O:8:"HTMLFile":1:{s:8:"filename";s:9:"help.html";}"

At the response side if you'd see, we have another code which has class named "IncludeFile" a vulnerable function "filename" and a magic function "function __toString()", let's make our own payload

This is our payload; pretty easy to understand, I have just replaced the class name, make sure the string char lengths are exactly matching to that string name as follows;

O:11:"IncludeFile":1:{s:8:"filename";s11:"/etc/passwd";}

This is cool, let's base64 encode; and boomm!! it worked

![](https://github.com/InfoSecAntara/theantaramane/assets/img/etc_passwd.png)

Using the power of reflection 





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

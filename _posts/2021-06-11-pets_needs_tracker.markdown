---
layout: post
title:      "Pets Needs Tracker"
date:       2021-06-11 04:42:17 +0000
permalink:  pets_needs_tracker
---


Creating a pet tracker web application has been a learning experience.  I feel that I learned alot about javascript, in particular about classes.  

I learned that ES6 JavasScript Classes are not built so that they can call eachother. 

This, for example, doesn't work: 

```
class Pet {

createNeedSel()
{ const newNeedSel = new NeedSel() }

}

class NeedSel {
}

```

I also learned that when you try to place a fetch call inside the constructor for a class, it makes it so that the functions extended from that class cease to function properly.

So this doesn't work:

```
class Pet {

constructor(){
   fetch( [insert code here] )
}

}

class Dog extends Pet {}


```

But what can work instead is this:

```
class Pet {

createThis(){
   fetch( [insert code here] )
}


class Dog extends Pet {}


```

There is also a default method "create()" for the ES6 classes which i have yet to read about.  I wonder if this method is built for a fetch request?  Something to investigate for future coding/learning.

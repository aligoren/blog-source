---
layout: post
title: JavaScript Add and Substract Days
tags: [JavaScript, Tips]
---

When I visited StackOverflow, I saw a question.

Briefly, he wants to add days to dates. Firstly I checked his codes. Also, his mathematics was wrong.

Anyway, I corrected his codes. I use VanillaJS really. You don’t need any framework or library.
<!--more-->
I wrote a code with Date object. Especially first function look like an extension of the Date object.

Actually second function unnecessary. You don’t have to use it.

## Code

```javascript
Date.prototype.addDays = function(days) {
  this.setDate(this.getDate() + parseInt(days));
  return this;
};


function getDate(days) {
  var date = new Date().addDays(days);
  return date
}
```

## For Example

<script async src="//jsfiddle.net/aligoren/p12awbfe/embed/"></script>
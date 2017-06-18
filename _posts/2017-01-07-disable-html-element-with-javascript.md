---
layout: post
title: Disable or Enable HTML Element with JavaScript
tags: [JavaScript, Vanilla JS, jQuery, Programming]
---

Hi. I needed to make the HTML elements enabled or disable.

When I try with VanillaJS I wrote this code:
<!--more-->

## VanillaJS Way

**JS Code**

```javascript
function disableOrEnableBtn() {
    var isDisabled = document.getElementById("myBtn").disabled;
    if(isDisabled) {
      document.getElementById("myBtn").disabled = false;
    } else {
      document.getElementById("myBtn").disabled = true;
    }
}
```

**HTML Code**

```html
<button id="myBtn">Button</button>

<button onclick="disableBtn();">Change Button Status</button>
```

See VanillaJS Example [https://jsfiddle.net/aligoren/pqa6xw51/](https://jsfiddle.net/aligoren/pqa6xw51/)

## jQuery Way

You can also use jQuery. This code is very basic:

**JS Code**

```javascript
$.disableBtn = function() {
	$('#myBtn').prop("disabled", (_, val) => !val);
}
```

**HTML Code**

```html
<button id="myBtn">Button</button>

<button onclick="$.disableBtn();">Change Button Status</button>
```

See jQuery Example [https://jsfiddle.net/aligoren/n14pzh5g/](https://jsfiddle.net/aligoren/n14pzh5g/)

## AngularJS Way

```html
<body ng-app="">
  <label>Change Button Status: <input type="checkbox" ng-model="checked"></label><br/>
  <button ng-model="button" ng-disabled="checked">Button</button>
</body>
```

See Example [https://jsfiddle.net/aligoren/6nby94kj/](https://jsfiddle.net/aligoren/6nby94kj/)
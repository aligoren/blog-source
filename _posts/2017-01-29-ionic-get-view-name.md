---
layout: post
title: How to Get Current View Name in Ionic Framework
tags: [JavaScript, AngularJS, Ionic, Tips]
---

Hi. I needed current view name to dynamic css changing in ionic framework. I’ve tried document.URL but it’s not worked. Do not use document.URL or others because you don’t need.

Ionic’s `$state` (actually angular’s $state) has `current.name property`.

<!--more-->

## For Example:

I do not recommend but just example :)

```javascript
// shopCtrl.js

if($state.current.name == 'intro') {
	$(".nav-bar").css('display', 'none')
}
```

Yes! it worked.
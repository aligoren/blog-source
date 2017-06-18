---
layout: post
title: JavaScript Currency Formatting
tags: [JavaScript, Programming, Tips]
---

Hi. I needed to Turkish currency formatting. I asked stackoverflow community.

They advice me the Numeral.js library. I think it’s a useful library. I asked myself. Why do I need a library? I didn’t need jQuery or any other library.
<!--more-->
We can use [Intl.NumberFormat](https://developer.mozilla.org/tr/docs/Web/JavaScript/Reference/Global_Objects/NumberFormat). It’s also simple to use.

## How we use?

```javascript
var price = 1500
var currency_symbol = "₺"

var formattedOutput = new Intl.NumberFormat('tr-TR', {
    style: 'currency',
    currency: 'TRY',
    minimumFractionDigits: 2,
  });

return formattedOutput.format(price).replace(currency_symbol, '')
```

We replaced currency symbol because we needed only price value.

## Options

- style: Formatting type
- currency: Which country's currency?
- minimumFractionDigits: Decimal setting

See jsFiddle for example: [https://jsfiddle.net/aligoren/qLgw02f0/](https://jsfiddle.net/aligoren/qLgw02f0/)
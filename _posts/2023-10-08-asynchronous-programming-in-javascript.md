---
layout: post
title: JavaScript'de Asenkron Programlama
date: 2023-10-08 00:00:00 +0300
categories: [javascript, programming, asynchronous]
---

# Asenkron Programlama ile JavaScript

JavaScript, yorumlamalı bir script dili olup, öncelikli olarak web uygulamaları oluşturmak için kullanılan, çok yönlü ve güçlü bir dil olarak öne çıkmaktadır. Bu dilde yazılmış bir kodun hemen çalışması yerine, bazı durumlarda, belirli bir işlemin sonucunu beklemesi gerekebileceği durumlar olabilir. Bu beklemeleri yönetmek amacıyla JavaScript'in asenkron programlama yetenekleri devreye girer.

## Asenkron Programlama Nedir?

Asenkron programlama, belirli bir işlemin tamamlanmasını beklemeden diğer işlemlerin devam edebilmesini sağlar. Web uygulamalarında, genellikle veri tabanı sorguları, dosya okuma/yazma işlemleri ve API çağrıları gibi I/O işlemleri için asenkron programlama kullanılır.

JavaScript'de, bu tür işlemler `callback` fonksiyonları, `Promise` nesneleri ve `async/await` syntaxı ile yönetilebilir.

## Callback Fonksiyonları

JavaScript'deki en temel asenkron programlama mekanizması, _callback_ fonksiyonlarıdır. Bir fonksiyonun parametresi olarak verilen başka bir fonksiyonda olduğu gibi, _callback_ fonksiyonları da başka bir fonksiyonun parametresi olarak geçer.

```javascript
function myDisplayer(some) {
  console.log(some);
}

function myCalculator(num1, num2, myCallback) {
  let sum = num1 + num2;
  myCallback(sum);
}

myCalculator(5, 5, myDisplayer);
```

## Promise

JavaScript'teki `Promise` objesi, asenkron işlemlerin sonucunun temsil edilmesi için kullanılır. Bir `Promise` objesi, asenkron işlem tamamlandığında sonucun değerini sağlar veya işlem başarısız olduğunda bir hata mesajı döndürür.

```javascript
let myPromise = new Promise(function (myResolve, myReject) {
  let x = 0;

  // Eğer x 0 ise myResolve() çağrılır
  if (x === 0) {
    myResolve("OK");
  } else {
    myReject("Error");
  }
});

myPromise.then(
  function (value) {
    myDisplayer(value);
  },
  function (error) {
    myDisplayer(error);
  }
);
```

## Async ve Await

`async` ve `await` anahtar kelimeleri, JavaScript'teki `Promise` objelerinin kullanımını kolaylaştırmak için ES8 (ECMAScript 2017) ile birlikte tanıtıldı.

```javascript
async function myDisplay() {
  let myPromise = new Promise(function (myResolve, myReject) {
    myResolve("I love You!!");
  });
  console.log(await myPromise);
}

myDisplay();
```

Bu makale, GPT-4 tarafından üretilmiştir.

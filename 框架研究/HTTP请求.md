# HTTP请求

> HTTP请求在开发过程中是非常常见的需求，当然有相当多的解决方案，其中有JS语言内置功能，也有开源社区贡献的开发库。

## Ajax

Ajax 的全称为 Asynchronous JavaScript + XML（异步JavaScript和XML），并不是新的编程语言，而是一种使用现有标准的方法。 Ajax 是在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术。

Ajax由JavaScript、XMLHTTPRequest、DOM对象组成，通过XMLHTTPRequest对象来向服务器发异步请求，从服务器获得数据，然后用JavaScript来操作DOM而更新页面。具体实现如下：

```javascript
  //基本操作
  var data = {};
  //1.创建XMLHttpRequest对象
  var xhr = new XMLHttpRequest();

  //2.向服务器发送请求
  //open方法第三个参数async，true为异步，false为同步
  xhr.open('get', 'https://www.demo.com/', true);
  xhr.onload = () => {
    data = JSON.parse(xhr.responseText);
  }
  xhr.send();


  // XMLHttpRequest中readyState(请求的状态)有5个可取值：
  //       0 = 未初始化，
  //       1 = 初始化，
  //       2 = 发送数据，
  //       3 = 传输中，
  //       4 = 发送完成
```

缺陷 : 配置和调用方式非常繁琐 , 不支持浏览器后退事件 ，安全性不高 ，请求后的数据需要使用 json 解析

## Fetch

XMLHttpRequest 是一个设计粗糙的 API，配置和调用方式非常繁琐，而且基于事件的异步写起来也不友好。

Fetch API  提供了一个 JavaScript接口，用于访问和操纵HTTP，例如请求和响应。它还提供了一个全局 fetch()方法，该方法提供了一种简单，promise的方式来跨网络异步获取资源。

```javascript
//es6
//Fetch 请求默认是不带 cookie 的，需要配置 fetch(url, {credentials: 'include'})
fetch(url).then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.log("error"))

//es7
async function(){
  try {
    let response = await fetch(url);
    let data = response.json();
    console.log(data);
  } catch(error) {
  console.log("error");
  }
}
```

优势 : 语法简洁，更加语义化，基于标准 Promise 实现，支持 async/await

## Axios

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。浏览器端发起XMLHttpRequests请求，node端发起http请求。并且能够自动转化json数据，是如今JS框架主流的 HTTP 库。

```javascript
//常规使用
axios.get('/user?ID=1024')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

axios.post('/user', {
    id : 1024
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

//并发请求
axios.all([
  axios.get('/user/1024'),
  axios.get('/user/1024/profile')
]).then(axios.spread(function(res1,res2){
  //所有请求都完成时会触发当前函数
  //当所有的请求都完成后，会收到一个数组，包含着响应对象
  //其中的顺序和请求发送的顺序相同
  //axios.spread方法会将其分割成多个单独的响应对象
  console.log(res1.data);
  console.log(res2.data);
}));

//API配置
//axios(config)
axios({
  method: 'post',
  url: '/user/1024',
  data: {
    name : 'snowball'
  }
});

//axios(url[, config])
//发送get请求(默认为get方式)
axios('/user/1024');

```

优势 : 支持同构(即前后端都可以使用)，支持 promise ，简洁方便 ，易操作 ，安全性好

## 参考链接

- Ajax :

  https://developer.mozilla.org/zh-CN/docs/AJAX

  https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest

- Fetch :

  https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API

  https://segmentfault.com/a/1190000003810652

- Axios :

  https://github.com/mzabriskie/axios

  https://segmentfault.com/a/1190000008470355?utm_source=tuicool&utm_medium=referral

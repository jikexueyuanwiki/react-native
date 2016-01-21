# 网络

React Native 的一个目标是成为一个游乐场所，在这里我们可以尝试不同的体系结构和疯狂的想法。自从浏览器使用起来不够灵活，我们别无选择，只能去实现整个堆栈。在这个我们并不打算改变什么的地方，我们试图尽可能忠实于浏览器的 APIS。网络协议栈是一个很好的例子。 

## XMLHttpRequest 

XMLHttpRequest API 是在 [iOS networking apis](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html) 之上实现的。与 web 显著的区别是其安全模式：由于没有 [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) 的概念，你可以从互联网上的任一网站上进行阅读。

```
var request = new XMLHttpRequest();
request.onreadystatechange = (e) => {
  if (request.readyState !== 4) {
    return;
  }
  if (request.status === 200) {
    console.log('success', request.responseText);
  } else {
    console.warn('error');
  }
};
request.open('GET', 'https://mywebsite.com/endpoint.php');
request.send();
```

请按照 [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)，一个对 API 进行了完整描述的文档。

作为一个开发人员，你可能不会直接将 XMLHttpRequest 对象作为他的 API，因为这是一个非常繁琐的工作。但事实上，他的实现和与浏览器 APIS 的兼容能够使你使用第三方库，例如，直接来自 npm 的 [Parse](https://parse.com/products/javascript) 和 [super-agent](https://github.com/visionmedia/superagent)。 

## Fetch 

[Fetch](https://fetch.spec.whatwg.org/) 是一种更好的网络 API，它的工作是通过标准委员会完成，并且已经在火狐浏览器上可以使用。默认情况下在 React Native 上也是可用的。

```
fetch('https://mywebsite.com/endpoint.php')
  .then((response) => response.text())
  .then((responseText) => {
    console.log(responseText);
  })
  .catch((error) => {
    console.warn(error);
  });
```

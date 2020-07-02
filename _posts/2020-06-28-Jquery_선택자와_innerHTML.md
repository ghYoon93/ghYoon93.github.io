---
title: JQuery selector와 innerHTML
categories: error
tags: [JQuery]
---



JQuery selector는 DOM 요소가 아닌 Jquery 컬렉션을 반환한다. 따라서 DOM 요소가 필요한 innerHTML이 동작하지 않는다.

```javascr
$('body').innerHTML = "HI";
```

##### 결과

```html
<html>
    

  <head>
    
  </head>
  <body>
    
  </body>
</html>
```



### 해결 방법

1. html();을 사용한다.

```javasc
$('body').html("HI");
```

##### 결과

```html
<html>
    

  <head>
    
  </head>
  <body>
    HI
  </body>
</html>
```

2. Jquery 컬렉션에서 DOM 요소를 가져온다.

```javasc
$('body').first().innerHTML = "HI";
$('body').get(0).innerHTML = "HI";
```



### 
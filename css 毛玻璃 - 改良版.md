### 毛玻璃效果

> 毛玻璃背景是一个很常见的网页样式，但大量实现方法都把问题复杂化了
> 现提供一个代码很直白且实现效果良好的实现方案，改良自[W3Schools](https://www.w3schools.com/)

## HTML部分

```
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>FrostedGlass</title>
    <link rel="stylesheet" href="frostedGlass.css">
  </head>
  <body>
    <div class="mainHolder">
      <div class="textHolder">
        <p>this is FrostedGlass</p>
      </div>
    </div>
  </body>
</html>
```

.mainHolder是主框体
.textHolder是毛玻璃区域
.p是浮于毛玻璃上的文字内容

## CSS部分

```
* {
  box-sizing: border-box;
}
.mainHolder {
  width: 600px;
  height: 600px;
  background-image: url(https://s3-us-west-2.amazonaws.com/s.cdpn.io/3/skyscrapers.jpg);
  background-attachment: fixed;
  background-position: center;
  background-size: cover;
  position: relative;
}
.textHolder {
  width: 100%;
  height: 200px;
  position: absolute;
  right: 0;
  bottom: 0;
  background: inherit;
  overflow: hidden;
}
.textHolder::before {
  content: '';
  position: absolute;
  top:0;
  right: 0;
  bottom: 0;
  left: 0;
  background: inherit;
  background-attachment: fixed;
  filter: blur(4px);
}
.textHolder::after {
  content: "";
  position: absolute;
  top:0;
  right: 0;
  bottom: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.25);
}
p {
  z-index: 1;
  color: white;
  position: relative;
  margin: 0;
}
```

解决毛玻璃效果里最核心的问题：模糊效果不能影响字体，采用了伪元素::after和::before
值得注意的是，在p标签里的position属性。设置为relative后，z-index会生效，将p从被遮挡状态“提起来”。
另外，对于不同的浏览器内核，filter的写法会有些许不同。
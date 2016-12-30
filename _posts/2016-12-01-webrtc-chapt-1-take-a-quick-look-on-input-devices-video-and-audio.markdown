---
layout: post
title: WebRTC Chapt 1 - Take a quick look on input devices video and audio
chineseTitle: WebRTC 第一章 - 简单快速看下终端输入设备（视频和音频）
icon: exchange
date:   2016-11-30 10:50:57 +0800
categories:
  - javascript
tags:
  - webrtc
---

---

## Overview

**原文引自**：[Capturing Audio & Video in HTML5](https://www.html5rocks.com/en/tutorials/getusermedia/intro/) - Eric Bidelman.

Audio/Video capture was a big problem for web development for a long time. For many years we've had to rely on browser plugins (`Flash` or `Silverlight`) to get the job done.

音频／视频的捕获长期以来都是网页开发的一个大问题，在过去很长一段时间内我们都是通过浏览器插件（如`Flash`或者`silverlight`等）去实现这样的功能。

HTML5 to the rescue. It might not be apparent, but the rise of HTML5 has brought a surge of access to device hardware. `Geolocation (GPS)`, `the Orientation API (accelerometer)`, `WebGL (GPU)`, and the `Web Audio API (audio hardware)` are perfect examples. These features are ridiculously powerful, exposing high level JavaScript APIs that sit on top of the system's underlying hardware capabilities.

HTML5解救了我们，也许还不是很明显，但是HTML5的出现带来激增的硬件连接方式，`Geolocation (GPS)`、`Orientation API`、`WebGL (GPU)`和`Audio API (audio hardware)`都是很完美的事例，这些特性都异常的强大，且暴露一些位于系统之上的兼容系统之下硬件的高级的Javascript API。

Support:

`getUserMedia()` has been supported since Chrome 21, Opera 18, and Firefox 17.

支持：

`getUserMedia()` 已经支持 Chrome 21+, Opera 18+, and Firefox 17+.

---

## Contents

- [**Feature detection** - 侦测特性](#feature-detection)
- [**Access input device** - 连接输入设备](#access-input-device)
- [**Media constraints** - 媒体常量](#media-constraints)
- [**Providing fallback** - 提供失败处理](#providing-fallback)
- [**Taking screenshots** - 截图](#taking-screenshots)
- [**Applying effects** - 提供效果](#applying-effects)
  - [**CSS Filters** - Css滤镜](#css-filters)
  - [**WebGL Textures** - Webgl质感](#webgl-textures)
- [**Web Audio API** - Web音频API](#web-audio-api)
- [**Conclusion** - Web音频API](#conclusion)

---

## Feature detection

Feature detecting is a simple check for the existence of `navigator.getUserMedia`

侦测特性可以简单的检查已有的对象`navigator.getUserMedia`

~~~javascript
function hasGetUserMedia() {
  return !!(navigator.getUserMedia || navigator.webkitGetUserMedia ||
            navigator.mozGetUserMedia || navigator.msGetUserMedia);
}

if (hasGetUserMedia()) {
  // Good to go!
} else {
  alert('getUserMedia() is not supported in your browser');
}
~~~

You can also use `Modernizr` to detect getUserMedia to avoid the vendor prefix dance yourself:

你也可以使用`Modernizr`去检测getUserMedia，这可以避免你自己去处理浏览器供应商的前缀组合。

~~~javascript
if (Modernizr.getusermedia){
  var gUM = Modernizr.prefixed('getUserMedia', navigator);
  gUM({video: true}, function( //...
  //...
}
~~~

---

## Access input device

To use the webcam or microphone, we need to request permission. The first parameter to `getUserMedia()` is an object specifying the details and requirements for each type of media you want to access. For example, if you want to access the webcam, the first parameter should be `{video: true}`. To use both the microphone and camera, pass `{video: true, audio: true}`

要使用摄像头和麦克风，我们需要请求权限。getUserMedia()的第一个参数指定你想要连接的媒体设备的明细的和需求。比如你想要使用摄像头(webcam),那么第一个参数应该是`{video: true}`，要同时使用麦克风和摄像头，需传入`{video: true, audio: true}`。


[Live Demo](https://zslucky.github.io/webrtc-example/chapt-1/demo1.html),
[Source Code](https://github.com/zslucky/webrtc-example/blob/master/chapt-1/demo1.html)

~~~html
<video autoplay></video>

<script>
  var errorCallback = function(e) {
    console.log('Reeeejected!', e);
  };

  // Not showing vendor prefixes.
  navigator.getUserMedia({video: true, audio: true}, function(localMediaStream) {
    var video = document.querySelector('video');
    video.src = window.URL.createObjectURL(localMediaStream);

    // Note: onloadedmetadata doesn't fire in Chrome when using it with getUserMedia.
    // See crbug.com/110938.
    video.onloadedmetadata = function(e) {
      // Ready to go. Do some stuff.
    };
  }, errorCallback);
</script>
~~~

 Instead of feeding the video a URL to a media file, we're feeding it a `Blob URL` obtained from a `LocalMediaStream` object representing the webcam.

替代以往向video直接插入一个媒体文件URL，现在加入一个`Blob URL`，他包含了一个呈现摄像头内容的`LocalMediaStream`对象。

---

## Media constraints

The first parameter to `getUserMedia()` can also be used to specify more requirements (or constraints) on the returned media stream. For example, instead of just indicating you want basic access to video (e.g. `{video: true}`), you can additionally require the stream to be HD:

`getUserMedia()`的第一个参数也可以用作指定返回的媒体流对象的规格（或者限制），比如代替基础的视频(e.g. `{video: true}`)，你可以额外的让他成为HD的高清视频。

[Live Demo](https://zslucky.github.io/webrtc-example/chapt-1/demo2.html),
[Source Code](https://github.com/zslucky/webrtc-example/blob/master/chapt-1/demo2.html)

~~~javascript
// For HD output
var hdConstraints = {
  video: {
    mandatory: {
      minWidth: 1280,
      minHeight: 720
    }
  }
};

navigator.getUserMedia(hdConstraints, successCallback, errorCallback);

// For normal VGA output
var vgaConstraints = {
  video: {
    mandatory: {
      maxWidth: 640,
      maxHeight: 360
    }
  }
};

navigator.getUserMedia(vgaConstraints, successCallback, errorCallback);
~~~

For more configurations, see the [constraints API](https://w3c.github.io/mediacapture-main/getusermedia.html)

更多的配置，请查阅 [constraints API](https://w3c.github.io/mediacapture-main/getusermedia.html)

---

## Providing fallback

For users that don't have support for `getUserMedia()`, one option is to fallback to an existing video file if the API isn't supported and/or the call fails for some reason:

为那些不支持`getUserMedia()`或者处理视频出错的用户提供一个其他解决方案，一种可选的方案就是选择一个存在的视频插入。

~~~javascript
// Not showing vendor prefixes or code that works cross-browser:

function fallback(e) {
  video.src = 'fallbackvideo.webm';
}

function success(stream) {
  video.src = window.URL.createObjectURL(stream);
}

if (!navigator.getUserMedia) {
  fallback();
} else {
  navigator.getUserMedia({video: true}, success, fallback);
}
~~~

---

## Taking screenshots

The \<canvas\> API's `ctx.drawImage(video, 0, 0)` method makes it trivial to draw \<video\> frames to \<canvas\>：

\<canvas\> APIT的`ctx.drawImage(video, 0, 0)`方法可以简单的从\<video\>中将多帧画入\<canvas\>：

[Live Demo](https://zslucky.github.io/webrtc-example/chapt-1/demo3.html),
[Source Code](https://github.com/zslucky/webrtc-example/blob/master/chapt-1/demo3.html)

~~~html
<video autoplay></video>
<img src="">
<canvas style="display:none;"></canvas>

<script>
  var video = document.querySelector('video');
  var canvas = document.querySelector('canvas');
  var ctx = canvas.getContext('2d');
  var localMediaStream = null;

  function snapshot() {
    if (localMediaStream) {
      ctx.drawImage(video, 0, 0);
      // "image/webp" works in Chrome.
      // Other browsers will fall back to image/png.
      document.querySelector('img').src = canvas.toDataURL('image/webp');
    }
  }

  video.addEventListener('click', snapshot, false);

  // Not showing vendor prefixes or code that works cross-browser.
  navigator.getUserMedia({video: true}, function(stream) {
    video.src = window.URL.createObjectURL(stream);
    localMediaStream = stream;
  }, errorCallback);
</script>
~~~

---

## Applying Effects

### CSS Filters

Using CSS Filters, we can apply some effects to the \<video\> as it is captured:

使用CSS滤镜，我们可以提供一些效果给捕获的视频。

[Live Demo](https://zslucky.github.io/webrtc-example/chapt-1/demo4.html),
[Source Code](https://github.com/zslucky/webrtc-example/blob/master/chapt-1/demo4.html)

~~~html
<style>
video {
  width: 307px;
  height: 250px;
  background: rgba(255,255,255,0.5);
  border: 1px solid #ccc;
}
.grayscale {
  filter: grayscale(1);
}
.sepia {
  filter: sepia(1);
}
.blur {
  filter: blur(3px);
}
...
</style>

<video autoplay></video>

<script>
var idx = 0;
var filters = ['grayscale', 'sepia', 'blur', 'brightness',
               'contrast', 'hue-rotate', 'hue-rotate2',
               'hue-rotate3', 'saturate', 'invert', ''];

function changeFilter(e) {
  var el = e.target;
  el.className = '';
  var effect = filters[idx++ % filters.length]; // loop through filters.
  if (effect) {
    el.classList.add(effect);
  }
}

document.querySelector('video').addEventListener(
    'click', changeFilter, false);
</script>
~~~

### WebGL Textures

One amazing use case for video capture is to render live input as a WebGL texture. I will introduce it in later blog,

另一件非常有意思的事是视频捕获可以将实时输入内容渲染为一个WebGL质感的视频。我会在之后的博客中详细介绍。

---

## Web Audio API

Chrome supports live microphone input from `getUserMedia()` to the Web Audio API for real-time effects. Piping microphone input to the Web Audio API looks like this:

Chrome支持从`getUserMedia()`得到麦克风的输入内容，并可以在音频API实时生效。将麦克风输入内容管道式的传输的音频API看上去就像以下的代码：

`\\TODO`: Add live demo and source code.

~~~javascript
window.AudioContext = window.AudioContext || window.webkitAudioContext;

var context = new AudioContext();

navigator.getUserMedia({audio: true}, function(stream) {
  var microphone = context.createMediaStreamSource(stream);
  var filter = context.createBiquadFilter();

  // microphone -> filter -> destination.
  microphone.connect(filter);
  filter.connect(context.destination);
}, errorCallback);
~~~

---

## Conclusion

In general, device access on the web has been a tough cookie to crack. Many people have tried, few have succeeded. Most of the early ideas have never taken hold outside of a proprietary environment nor have they gained widespread adoption.

普遍来看在网页中连接设备还是一块难啃的骨头，很多人尝试过，少部分人成功了。早期大部分的想法都没有能够带出去到真正合适的环境中或者甚至都没有能被普遍的采用。

The real problem is that the web's security model is very different from the native world.

真正的问题是网页安全模型有别于原生代码环境。

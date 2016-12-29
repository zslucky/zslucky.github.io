---
layout: post
title: WebRTC Chapt 2 - Communication with RTCPeerConnection without server.
date:   2016-12-02 10:50:57 +0800
published: true
categories:
  - javascript
tags:
  - webrtc
---

---

## Overview

**原文引自**：[Capturing Audio & Video in HTML5](https://www.html5rocks.com/en/tutorials/getusermedia/intro/) - Eric Bidelman.

`RTCPeerConnection` is the WebRTC component that handles stable and efficient communication of streaming data between peers.

Below is a WebRTC architecture diagram showing the role of `RTCPeerConnection`. As you will notice, the green parts are complex!

`RTCPeerConnection`是一个WebRTC组件，他可以稳定和高效的管理点与点之间通讯的流数据。以下这张WebRTC组织结构图显示了`RTCPeerConnection`扮演的角色。也许你会注意到了，绿色的部分相当复杂！

![WebRTC Architecture]({{ site.url }}/static/webrtc/chapt2-webrtcArchitecture.png)

From a JavaScript perspective, the main thing to understand from this diagram is that RTCPeerConnection shields web developers from the myriad complexities that lurk beneath. The codecs and protocols used by WebRTC do a huge amount of work to make real-time communication possible, even over unreliable networks:

从Javascript观点出发，图中需要理解的最重要的一件事是RTCPeerConnection保护了web开发人员，使其能从众多的隐藏的错综复杂的问题中脱身。WebRTC使用的编码和协议做了大量的工作来保证实时通讯的可能性，甚至可以运行在不稳定的网络状况之上。

- packet loss concealment
- echo cancellation
- bandwidth adaptivity
- dynamic jitter buffering
- automatic gain control
- noise reduction and suppression
- image 'cleaning'.

---

## Contents

- [**Communication without servers** - 不与服务器通讯](#communication-without-servers)
  - [**Caller**](#caller)
  - [**Callee**](#callee)
- [**Communication with servers** - 与服务器通讯](#communication-with-servers)
  - [**A simple video chat client**](#a-simple-video-chat-client)
    - [**Making a call**](#making-a-call)


---

## Communication without servers

This doesn't constitute anything very useful, caller and callee are on the same page, but it does make the workings of the RTCPeerConnection API a little clearer, since the RTCPeerConnection objects on the page can exchange data and messages directly without having to use intermediary signaling mechanisms.

呼叫者和被叫者存在于同一个页面，这本身并没有形成任何有用的，但是他能帮我们把RTCPeerConnection API稍微弄清楚一些，因为RTCPeerConnection对象在一个页面上可以直接交互数据和消息，并不需一定需要去使用signaling这样的中间处理机制。


> One gotcha: the optional second 'constraints' parameter of the `RTCPeerConnection()` constructor is different from the constraints type used by `getUserMedia()`: see [w3.org/TR/webrtc/#constraints](https://w3.org/TR/webrtc/#constraints) for more information.
>
> 一个注意点：`RTCPeerConnection()`构造函数的第二个'constraints'参数与`getUserMedia()`的是不同的，详细请参考[w3.org/TR/webrtc/#constraints](https://w3.org/TR/webrtc/#constraints)

In this example, pc1 represents the local peer (caller) and pc2 represents the remote peer (callee).

在这个例子中，pc1表现为本地机（呼叫者），pc2表现为远程机（被叫者）。

> Follow is just a demo, full code please refer to [Source Code](https://github.com/zslucky/webrtc-example/blob/master/chapt-2/demo1.html), and live demo please see [Live Demo](https://zslucky.github.io/webrtc-example/chapt-2/demo1.html).

### Caller

1.Create a new RTCPeerConnection and add the stream from

1.创建一个RTCPeerConnection并为其添加来源流文件。

~~~javascript
// servers is an optional config file (see TURN and STUN discussion below)
pc1 = new webkitRTCPeerConnection(servers);
// Add stream source to pc1
pc1.addStream(localStream);

// Set on ice candidate event handler
pc1.onicecandidate = function pc1onicecandidate(e) {
  onIceCandidate(pc2, e);
};

function onIceCandidate(otherPC, event) {
  if (event.candidate) {
    otherPC.addIceCandidate(
      new RTCIceCandidate(event.candidate)
    ).then(
      function() {
        console.log('otherPC add candidate success!');
      },
      errorHandler
    );
  }
}
~~~

2.Create an offer and set it as the local description for pc1 and as the remote description for pc2. This can be done directly in the code without using signaling, because both caller and callee are on the same page:

2.创建一个offer并且设置他的本地描述为pc1，远程描述为pc2，这一点可以直接在代码中完成无需使用signaling，因为它们在一个页面中。

~~~javascript
pc1.createOffer(gotDescription1);
//
function gotDescription1(desc){
  pc1.setLocalDescription(desc);
  trace("Offer from pc1 \n" + desc.sdp);
  pc2.setRemoteDescription(desc);
  pc2.createAnswer(gotDescription2);
}

//
function gotDescription2(desc){
  pc2.setLocalDescription(desc);
  trace("Offer from pc1 \n" + desc.sdp);
  pc1.setRemoteDescription(desc);
}
~~~

### Callee

1.Create pc2 and, when the stream from pc1 is added, display it in a video element:

1.同样的创建pc2并且当pc1的数据流杯添加时，将它显示到video元素上：

~~~javascript
pc2 = new webkitRTCPeerConnection(servers);
pc2.onaddstream = gotRemoteStream;

pc2.onicecandidate = function pc1onicecandidate(e) {
  onIceCandidate(pc1, e);
};

//...
function gotRemoteStream(e){
  vid2.src = URL.createObjectURL(e.stream);
}
~~~

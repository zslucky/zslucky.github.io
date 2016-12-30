---
layout: post
title: WebRTC Chapt 3 - Communication with RTCPeerConnection with server.
chineseTitle: WebRTC 第三章 - 使用RTCPeerConnection连同Server进行通讯
icon: exchange
date:   2016-12-18 10:50:57 +0800
published: true
categories:
  - javascript
tags:
  - webrtc
---

---

## Overview

**原文引自**：[Capturing Audio & Video in HTML5](https://www.html5rocks.com/en/tutorials/getusermedia/intro/) - Eric Bidelman.

We have seen a demo that communication on the same page, here is a demo for remote communication.

The code refer to [WeebRTC Codelab](https://bitbucket.org/webrtc/codelab/src), this blog refer to [html5rocks](https://www.html5rocks.com/en/tutorials/webrtc/basics/).

---

## Contents

- [**Communication with servers** - 与服务器通讯](#communication-with-servers)
  - [**A simple video chat client** - 一个简单视频聊天客户端](#a-simple-video-chat-client)
    - [**Making a call** - 一次呼叫](#making-a-call)
    - [**Signaling with the Channel API** - 使用Signaling连同CHannel API](#signaling-with-the-channel-api)
  - [**Network topologies** - 网络类型](#network-topologies)

---

## Communication with servers

In the real world, WebRTC needs servers, however simple, so the following can happen:

在真实环境中，WebRTC需要服务器，不管他多简单，所以以下的事情会发生：

- Users discover each other and exchange 'real world' details such as names.（用户互相发现并交换详细信息比如名字）
- WebRTC client applications (peers) exchange network information.（WebRTC客户端应用交换网络信息）
- Peers exchange data about media such as video format and resolution.（主机端之间交换媒体数据，比如视频格式和分辨率）
- WebRTC client applications traverse NAT gateways and firewalls.（WebRTC客户端应用穿越NAT网关和防火墙）

In other words, WebRTC needs four types of server-side functionality:

换句话说，WebRTC需要4中类型的服务器端功能：

- User discovery and communication.（用户发现与通讯）
- Signaling.（发信号）
- NAT/firewall traversal.（NAT／防火墙的穿越）
- Relay servers in case peer-to-peer communication fails.（主机端之间通讯失败的接力服务器）

`NAT` traversal, peer-to-peer networking, and the requirements for building a server app for user discovery and signaling, are beyond the scope of this article. Suffice to say that the `STUN` protocol and its extension `TURN` are used by the `ICE` framework to enable RTCPeerConnection to cope with `NAT` traversal and other network vagaries.

`NAT` 穿越、端到端网络连接和一些必备条件来搭建一个让用户发现或者发信号的服务端APP，是超出这篇文章范围的。`ICE`框架的`STUN`协议和他的扩展`TURN`可以让`RTCPeerConnection`去应付`NAT`穿越和其他变幻莫测的网络。

`ICE` is a framework for connecting peers, such as two video chat clients. Initially, `ICE` tries to connect peers directly, with the lowest possible latency, via `UDP`. In this process, `STUN` servers have a single task: to enable a peer behind a `NAT` to find out its public address and port. (Google has a couple of `STUN` severs)

`ICE`是一个用于端到端连接框架，比如两个视屏聊天的客户端。起初，`ICE`尝试通过UDP直接连接两端，尽可能利用最低的延时。在这个过程中，`STUN`服务器有一个单一的任务：让一个位于NAT背后的主机端可以去找出他的公网地址和端口。（谷歌有很多`STUN`服务器）

![STUN]({{ site.url }}/static/webrtc/chapt3-stun.png)

If UDP fails, `ICE` tries TCP: first HTTP, then HTTPS. If direct connection fails—in particular, because of enterprise NAT traversal and firewalls—ICE uses an intermediary (relay) TURN server. In other words, ICE will first use STUN with UDP to directly connect peers and, if that fails, will fall back to a TURN relay server. The expression 'finding candidates' refers to the process of finding network interfaces and ports.

如果UDP失败了，`ICE`会尝试TCP：先HTTP，然后HTTPS。如果直接连接因特殊情况失败，原因是企业级NAT穿越和ICE防火墙使用了`TURN`作为中间件。换句话说，`ICE`会首先使用UDP的STUN来直接连接主机端，如果失败了，就会启用备用方案使用TURN服务器接力，表达式'finding candidates'退回到流程中的查找网络接口和端口号。

![Data Path Ways]({{ site.url }}/static/webrtc/chapt3-datapathways.png)

### A simple video chat client

The demo starts by running the initialize() function:

这个例子从`initialize()`开始：

~~~javascript
function initialize() {
  console.log("Initializing; room=99688636.");
  card = document.getElementById("card");
  localVideo = document.getElementById("localVideo");
  miniVideo = document.getElementById("miniVideo");
  remoteVideo = document.getElementById("remoteVideo");
  resetStatus();
  openChannel('AHRlWrqvgCpvbd9B-Gl5vZ2F1BlpwFv0xBUwRgLF/* ...*/');
  doGetUserMedia();
}
~~~

Note that values such as the room variable and the token used by `openChannel()`, are provided by the Google App Engine app itself。

注意那些被`openChannel()`使用的值，如房间号和token，都是由google app引擎自己提供的。

This code initializes variables for the HTML video elements that will display video streams from the local camera (localVideo) and from the camera on the remote client (remoteVideo). `resetStatus()` simply sets a status message.

这段代码为HTML video元素初始化数据，之后会将本地摄像头的视频流和远程摄客户端摄像头内容展现出来。`resetStatus()`简单的重置消息状态。

The `openChannel()` function sets up messaging between WebRTC clients:

`openChannel()`功能建立起了WebRTC客户端之间的消息传输：

~~~javascript
function openChannel(channelToken) {
  console.log("Opening channel.");
  var channel = new goog.appengine.Channel(channelToken);
  var handler = {
    'onopen': onChannelOpened,
    'onmessage': onChannelMessage,
    'onerror': onChannelError,
    'onclose': onChannelClosed
  };
  socket = channel.open(handler);
}
~~~

For signaling, this demo uses the Google App Engine `Channel API`, which enables messaging between JavaScript clients without polling.

对于`Signaling`部分，这个例子使用了Google App Engine的Channel API, 他可以让消息在javascript客户端之间获得。

> `Channel API` will be turned off on October 31, 2017. See annoucement [https://cloud.google.com/appengine/docs/deprecations/channel](https://cloud.google.com/appengine/docs/deprecations/channel)
>
> `Channel API`将会在2017/10/01被google关闭，看申明[https://cloud.google.com/appengine/docs/deprecations/channel](https://cloud.google.com/appengine/docs/deprecations/channel)

![AppRTC Architecture]({{ site.url }}/static/webrtc/chapt3-apprtcArchitecture.png)

Establishing a channel with the Channel API works like this:

利用`Channel API`来建立一个频道工作看起来如下：

1. Client A generates a unique ID. （客户端A生成一个唯一的ID）
2. Client A requests a Channel token from the App Engine app, passing its ID. （客户端A向App Engine app传入他的ID并请求一个Channel的token）
3. App Engine app requests a channel and a token for the client's ID from the Channel API. （App Engine app向Channel API为客户端的ID请求一个频道和一个token）
4. App sends the token to Client A. （App 发送这个Token给客户端A）
5. Client A opens a socket and listens on the channel set up on the server. （客户端A打开一个socket并监听这个创建在服务器上的频道）

![Channel Establishing]({{ site.url }}/static/webrtc/chapt3-channelEstablishing.png)

Sending a message works like this:

发送消息的工作看起来如下：

1. Client B makes a POST request to the App Engine app with an update. （客户端B向App Engine应用发起一个带有更新的POST请求）
2. The App Engine app passes a request to the channel. （App Engine应用将请求传递给频道）
3. The channel carries a message to Client A. （频道从客户端A抓去一条消息）
4. Client A's onmessage callback is called. （客户端A的onmessage回掉会被触发）

![Channel Sending]({{ site.url }}/static/webrtc/chapt3-channelSending.png)

Just to reiterate: signaling messages are communicated via whatever mechanism the developer chooses: the signaling mechanism is not specified by WebRTC. The Channel API is used in this demo, but other methods (such as WebSocket) could be used instead.

仅仅是重申：signaling消息可以通过开发者选择的任何一种机制来通信：signaling机制并不是WebRTC特定的。这个例子中使用的是Channel API，但是其他方式（比如WebSocket）也可以用来替代。

After the call to openChannel(), the `getUserMedia()` function called by initialize() checks if the browser supports the getUserMedia API. If all is well, onUserMediaSuccess is called:

在执行openChannel()之后，initialize()当中的`getUserMedia()`方法会检查浏览器是否支持getUserMedia API。如果所有都很好，onUserMediaSuccess会被执行。

~~~javascript
function onUserMediaSuccess(stream) {
  console.log("User has granted access to local media.");
  // Call the polyfill wrapper to attach the media stream to this element.
  attachMediaStream(localVideo, stream);
  localVideo.style.opacity = 1;
  localStream = stream;
  // Caller creates PeerConnection.
  if (initiator) maybeStart();
}
~~~

This causes video from the local camera to be displayed in the localVideo element, by creating an object (Blob) URL for the camera's data stream and then setting that URL as the src for the element. (createObjectURL is used here as a way to get a URI for an 'in memory' binary resource, i.e. the LocalDataStream for the video.) The data stream is also set as the value of localStream, which is subsequently made available to the remote user.

这个示例中本地摄像头的视频将会展现在localVideo元素中，这是通过创建BLOB URL来设置摄像头的数据流，然后将URL作为元素的数据源。（createObjectURL被用作是一种获取存在于内存中二进制数据URL的方法，比如视频的LocalDataStream对象）数据流也会同时被设置到localStream变量中，他会经常性对远程用户表现为可用状态。


Prepare to call, use maybeStart():

准备去执行，使用maybeStart()：


~~~javascript
function maybeStart() {
  if (!started && localStream && channelReady) {
    // ...
    createPeerConnection();
    // ...
    pc.addStream(localStream);
    started = true;
    // Caller initiates offer to peer.
    if (initiator)
      doCall();
  }
}
~~~

This function uses a handy construct when working with multiple asynchronous callbacks: maybeStart() may be called by any one of several functions, but the code in it is run only when localStream has been defined and channelReady has been set to true and communication hasn't already started. So—if a connection hasn't already been made, and a local stream is available, and a channel is ready for signaling, a connection is created and passed the local video stream. Once that happens, started is set to true, so a connection won't be started more than once.

这个方法使用一个好用的构造来处理当多个异步回调同时工作的情况：maybeStart() 可能会被众多方法中的任何一个调用，但是代码只会在当localStream被定义，channelReady被设置为true还有通讯还没有开始时执行。所以如果一个连接还没开始、本地数据流已经可用、一个频道已经为signaling准备好，一个连接被创建和传入本地视频流，当出现以上情况，started会被设置成true，所以一个连接不会被开始超过一次。

#### Making a call

`createPeerConnection()`, called by maybeStart(), is where the real action begins:

`createPeerConnection()`, 会被maybeStart()调用，这是真正行动的开始：

~~~javascript
function createPeerConnection() {
  var pc_config = {"iceServers": [{"url": "stun:stun.l.google.com:19302"}]};
  try {
    // Create an RTCPeerConnection via the polyfill (adapter.js).
    pc = new RTCPeerConnection(pc_config);
    pc.onicecandidate = onIceCandidate;
    console.log("Created RTCPeerConnnection with config:\n" + "  \"" +
      JSON.stringify(pc_config) + "\".");
  } catch (e) {
    console.log("Failed to create PeerConnection, exception: " + e.message);
    alert("Cannot create RTCPeerConnection object; WebRTC is not supported by this browser.");
      return;
  }

  pc.onconnecting = onSessionConnecting;
  pc.onopen = onSessionOpened;
  pc.onaddstream = onRemoteStreamAdded;
  pc.onremovestream = onRemoteStreamRemoved;
}
~~~

The underlying purpose is to set up a connection, using a STUN server, with `onIceCandidate()` as the callback (see above for an explanation of ICE, STUN and 'candidate'). Handlers are then set for each of the `RTCPeerConnection` events: when a session is connecting or open, and when a remote stream is added or removed. In fact, in this example these handlers only log status messages—except for `onRemoteStreamAdded()`, which sets the source for the remoteVideo element:

以下代码意图为建立一个连接，利用STUN服务器并把`onIceCandidate()`作为回调（ICE, STUN和'candidate'参考之前解释）。每个`RTCPeerConnection`事件都会被赋予句柄：当一个连接会话开始，并且远程数据流被添加或移除，实际上，在这个事例中这些句柄只是记录`onRemoteStreamAdded()`为remoteVideo元素添加数据源时的状态：

~~~javascript
function onRemoteStreamAdded(event) {
  // ...
  miniVideo.src = localVideo.src;
  attachMediaStream(remoteVideo, event.stream);
  remoteStream = event.stream;
  waitForRemoteVideo();
}
~~~

Once `createPeerConnection()` has been invoked in `maybeStart()`, a call is intitiated by creating and offer and sending it to the callee:

一旦 `createPeerConnection()` 被`maybeStart()`调用，一个初始化的请求会被发送给被叫者。

~~~javascript
function doCall() {
  console.log("Sending offer to peer.");
  pc.createOffer(setLocalAndSendMessage, null, mediaConstraints);
}
~~~

The offer creation process here is similar to the no-signaling example above but, in addition, a message is sent to the remote peer, giving a serialized SessionDescription for the offer. This process is handled by `setLocalAndSendMessage()`:

这个offer创建过程类似于no-signaling的例子，但是事实上一条消息被发送至了远程主机端，并给予offer一个序列化的SessionDescription，这个过程被`setLocalAndSendMessage()`所处理。

~~~javascript
function setLocalAndSendMessage(sessionDescription) {
  // Set Opus as the preferred codec in SDP if Opus is present.
  sessionDescription.sdp = preferOpus(sessionDescription.sdp);
  pc.setLocalDescription(sessionDescription);
  sendMessage(sessionDescription);
}
~~~

#### Signaling with the Channel API

The `onIceCandidate()` callback invoked when the RTCPeerConnection is successfully created in `createPeerConnection()` sends information about candidates as they are 'gathered':

当RTCPeerConnection在`createPeerConnection()`被成功创建，`onIceCandidate()`回调函数会被调用，然后把candidates信息发送出去。

~~~javascript
function onIceCandidate(event) {
  if (event.candidate) {
    sendMessage({type: 'candidate',
      label: event.candidate.sdpMLineIndex,
      id: event.candidate.sdpMid,
      candidate: event.candidate.candidate});
  } else {
    console.log("End of candidates.");
  }
}
~~~

Outbound messaging, from the client to the server, is done by `sendMessage()` with an XHR request:

发送信息，从客户端岛服务端，由`sendMessage()`的XHR请求完成：

~~~javascript
function sendMessage(message) {
  var msgString = JSON.stringify(message);
  console.log('C->S: ' + msgString);
  path = '/message?r=99688636' + '&u=92246248';
  var xhr = new XMLHttpRequest();
  xhr.open('POST', path, true);
  xhr.send(msgString);
}
~~~

XHR works fine for sending signaling messages from the client to the server, but some mechanism is needed for server-to-client messaging: this application uses the Google App Engine Channel API. Messages from the API are handled by `processSignalingMessage()`:

XHR可以很好的从客户端向服务端发送signaling信息，但是有些原理是必须被用到处理服务器向客户端发消息的：这个客户端用了`processSignalingMessage()`来处理Google App Engine的Channel API

~~~javascript
function processSignalingMessage(message) {
  var msg = JSON.parse(message);

  if (msg.type === 'offer') {
    // Callee creates PeerConnection
    if (!initiator && !started)
      maybeStart();

    pc.setRemoteDescription(new RTCSessionDescription(msg));
    doAnswer();
  } else if (msg.type === 'answer' && started) {
    pc.setRemoteDescription(new RTCSessionDescription(msg));
  } else if (msg.type === 'candidate' && started) {
    var candidate = new RTCIceCandidate({
      sdpMLineIndex:msg.label,
      candidate:msg.candidate
    });
    pc.addIceCandidate(candidate);
  } else if (msg.type === 'bye' && started) {
    onRemoteHangup();
  }
}
~~~

If the message is an answer from a peer (a response to an offer), RTCPeerConnection sets the remote SessionDescription and communication can begin. If the message is an offer (i.e. a message from the callee) RTCPeerConnection sets the remote SessionDescription, sends an answer to the callee, and starts connection by invoking the RTCPeerConnection `startIce()` method:

如果消息是来自一个主机端的answer（一个offer的回复），RTCPeerConnection会将其设置为远程SessionDescription，并且开始通讯。如果一个消息是offer（一个消息来自被叫者）RTCPeerConnection将其设置为远程SessionDescription，然后发送一个answer给被叫者，并调用`startIce()`开启连接。

~~~javascript
function doAnswer() {
  console.log("Sending answer to peer.");
  pc.createAnswer(setLocalAndSendMessage, null, mediaConstraints);
}
~~~

And that's it! The caller and callee have discovered each other and exchanged information about their capabilities, a call session is initiated, and real-time data communication can begin.

这就是它！呼叫者和被叫者互相发现了对方并且交换了信息，一次回话被出示换了，并且实时的数据通讯可以开始了。

### Network topologies

`WebRTC` as currently implemented only supports one-to-one communication, but could be used in more complex network scenarios: for example, with multiple peers each communicating each other directly, peer-to-peer, or via a Multipoint Control Unit (MCU), a server that can handle large numbers of participants and do selective stream forwarding, and mixing or recording of audio and video:

`WebRTC`目前只实现了单对单的通讯，但是可以被应用在更复杂的场景：比如，多个主机端单对单相互直接连接，或者通过MCU，他能处理大量参与者，做先择性数据流转发，还有混合及记录音频视频。

![MCU]({{ site.url }}/static/webrtc/chapt3-mcu.png)

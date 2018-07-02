---
title: "视频自动播放"
description: "视频自动播放"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - auto play video
  - auto play audio
---

以前做过一些音视频方面的页面，一般pc端都可以自动播放，而移动端不可以，不过最近遇到pc端也不可以自动播放的情况，主要是Chrome和Safari，试了下发现果然是静音可以自动播放，非静音就不可以。应该是不久之前改变的策略吧。

> <site><a target="_blank" href="https://developers.google.com/web/updates/2017/09/autoplay-policy-changes">Autoplay Policy Changes</a></site>

> <site><a target="_blank" href="https://webkit.org/blog/7734/auto-play-policy-changes-for-macos/">Auto-Play Policy Changes for macOS</a></site>

> <site><a target="_blank" href="https://webkit.org/blog/6784/new-video-policies-for-ios/">New &lt;video&gt; Policies for iOS</a></site>

> <site><a target="_blank" href="https://wiki.mozilla.org/Firefox/Block_autoplay#Background">Firefox/Block autoplay</a></site>

> <site><a target="_blank" href="https://www.zdnet.com/article/firefox-in-2018-well-tackle-bad-ads-breach-alerts-auto-play-video-says-mozilla/">Firefox in 2018: We'll tackle bad ads, breach alerts, autoplay video, says Mozilla</a></site>

> <site><a target="_blank" href="https://blog.videojs.com/autoplay-best-practices-with-video-js/">Video.js Blog</a></site>

另外要再试一试Firefox浏览器以及音频（audio标签和web audio）是否也是这样。

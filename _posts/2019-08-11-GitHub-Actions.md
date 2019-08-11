---
layout: post
title:  ""
date:   2019-08-11 21:00:45 +0800
categories: [Security, FIDO]
tags: 
 - FIDO
 - WebAuthn
 - Two-factor Authentication
 - UAF U2F
 - Stronger Authentication
---

Github Actions：再次改变软件开发
编译青春
编译青春
腾讯AlloyTeam / FEPulse
10 人赞同了该文章
本文转自 FEPulse 公众号（微信搜索 FEPulse，精选国内外最新前端资讯，为你把握前端脉搏）。



Github Actions 是 GitHub Universe 大会上发布的，被 Github 主管 Sam Lambert 称为“再次改变软件开发”的一款重磅功能（“we believe we will once again revolutionize software development.”）。本文目的是向大家介绍这一 Github 全新的功能，更多内容可以查看文末的拓展阅读。



什么是 Github Actions，官网的介绍是：

With GitHub Actions you can automate your workflow from idea to production.


还是很迷糊。不急，我们先看现在的 Github 是什么？代码仓库，一个提供了分布式版本控制和源代码管理的代码仓库。想象一下这样一种场景，你写好了一个网站的代码，并且存储到了 Github 上，但完事了吗？没有，你还需要部署代码才能让别人访问你的网站。另外，如果你修改了代码，还需要单独测试。理想的情况应该是：当你将代码提交到 master 时，测试、部署等等所有工作自动执行。之前，Travis、Pre-commit Hooks 可以帮助我们实现部分自动化，而现在有了 Github Actions，通通皆可抛。



Github Actions 可以自动化和定制化项目的 Workflow，像官网显示的那样。


Workflow 比较好理解，将对项目的操作概括和按顺序整理，在遇到触发条件时 Workflow 就会按照开发者事先的设置串行或并行地运行一系列 Action，这就是 Github Actions 名称的由来。上面那张图中，Action 即一个个方框，Workflow 即将 Action 连接起来的图表。触发条件有很多种，比如 push 代码到 Github，比如 assign 了一个 issue，比如创建了一个 milestone 等等，这些都是 Github 提供的事件，工作流只要监听关心的事件即可。（目前 Github 一共提供了 26 种事件，想看所有事件可以查看：https://developer.github.com/actions/creating-workflows/workflow-configuration-options/#events-supported-in-workflow-files）



直观地理解了 Workflow 和 Action，下面再对 Github Actions 的核心 Action 作更深入地理解。Action 是一小段可以运行的代码，可以用来做很多事情。比如你可以设置一个自动测试的 Action，当提交代码到 Github 后，Action 便会触发自动测试；再比如你可以设置一个自动部署的 Action，当代码通过测试后直接部署到腾讯云、阿里云、Azure 上。除此以外，你还可以拿 Action 做很多事。比如当前项目是一个 NPM Package，你可以设置一个 Action 用来自动 Publish；比如你需要监听项目的 issue，所以你可以设置一个 Action，当项目中有 issue 创建，给你的微信发一条提醒；比如 minify 或 uglify 你的 JS 代码……Action 的想象空间很大，全看你的需求。目前 Github 一共发布了 450 个示例 Action，你也可以创建、分享你的 Action，别人也能搜到你的 Action。



讲道理，讲完基本概念下面就要开始实操了，但 Github Acions 还处于 Beta 阶段，并没有对所有人开放，想要提前使用的可以在官网尝试申请。因为我还没拿到测试资格，所以后面有机会的话再说吧。不过已经有 Github Actions 的第一批实践者写了一篇文章关于如何设置以及如何创建一个 Action。





最后，再次邀请大家关注我们的公众号 FEPulse，第一时间获取我们精心整理的最新前端资讯。也欢迎在公众号文章或者后台留言讨论。



拓展阅读：

https://zhuanlan.zhihu.com/p/47328730

https://zhuanlan.zhihu.com/p/49152782

https://github.com/features/actions/：官网；
https://developer.github.com/actions/： 文档；
Introducing GitHub Actions：详细介绍了如何设置 Action 和创建新的 Action；
GitHub Actions: built by you, run by us：一些 Action Demo；
GitHub Actions Creates a Buzz for Automated Dev Workflows：新闻报道；
GitHub grabs a piece of the Actions: 'A project that will do for software development what we did for the pull request'：新闻报道。


## 更多参考资源:

[FreeOTP is a two-factor authentication application for systems utilizing one-time password protocols.](https://github.com/freeotp/freeotp-android)    
[andOTP is a two-factor authentication App for Android 4.4+.](https://github.com/andOTP/andOTP)    
[HOTP (HMAC-Based One-Time Password Algorithm) RFC 4226](http://www.ietf.org/rfc/rfc4226.txt)    
[TOTP (Time-Based One-Time Password Algorithm) RFC 6238](http://www.ietf.org/rfc/rfc6238.txt)    
[Accessing GitHub using two-factor authentication](https://help.github.com/en/articles/accessing-github-using-two-factor-authentication)








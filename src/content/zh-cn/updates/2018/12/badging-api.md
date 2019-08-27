project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"
description: Badging API是一种新的Web平台API，允许安装的Web应用程序设置应用程序范围的徽章，显示在与应用程序关联的特定于操作系统的位置，例如货架或主屏幕。

{# wf_published_on: 2018-12-11 #} {# wf_updated_on: 2019-08-21 #} {# wf_featured_image: /web/updates/images/generic/notifications.png #} {# wf_tags: capabilities,badging,install,progressive-web-apps,serviceworker,notifications,origintrials #} {# wf_featured_snippet: The Badging API is a new web platform API that allows installed web apps to set an application-wide badge, shown in an operating-system-specific place associated with the application, such as the shelf or home screen. Badging makes it easy to subtly notify the user that there is some new activity that might require their attention, or it can be used to indicate a small amount of information, such as an unread count. #} {# wf_blink_components: UI>Browser>WebAppInstalls #}

{# When updating this post, don't forget to update /updates/capabilities.md #}

# 应用程序图标的徽章{: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

<div class="clearfix"></div>

<aside class="caution">我们目前正在开发此API作为新<a href="/web/updates/capabilities">功能项目的一部分</a> ，从Chrome 73开始，它可作为<a href="#ot"><b>原始试用版提供</b></a> 。随着Badging API的发展，这篇文章将会更新。 <br> <b>最后更新：</b> 2019年8月21日</aside>

## 什么是Badging API？ {: #what }

<figure class="attempt-right">
  <img src="/web/updates/images/2018/12/badges-on-windows.jpg">
  <figcaption>具有8个通知的Twitter示例和显示标志类型徽章的另一个应用程序。</figcaption>
</figure>

Badging API是一种新的Web平台API，允许已安装的Web应用程序设置应用程序范围的徽章，显示在与应用程序关联的特定于操作系统的位置（例如架子或主屏幕）。

标记可以轻松地巧妙地通知用户有一些可能需要他们注意的新活动，或者它可以用于指示少量信息，例如未读计数。

徽章往往比通知更加用户友好，并且可以以更高的频率更新，因为它们不会中断用户。并且，因为它们不会中断用户，所以不需要使用它们的特殊权限。

[阅读解释者](https://github.com/WICG/badging/blob/master/explainer.md) {: .button .button-primary }

<div class="clearfix"></div>

### Badging API的建议用例{: #use-cases }

可能使用此API的网站示例包括：

- 聊天，电子邮件和社交应用，用于表示新邮件已到达，或显示未读邮件的数量。
- 生产力应用程序，表示已完成长时间运行的后台任务（如渲染图像或视频）。
- 游戏，表示需要玩家行动（例如，在国际象棋中，当玩家轮到时）。

## 当前状态{: #status }

步 | 状态
--- | ---
1.创建解释器 | [完成](https://github.com/WICG/badging/blob/master/explainer.md)
2.创建规范的初始草案 | [完成](https://wicg.github.io/badging/)
**3.收集反馈并重复设计** | [**进行中**](#feedback)
**4.原产地试验** | [**进行中**](#ot)
5.发射 | 没有开始

### 看到它在行动

1. 在Windows或Mac上使用Chrome 73或更高版本，打开[Badging API演示](https://badging-api.glitch.me/) 。
2. 出现提示时，单击“ **安装”**以安装应用程序，或使用Chrome菜单进行安装，然后将其作为已安装的PWA打开。请注意，它必须作为已安装的PWA运行（在任务栏或停靠栏中）。
3. 单击“ **设置”**或“ **清除”**按钮，从应用程序图标设置或清除徽章。您还可以为*徽章值*提供一个数字。

注意：虽然*Chrome中*的Badging API要求安装的应用程序带有实际上可以标记的图标，但我们建议不要根据安装状态调用Badging API。 Badging API可以应用于浏览器可能想要显示徽章的*任何地方* ，因此开发人员不应该对浏览器在哪些情况下使徽章工作做出任何假设。只需在API存在时调用它。如果它工作，它的工作原理。如果没有，它根本就没有。

## 如何使用Badging API {: #use }

从Chrome 73开始，Badging API可用作Windows（7+）和macOS的原始试用版。 [Origin测试](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/README.md)允许您尝试新功能，并向我们和Web标准社区提供有关可用性，实用性和有效性的反馈。有关更多信息，请参阅[Web开发人员](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md)的[Origin Trials指南](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md) 。

### 支持跨平台的徽章

在Windows和macOS上支持Badging API（在原始试用版中）。不支持Android，因为它要求您显示通知，但这可能会在将来发生变化。 Chrome OS支持正在等待平台上的徽章实施。

### 注册原始审判{: #ot }

1. [请求](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)您的原产地[令牌](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481) 。
2. 将令牌添加到您的页面，有两种方法可以在您的源中的任何页面上提供此令牌： 
    - 将`origin-trial` `<meta>`标记添加到任何页面的头部。例如，这可能类似于： `<meta http-equiv="origin-trial" content="TOKEN_GOES_HERE">` 
    - 如果您可以配置服务器，还可以使用`Origin-Trial` HTTP标头在页面上提供令牌。生成的响应标头应如下所示： `Origin-Trial: TOKEN_GOES_HERE` 

### 原产地试验的替代方案

如果您想在本地试用Badging API而不使用原始试用版，请在`chrome://flags`启用`#enable-experimental-web-platform-features` `chrome://flags` 。

### 在原始试验期间使用Badging API

Dogfood：在原始试验期间，API将通过`window.ExperimentalBadge` 。以下代码基于当前设计，并将在作为标准化API在浏览器中登录之前进行更改。

要使用Badging API，您的网络应用需要符合[Chrome的可安装标准](/web/fundamentals/app-install-banners/#criteria) ，用户必须将其添加到主屏幕。

`ExperimentalBadge`接口是`window`上的成员对象。它包含两种方法：

- `set([number])` ：设置应用程序的徽章。如果提供了值，则将徽章设置为提供的值，否则显示纯白点（或适用于平台的其他标记）。
- `clear()` ：删除应用程序的徽章。

```js
// In a web page
const unreadCount = 24;
window.ExperimentalBadge.set(unreadCount);
```

`ExperimentalBadge.set()`和`ExperimentalBadge.clear()`可以从前台页面调用，或者可能在将来调用服务工作者。在任何一种情况下，它都会影响整个应用程序，而不仅仅是当前页面。

在某些情况下，操作系统可能不允许徽章的准确表示，在这种情况下，浏览器将尝试为该设备提供最佳表示。例如，虽然Android不支持Badging API，但Android只显示一个点而不是数字值。

注意：不要假设用户代理如何显示徽章。我们希望一些用户代理将采用类似“4000”的数字并将其重写为“99+”。如果你自己饱和（例如“99”），那么“+”就不会出现。无论实际数量如何，只需设置`Badge.set(unreadCount)`并让用户代理处理相应的显示。

## 反馈{: #feedback }

我们需要您的帮助，以确保Badging API以满足您需求的方式运行，并且我们不会错过任何关键方案。

<aside class="key-point"><b>我们需要你的帮助！</b> - 当前设计（允许整数或标志值）是否满足您的需求？如果不能，请在<a href="https://github.com/WICG/badging/issues">WICG /徽章回购中</a>提交一个问题，并提供尽可能详细的信息。此外，还有一些尚待讨论的<a href="https://github.com/WICG/badging/blob/master/choices.md">未解决问题</a> ，我们很有兴趣听取您的反馈意见。</aside>

我们也很想知道您打算如何使用Badging API：

- 了解用例或想法的用例吗？
- 你打算用这个吗？
- 喜欢它，并想表达你的支持？

分享您对[Badging API WICG话语](https://discourse.wicg.io/t/badging-api-for-showing-an-indicator-on-a-web-apps-shelf-icon/2900)讨论的看法。

{% include "web/_shared/helpful.html" %}

## 有用的链接{: #helpful }

- [公共解释者](https://github.com/WICG/badging/blob/master/explainer.md)
- [徽章API演示](https://badging-api.glitch.me/) | [徽章API演示源](https://glitch.com/edit/#!/badging-api?path=demo.js)
- [跟踪错误](https://bugs.chromium.org/p/chromium/issues/detail?id=719176)
- [ChromeStatus.com条目](https://www.chromestatus.com/features/6068482055602176)
- 申请[原始审判令牌](https://developers.chrome.com/origintrials/#/view_trial/1711367858400788481)
- [如何使用原始试用令牌](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md#how-do-i-enable-an-experimental-feature-on-my-origin)
- 闪烁组件： `UI>Browser>WebAppInstalls`

{% include "web/_shared/rss-widget-updates.html" %}

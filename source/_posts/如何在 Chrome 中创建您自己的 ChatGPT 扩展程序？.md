---
title: 如何在 Chrome 中创建您自己的 ChatGPT 扩展程序？
date: 2023-07-07 12:23:55
categories:
  - AI
tags:
  - ChatGPT
  - Chrome
  - 教程
description: 本文将详细指导您如何利用代码，将ChatGPT集成到您自己的APP或者网站中，提高整个程序的生产力。
cover: https://s2.loli.net/2023/07/25/r4G9umXqWVafbzZ.png
---

ChatGPT 所生成的卓越内容质量确实让我们震惊，让我们惊叹不已。它很快就成为了不仅是我，而且还有许多其他人的首选生产力工具。

ChatGPT具有无限的可能性，我不禁对将其融入各种应用程序感到兴奋。我没有等待其他人推出类似的产品，例如 Microsoft Copilot，我的热情驱使我自己动手解决问题。我迫不及待地想使用 Chrome 扩展程序创建我自己的副驾驶解决方案。

## 想法与计划

如果我可以将 ChatGPT 插入 Chrome 并使其在我使用的大多数 Web 应用程序上可用，我就可以更直接地访问 ChatGPT 的功能，甚至优化流程。这将极大地帮助提高我的生产力。

我需要创建自己的 Chrome 扩展程序并将其与 ChatGPT 连接起来。我想通过鼠标右键单击创建自己的菜单来调用 ChatGPT 服务并将内容发送回我的浏览器。

![](https://s2.loli.net/2023/07/25/ZcO7jXLC2DEetT1.png)

我想创建 3 个功能：重写任何页面上选定的段落（例如我的 Gmail、WhatsApp 等）；总结所选文章；根据选定的对话日志撰写回复。

## 基础知识

Chrome 有一个完整的框架，您可以使用它来创建强大的扩展。它基本上注入后台 Javascript 来与 Chrome API 配合使用，并注入内容 Javascript 来操作 DOM 对象。

另一方面，ChatGPT 更容易使用。它提供了一个RESTful API，您可以通过几行代码来使用它。

## 解决方案

1. 创建本地项目，后台JS和内容JS

![](https://s2.loli.net/2023/07/25/X5jOt2ybuMIDK7W.png)

2. background.js 用于注册鼠标右键的上下文菜单，以便我可以与其交互。

3. content.js 用于查找页面上选定的内容，然后将其传递给 ChatGPT API。 ChatGPT 给出回复后，将其显示在相应的页面上。

## 结果

让我们尝试一下。在我启用该扩展的网站上，我只需选择一个段落，右键单击并转到 ChatGPT 菜单即可要求 ChatGPT 重写该段落或对其进行总结或撰写回复。结果，响应显示在我选择的部分的正下方。

![](https://s2.loli.net/2023/07/25/LmicUxzpQarG2nJ.png)

**有兴趣的可以参考一下代码：**

**manifest.js**：

```js
Key configurations: 
Set the right permission
  "permissions": [
    "tabs",
    "contextMenus",
    "scripting"
  ],
Load the required scripts
  "content_scripts": [
    {
      "js": ["scripts/jquery-3.7.0.min.js", "scripts/content.js"],
      "matches": [
        "https://developer.chrome.com/docs/*"
      ]
    }
```

**background.js**：

```js
//3. use messaging to pass the event to frondend content.js
// https://developer.chrome.com/docs/extensions/mv3/messaging/
function fnCallGPTService(menuItemId){
    console.log("fnCallGPTService called");
    (async () => {
        const [tab] = await chrome.tabs.query({active: true, lastFocusedWindow: true});
        const response = await chrome.tabs.sendMessage(tab.id, 
              {userAction: "GPT_REWRITE", menuItemId: menuItemId});
        console.log(response);
      })();
}

//1. Register the menu entry
chrome.runtime.onInstalled.addListener(async () => {
      chrome.contextMenus.create({
        id: 'rewrite',
        title: 'Smart Rewrite',
        type: 'normal',
        contexts: ['page','selection'],
      });
  });


//2. Add listener to handle the click event
// Open a new search tab when the user clicks a context menu
chrome.contextMenus.onClicked.addListener((item, tab) => {
    console.log("contextMenus onClicked");
    fnCallGPTService(item.menuItemId);
});
```

**content.js**：

```js
function fnGptRequest(action){
    let text = "";
    if (window.getSelection) {
        text = window.getSelection().toString();
    } else if (document.selection && document.selection.type != "Control") {
        text = document.selection.createRange().text;
    }
    console.log(`text: ${text}`);

    let basePrompt = create your own base prompt
    if(action == "summerize") 
        basePrompt = create your own base prompt
    if(action == "compose") 
        basePrompt = create your own base prompt
    var settings = {
        "url": replace with your API endpoint,
        "method": "POST",
        "timeout": 0,
        "headers": {
          "Content-Type": "application/json",
          "Authorization": replace with your access token
        },
        "data": JSON.stringify({
          "model": choose your model,
          "messages": [
            {
              "role": "user",
              "content": basePrompt + text
            }
          ]
        }),
      };
    console.log("Sending to ChatGPT");
    
    $.ajax(settings).done(function (response) {
        const reply = response.choices[0].message.content
        const replyHTML = fnGetPopupTemplate(reply);
        $("body").append(replyHTML);
    });
    
}

//4. add listener to receive message from extension. 
chrome.runtime.onMessage.addListener(
    function(request, sender, sendResponse) {
      if (request.userAction === "GPT_REWRITE"){
        if(request.menuItemId == 'rewrite'){
            sendResponse({frondendMsg: "Received GPT_REWRITE rewrite request"});
            fnGptRequest("rewrite");
        }else if(request.menuItemId == 'summerize'){
            sendResponse({frondendMsg: "Received GPT_REWRITE summerize request"});
            fnGptRequest("summerize");
        }else if(request.menuItemId == 'compose'){
            sendResponse({frondendMsg: "Received GPT_REWRITE compose request"});
            fnGptRequest("compose");
        }
      }
    }
    
  );

function fnGetPopupTemplate(textToDisplay){
    //create your own function to display the reply whatever way you want
}
```

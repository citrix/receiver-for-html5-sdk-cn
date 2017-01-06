# 适用于 HTML5 的 HDX SDK

## 简介

Citrix Receiver for HTML5 允许您通过 Web 站点自定义 Citrix 托管应用程序和桌面的交付模型，增强了对 HDX 和 SDK 会话的支持功能。 此功能对在您的企业门户中构建丰富应用程序体验特别有用。 从 Web 站点启动 Citrix 托管应用程序和桌面过程中，可以使用此功能在 Web 服务器上托管 Citrix Receiver for HTML5 时以服务方式向用户提供丰富应用程序体验。

## 入门

  1. 将 `CitrixHTML5SDK.js`、`HDXLauncher.js`、`HDXEngine.html` 文件复制到与父级 HTML 页面相同的文件夹中。
  2. 将 `CitrixHTML5SDK.js` 包含在父级 HTML 页面中。
  3. 设置 HTML5 客户端的完整路径。
  4. 设置用于启动会话的连接参数。
  5. 附加事件（如果需要）。
  6. 通过传递 ICA 启动会话。可以参阅 [StoreFront Web API](https://www.citrix.com/downloads/storefront-web-interface/sdks/storefront-services-api-30.html) 来提取 ICA。

单击[此处](./citrix-receiver)访问完整的 API 文档。
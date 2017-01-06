# 类：Session

## 会话

#### <span class="type-signature"></span>new Session<span class="signature">()</span><span class="type-signature"></span>

### 成员

#### <span class="type-signature"></span>container<span class="type-signature"></span>

包含在其中启动会话的容器的类型和值。

##### 属性：

<table class="props">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>container</code>
    </td>
    
    <td class="type">
      <span class="param-type">对象</span>
    </td>
    
    <td class="description last">
      <h6>
        属性
      </h6>
      
      <table class="props">
        <tr>
          <th>
            名称
          </th>
          
          <th>
            类型
          </th>
          
          <th class="last">
            说明
          </th>
        </tr>
        
        <tr>
          <td class="name">
            <code>type</code>
          </td>
          
          <td class="type">
            <span class="param-type">字符串</span>
          </td>
          
          <td class="description last">
            容器的类型。将在 launchType 设置为“embed”或“newtab”时，分别设置为“iframe”或“window” 。
          </td>
        </tr>
        
        <tr>
          <td class="name">
            <code>value</code>
          </td>
          
          <td class="type">
            <span class="param-type">对象</span>
          </td>
          
          <td class="description last">
            包含 iframe DOM 对象或对在其中基于 launchType 启动会话的窗口的引用。
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

<br />

#### <span class="type-signature">（只读）</span>id<span class="type-signature"></span>

##### 属性： {#properties-2.subsection-title}

<table class="props">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>id</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      对象的 ID。
    </td>
  </tr>
</table>

### 方法

#### <span class="type-signature">（内部）</span>addListener<span class="signature">(eventType, eventListener)</span><span class="type-signature"></span>

按 eventType 注册 eventListener。

##### 参数：

<table class="params">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>eventType</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      需要为其附加侦听器的事件的类型。<br /><br />支持的事件类型：<br /><a href="/Session#~event:onConnection">onConnection</a><br /><a href="/Session#~event:onConnectionClosed">onConnectionClosed</a><br /><a href="/Session#~event:onURLRedirection">onURLRedirection</a><br /><a href="/Session#~event:onError">onError</a><br />
    </td>
  </tr>
  
  <tr>
    <td class="name">
      <code>eventListener</code>
    </td>
    
    <td class="type">
      <span class="param-type"><a href="/global#eventListener">eventListener</a></span>
    </td>
    
    <td class="description last">
      要处理事件的侦听器。
    </td>
  </tr>
</table>

##### 示例

```java
// Adding onConnection event handler
function connectionHandler(event){
    console.log("Event Received : " + event.type);
    console.log(event.data);        
}               
sessionObject.addListener("onConnection",connectionHandler);

// Adding onConnectionClosed event handler
function connectionClosedHandler(event){
    console.log("Event Received : " + event.type);      
    console.log(event.data);        
}
sessionObject.addListener("onConnectionClosed",connectionClosedHandler);

// Adding onError event handler
function onErrorHandler(event){
    console.log("Event Received : " + event.type);      
    console.log(event.data);
}
sessionObject.addListener("onError",onErrorHandler);

//Adding onURLRedirection event handler
function onURLRedirectionHandler(event){
    console.log("Event Received : " + event.type);      
    console.log(event.data);
}
sessionObject.addListener("onURLRedirection",onURLRedirectionHandler);
```

#### <span class="type-signature">（内部）</span>changeResolution<span class="signature">(bounds)</span><span class="type-signature"></span>

更改会话的分辨率。

##### 参数：

<table class="params">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>bounds</code>
    </td>
    
    <td class="type">
      <span class="param-type">对象</span>
    </td>
    
    <td class="description last">
      包含会话分辨率设置。
    </td>
  </tr>
</table>

##### 属性：

<table class="props">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>bounds.autoresize</code>
    </td>
    
    <td class="type">
      <span class="param-type">布尔值</span>
    </td>
    
    <td class="description last">
      应该设置为 false，从而为会话提供固定宽度和高度。如果此值设置为 true，则将对会话调整大小以匹配 iframe 元素或选项卡的大小。
    </td>
  </tr>
  
  <tr>
    <td class="name">
      <code>bounds.width</code>
    </td>
    
    <td class="type">
      <span class="param-type">数字</span>
    </td>
    
    <td class="description last">
      以像素为单位指定的会话的宽度。仅当 autoresize 设置为 false 时，将设置此值。
    </td>
  </tr>
  
  <tr>
    <td class="name">
      <code>bounds.height</code>
    </td>
    
    <td class="type">
      <span class="param-type">数字</span>
    </td>
    
    <td class="description last">
      以像素为单位指定的会话的高度。仅当 autoresize 设置为 false 时，将设置此值。
    </td>
  </tr>
</table>

##### 示例

```java
示例 1：将分辨率更改为固定宽度和高度  
var bounds = {
    "autoresize":false,
    "width": "800",
    "height":"600"
}
sessionObject.changeResolution(bounds);
```

```java
示例 2：更改会话分辨率以匹配 iframe 元素或选项卡的大小
var bounds = {
    "autoresize": true
}
sessionObject.changeResolution(bounds);
```

#### <span class="type-signature">（内部）</span>disconnect<span class="signature">()</span><span class="type-signature"></span>

断开会话连接。

#### <span class="type-signature">（内部）</span>removeListener<span class="signature">(eventType, eventListener)</span><span class="type-signature"></span>

按 eventType 删除 eventListener。

##### 参数：

<table class="params">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>eventType</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      需要为其删除侦听器的事件的类型。<br /><br />支持的事件类型：<br /><a href="/Session#~event:onConnection">onConnection</a><br /><a href="/Session#~event:onConnectionClosed">onConnectionClosed</a><br /><a href="/Session#~event:onURLRedirection">onURLRedirection</a><br /><a href="/Session#~event:onError">onError</a><br />
    </td>
  </tr>
  
  <tr>
    <td class="name">
      <code>eventListener</code>
    </td>
    
    <td class="type">
      <span class="param-type"><a href="/global#eventListener">eventListener</a></span>
    </td>
    
    <td class="description last">
      要处理事件的侦听器。
    </td>
  </tr>
</table>

##### 示例

```java
//Removing the event handler for onConnection event
function connectionHandler(eventObj){
    console.log("Event Received : " + event.type);      
    console.log(event.data);
}
sessionObject.removeListener("onConnection",connectionHandler);
```

#### <span class="type-signature">（内部）</span>sendSpecialKeys<span class="signature">(keys)</span><span class="type-signature"></span> {#~sendSpecialKeys .name}

向会话发送键组合。此版本中仅支持“ctrl+alt+del”。

##### 参数：

<table class="params">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>keys</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      键组合字符串。
    </td>
  </tr>
</table>

##### 示例

```{.prettyprint}
var keys = "ctrl+alt+del"; //Only this key combination is supported in this version.
sessionObject.sendSpecialKeys(keys);
```

#### <span class="type-signature">（内部）</span>start<span class="signature">(launchData)</span><span class="type-signature"></span>

启动会话。

##### 参数：

<table class="params">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>launchData</code>
    </td>
    
    <td class="type">
      <span class="param-type">对象</span>
    </td>
    
    <td class="description last">
      包含 ICA 的类型和值。
    </td>
  </tr>
</table>

##### 属性：

<table class="props">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>launchData.type</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      指定 ICA 数据的数据类型。允许的值为“json”或“ini”。
    </td>
  </tr>
  
  <tr>
    <td class="name">
      <code>launchData.value</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      要启动会话的 ICA 数据。类型为“Json”时应该为 JSON 对象，类型为“ini”时应该为从 .ini 文件读取的一个字符串。
    </td>
  </tr>
</table>

##### 示例

```java
示例 1：ICA 数据采用 JSON 格式时                             
var icaObj = {
    "ClientName":"HTML5-Receiver",
    "Domain":"<domain_name>",
    "ClearPassword":"<password>",
    "InitialProgram":"<initial program",
    "Title":"<title>",
    "Address":"<ip address>",
    "Username":"<user name>",
    "wsPort":"<port val>"
}
var launchData = {"type" : "json",value : icaObj};
sessionObject.start(launchData);
```

<br />

```java
示例 2：ICA 数据采用 INI 格式时
var launchData = {"type" :"ini",value :"<ica data in ini format>"};
sessionObject.start(launchData);
```

### 事件

#### onConnection

在从客户端连接到服务器的过程中接收各种状态。

##### 类型：

> - <span class="param-type">对象</span>

##### 属性： {#properties-5.subsection-title}

<table class="props">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>state</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      不同连接状态如下︰ <br />“connecting”：显示连接对话框之前连接开始时发生。 <br />“connected”：连接已完成且服务器和客户端开始交换数据时发生。 <br />“sessionReady”：会话已完全初始化、启动并准备好允许用户执行交互操作时发生。 <br />
    </td>
  </tr>
</table>

##### 示例

```java
为 onConnection 事件生成事件对象示例。    

{   
    "id":"<session id>",
    "type" : "onConnection",
    "data":{
        "state" : "connecting" // Event is triggered 3 times with different states mentioned above.
    }
}
```

#### onConnectionClosed

与服务器的连接关闭时发生。

##### 示例

```json
为 onConnectionClosed 事件生成事件对象示例。             

{   
    "id":"<session id>",
    "type" : "onConnectionClosed",
}
```

#### onError

Citrix Receiver 中出现任何错误时发生。

##### 类型：

> - <span class="param-type">对象</span>

##### 属性：

<table class="props">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>id</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      <locale_file>.js 中定义的字符串 ID。例如，en.js 表示英语，ko.js 表示韩语等，对于支持的所有区域设置， ID 保持相同。
    </td>
  </tr>
  
  <tr>
    <td class="name">
      <code>message</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      键的本地化错误消息。客户可以在语言文件中提供自定义字符串，以在部署上下文中获得有意义的错误。
    </td>
  </tr>
</table>

##### 示例

```json
为 onError 事件生成事件对象示例。             

{   
    "id":"<session id>",
    "type" : "onError",
    "data":{
            "id":"<key_in_locale_file>" 
            "message" : "<value_for_the_key_in_locale_file>"
        }
}                           
```

#### onURLRedirection {#~event:onURLRedirection .name}

在服务器上配置了 URL 重定向时以及任何 URL 被传递到 HTML5 引擎进行处理时发生。消息中将包含重定向到客户端的 URL。

##### 类型：

> - <span class="param-type">对象</span>

##### 属性：

<table class="props">
  <tr>
    <th>
      名称
    </th>
    
    <th>
      类型
    </th>
    
    <th class="last">
      说明
    </th>
  </tr>
  
  <tr>
    <td class="name">
      <code>url</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      将包含重定向到客户端的 URL 的 url 值。
    </td>
  </tr>
</table>

##### 示例

```json
为 onURLRedirection 事件生成事件对象示例。        

{   
    "id":"<session id>",
    "type" : "onURLRedirection",
    "data":{
        "url" : "<url_received_to_redirect>"
    }
}               
```
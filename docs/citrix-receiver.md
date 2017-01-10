# 命名空间：receiver

## receiver

citrix.receiver

## 成员<span class="type-signature">(readonly) </span><code>apiVersion</code><span class="type-signature"></span>### 属性：

| 名称           | 类型                                  | 说明                     |
| ------------ | ----------------------------------- | ---------------------- |
| `apiVersion` | <span class="param-type">字符串</span> | HTML5 Receiver API 版本。 |

## 方法<span class="type-signature">(static) </span>createSession<span class="signature">(id<span class="signature-attributes">opt</span>, connectionParams, onSessionCreated)</span><span class="type-signature"></span>创建一个新会话并通过回调返回会话实例。可使用会话实例启动会话、注册和处理事件，以及断开会话连接。

#### 参数：<table class=params>名称

类型

属性<th class=last>说明<td class=name>`id`<td class=type><span class=param-type>字符串</span><td class=attributes><可选>  


<td class="last description">
  创建会话时分配的 ID。
  
  <tr>
    <td class=name><code>connectionParams</code><td class=type><span class=param-type><a href=/global#connectionParams>connectionParams</a></span><td class=attributes>
    
    <td class="last description">
      用于创建会话的配置选项。
      
      <tr>
        <td class=name><code>onSessionCreated</code><td class=type><span class=param-type><a href=/global#onSessionCreated>onSessionCreated</a></span><td class=attributes>
        
        <td class="last description">
          包含创建的会话对象的回调。<br /><br />签名示例如下︰:<br />function <function_name>(session_object){…}</table> 
          
          <h4>
            引发：
          </h4>
          
          <p>
            无法创建会话对象。
          </p>
          
          <p>
            类型<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="param-type"><a href="/ReceiverError">ReceiverError</a></span>
          </p>
          
          <h4>
            示例
          </h4>
          
          <p>
            下面的代码在 iframe 中启动应用程序/桌面。设置首选项以隐藏会话中工具栏。
          </p>
          
          <pre><code class="js">try{
    citrix.receiver.setPath("http://html5client_hosted_url/");
    var id = "session1"; //Optional parameter
    var connectionParams = {
            "launchType" : "embed",
            "container" : {
                "type" : "iframe",
                "value" : "sessionIframe"
            },
            "preferences" : {
                "ui" : {
                    "toolbar" : {
                        "menubar" : false
                    }
                }
            }
        };
    function sessionCreated(sessionObject){

        //Handle session interactions like events, start, disconnect here.              
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

        //ICADATA has been constructed for example. Recommending to use StoreFront/WebInterface SDK to get ICA. 
        //Refer session.start() for more details.
        var icaData = {
            "Domain":"abcd",
            "ClearPassword":"xxxxxxxxx",
            "InitialProgram":"#Desktop",
            "Title":"Desktop",
            "Address":"xx.xx.xx.xx",
            "Username":"xyz"                
        };
        var launchData = {"type" :"json",value :icaData};   
        sessionObject.start(launchData);
    }
    citrix.receiver.createSession(id,connectionParams,sessionCreated);
}catch(ex){
    console.log(ex);
}
</code></pre>
          
          <h2>
            方法
          </h2>
          
          <p>
            <span class="type-signature">(static) </span>setPath<span class="signature">(path)</span><span class="type-signature"></span> {#.setPath .name}
          </p>
          
          <p>
            设置 HTML5Engine 路径。必须在创建会话之前设置。
          </p>
          
          <h4>
            参数：
          </h4>
          
          <table>
            <tr>
              <th>
                名称
              </th>
              
              <th>
                类型
              </th>
              
              <th>
                说明
              </th>
            </tr>
            
            <tr>
              <td>
                <code>path</code>
              </td>
              
              <td>
                <span class="param-type">字符串</span>
              </td>
              
              <td>
                HTML5Client 文件夹的路径。
              </td>
            </tr>
          </table>
          
          <h4>
            引发：
          </h4>
          
          <pre><code>Path is empty.
</code></pre>
          
          <p>
            类型<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="param-type"><a href="/ReceiverError">ReceiverError</a></span>
          </p>
          
          <p>
            <span class="type-signature">(static) </span>viewLog<span class="signature">()</span><span class="type-signature"></span> {#.viewLog .name}
          </p>
          
          <p>
            在新选项卡中打开日志记录页面。应该在调用此 API 之前设置 HTML5Engine 路径。
          </p>
          
          <h4>
            引发：
          </h4>
          
          <p>
            未设置 HTML5 引擎路径。
          </p>
          
          <p>
            类型<br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="param-type"><a href="/ReceiverError">ReceiverError</a></span>
          </p>
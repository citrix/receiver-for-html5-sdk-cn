# 全局

## 类型定义

### connectionParams

用于创建会话的配置选项。

#### 类型：

> - <span class="param-type">对象</span>

#### 属性：

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
      <code>connectionParams</code>
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
          
          <th>
            属性
          </th>
          
          <th class="last">
            说明
          </th>
        </tr>
        
        <tr>
          <td class="name">
            <code>launchType</code>
          </td>
          
          <td class="type">
            <span class="param-type">字符串</span>
          </td>
          
          <td class="attributes">
            <可选><br />
          </td>
          
          <td class="description last">
            使用的值为“newtab”或“embed”。默认为“newtab”。<br /><br />“newtab”- 在新选项卡中启动会话。<br />“embed”- 在 Iframe 中打开会话。
          </td>
        </tr>
        
        <tr>
          <td class="name">
            <code>container</code>
          </td>
          
          <td class="type">
            <span class="param-type">对象</span>
          </td>
          
          <td class="attributes">
          </td>
          
          <td class="description last">
            指定 launchType 为“embed”时用于会话的容器的 ID 和类型。 <br />
            
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
                  <code>id</code>
                </td>
                
                <td class="type">
                  <span class="param-type">字符串</span>
                </td>
                
                <td class="description last">
                  要嵌入会话的 iframe 元素的 ID。launchType 为“embed”时要使用的必需参数。
                </td>
              </tr>
              
              <tr>
                <td class="name">
                  <code>type</code>
                </td>
                
                <td class="type">
                  <span class="param-type">字符串</span>
                </td>
                
                <td class="description last">
                  要嵌入会话的元素的类型。应该设置为“iframe”。
                </td>
              </tr>
            </table>
          </td>
        </tr>
        
        <tr>
          <td class="name">
            <code>bounds</code>
          </td>
          
          <td class="type">
            <span class="param-type">对象</span>
          </td>
          
          <td class="attributes">
            <可选><br />
          </td>
          
          <td class="description last">
            为会话设置固定宽度和高度。 
            
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
                  <code>autoresize</code>
                </td>
                
                <td class="type">
                  <span class="param-type">布尔值</span>
                </td>
                
                <td class="description last">
                  应该设置为 false，从而为会话提供固定宽度和高度。默认情况下，此值设置为 true，在这种情况下，将对会话调整大小以匹配 iframe 元素或选项卡的大小。
                </td>
              </tr>
              
              <tr>
                <td class="name">
                  <code>width</code>
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
                  <code>height</code>
                </td>
                
                <td class="type">
                  <span class="param-type">数字</span>
                </td>
                
                <td class="description last">
                  以像素为单位指定的会话的高度。仅当 autoresize 设置为 false 时，将设置此值。
                </td>
              </tr>
            </table>
          </td>
        </tr>
        
        <tr>
          <td class="name">
            <code>closeOptions</code>
          </td>
          
          <td class="type">
            <span class="param-type">对象</span>
          </td>
          
          <td class="attributes">
            <可选><br />
          </td>
          
          <td class="description last">
            断开会话连接的操作。默认为 type="close"。 
            
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
                  指定的操作类型。<br />如果 type="redirectUrl"，则将选项卡重定向到值中指定的 URL。<br />如果 type="close"，则在 launchType 为“embed”时将 iframe src 设置为“about:blank”，在 launchType 为“newtab”时关闭选项卡。
                </td>
              </tr>
              
              <tr>
                <td class="name">
                  <code>value</code>
                </td>
                
                <td class="type">
                  <span class="param-type">字符串</span>
                </td>
                
                <td class="description last">
                  指定要重定向的 URL。type 设置为“close”时，将忽略此值。
                </td>
              </tr>
              
              <tr>
                <td class="name">
                  <code>showDisconnectAlert</code>
                </td>
                
                <td class="type">
                  <span class="param-type">布尔值</span>
                </td>
                
                <td class="description last">
                  会话即将由于关闭/重新加载选项卡之类的操作而断开连接时，会向用户提示留在/退出当前页面。 <br /><br />如果该值设置为 true，则显示提示，否则不显示提示。 <br /><br />默认值为 true。
                </td>
              </tr>
            </table>
          </td>
        </tr>
        
        <tr>
          <td class="name">
            <code>preferredLang</code>
          </td>
          
          <td class="type">
            <span class="param-type">字符串</span>
          </td>
          
          <td class="attributes">
            <可选><br />
          </td>
          
          <td class="description last">
            指定要在会话中使用的首选语言代码。 如果指定的语言代码无效或不支持，则回退到“en”。 <br /><br />支持的语言代码︰ en、de、es、fr、ja、ko、ru、zh、zh-cn、zh-tw <br /><br />如果未指定该值，则使用浏览器的语言代码。
          </td>
        </tr>
        
        <tr>
          <td class="name">
            <code>preferences</code>
          </td>
          
          <td class="type">
            <span class="param-type">对象</span>
          </td>
          
          <td class="attributes">
            <可选><br />
          </td>
          
          <td class="description last">
            使用 JSON 隐藏/显示工具栏或各个工具栏项目，阻止 FTU、URLRedirection 和错误对话框。<br />请参阅下面的示例。
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

#### 示例

```java
connectionParams 完整示例
{
    "launchType" : "embed",
    "container" : {
        "id" : "<iframe id>",
        "type" : "iframe"
    },
    "bounds" :{
        "autoresize":false,
        "width": "800",
        "height":"600"
    },  
    "closeOptions" : {
        "type" : "redirecturl",
        "value": "<url to redirect>",
        "showDisconnectAlert" : true //false won't prompt when the session is about to disconnect due to the actions like close/reload of the tab.
    },
    "preferredLang" : "ja", //Setting to Japanese
    "preferences" : {
        "ui" : {
            'toolbar' : {
                "menubar":true, //false - hides the toolbar
                "clipboard":true, //false - hides the clipboard button from toolbar         
                "fileTransfer":true, //false - hides the file upload and download buttons from toolbar
                "about":true, //false - hides the about button from toolbar
                "lock":true, //false - hides the ctrl+alt+del button from toolbar
                "disconnect":true, //false - hides the disconnect button from toolbar
                "logoff":true, // false - hides the logoff button from toolbar
                "fullscreen":true, //false - hides the fullscreen button from toolbar
                "keyboard":true, //false - hides the keyboard button from toolbar, this button appears only in touch devices
                "multitouch":true, //false - hides the multitouch button from toolbar, this button appears only in touch devices
                "switchApp":true, //false - hides the switchApp button from toolbar, this button appears only for apps session
                "preferences":true, //false - hides the preferences button from toolbar
                "gestureGuide":true //false - hides the gestureGuide button from toolbar, this button appears only in touch devices
            },
            "hide":{
                "urlredirection" : false, //true - hides the urlredirection dialog shown by HTML5 Engine
                "error" : false, //true - hides the error dialog shown by HTML5 Engine
                "ftu" : false //true - hides the FTU(first time user dialog) shown by HTML5 Engine
            },
            "appSwitcher": {
                "showTaskbar": true, //false - disables the desktop appSwitcher/taskbar seen at the bottom 
                "autoHide": false, //true - selects the Auto Hide checkbox present in the context menu of desktop appSwitcher/taskbar at the bottom  
                "showIconsOnly": false //true - selects the Show Icons only checkbox present in the context menu of desktop appSwitcher/taskbar at the bottom  
            }
        }
    }
}
```

### eventListener<span class="signature">(event)</span>

要处理事件的侦听器。

#### 参数：

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
      <code>event</code>
    </td>
    
    <td class="type">
      <span class="param-type">对象</span>
    </td>
    
    <td class="description last">
      对应于注册的 eventType 的对象。
    </td>
  </tr>
</table>

#### 属性：

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
      <code>event.id</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      会话对象的 ID。
    </td>
  </tr>
  
  <tr>
    <td class="name">
      <code>event.type</code>
    </td>
    
    <td class="type">
      <span class="param-type">字符串</span>
    </td>
    
    <td class="description last">
      触发的事件类型。
    </td>
  </tr>
  
  <tr>
    <td class="name">
      <code>event.data</code>
    </td>
    
    <td class="type">
      <span class="param-type">对象</span>
    </td>
    
    <td class="description last">
      对应于触发的事件的数据。<br /><br /><a href="/Session#~event:onConnection">onConnection</a><br /><a href="/Session#~event:onConnectionClosed">onConnectionClosed</a><br /><a href="/Session#~event:onURLRedirection">onURLRedirection</a><br /><a href="/Session#~event:onError">onError</a><br />
    </td>
  </tr>
</table>

### onSessionCreated

创建会话对象时的回调。

#### 参数：

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
      <code>sessionObject</code>
    </td>
    
    <td class="type">
      <span class="param-type"><a href="/Session">会话</a></span>
    </td>
    
    <td class="description last">
      要与注册和处理事件、启动和断开连接之类的会话交互的会话对象。
    </td>
  </tr>
</table>

#### 示例

```javascript
function sessionCreated(sessionObject){
    //Handle session interactions like events, start, disconnect here.  
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
```
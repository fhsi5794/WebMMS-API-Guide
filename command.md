## Commands & Usage

---

* #### MMS

  Input:

  ```js
  app:     app name
  wsurl:   WebSocket server URL
  ```

  Example:

  ```js
  var wsurl = ‘boss.ypcloud.com:8081’;
  WebMMS = new MMS(‘myapp’, wsurl);
  ```

#### 

* #### OpenMMS

  Input:

  ```js
  regwsOK: callback after registering with WebSocket server
  regdOK: callback after registering with dCenter, no callback means that the app doesn't have to register with dCenter
  wsState: callback when the WebSocket state changes
  rcveMsg: callback when the WebSocket receives a message
  ```

  Example:

  ```js
  WebMMS = new MMS(‘myapp’,wsurl); 
  WebMMS.OpenMMS( regwsOK, regdOK, wsState, rcveMsg ); 
  var regwsOK = function(result){ 
   console.log('regWS OK %s', result.numUsers);
  } 
  var wsState = function(result){
   console.log(WS error: %s', result); 
  } 
  var regdOK = function(result){
   console.log('regtoCenter: %s', result.ErrMsg);
  }
  var rcveMsg = function(from, data){
   console.log('rcve: from=%s, data=%s', from, JSON.stringify(data)); 
  }
  ```

#### 

* #### SendMMS
* Input:

  ```js
  target: target device
  data: data to be sent
  cb: callback with return msg
  ```

  Example:

  ```js
  var target = 'ylobby';
  var data = 'notify Hello 60 red 3'; 
  var WebMMS = new MMS(‘myapp’,wsurl);
  WebMMS.SendMMS(target, data,  
    function(result){ 
     console.log('sendMMS: result=%s', JSON.stringify(result)); 
    } 
  );
  ```

#### 

* #### StateMMS

  Input:

  ```js
  cmd: command 
  list of commands:
   "nearby": get motebus device(s) in the neighborhood
   "search": search device
   "get": get state or information, depending on the arg 
   "set": set state or information, depending on the arg and argv 
  arg:  command arguments
  argv: argument values
  cb: callback with return msg
  ```

  Example:

  ```js
  nearby: query nearby motebus device from mCenter
   var WebMMS = new MMS(‘myapp’,wsurl);
   WebMMS.StateMMS( 'nearby', '', '', 
    function(devlist){ 
      console.log('nearby: list=%s', JSON.stringify(devlist));
    }
  ); 
  get device: get my device information 
    var WebMMS = new MMS(‘myapp’,wsurl);
    WebMMS.StateMMS('get', 'device', '', 
     function(info){ 
       console.log('get device: info=%s', JSON.stringify(info)); 
       //info: {DDN,EiOwner,EiName,EiType,EiTag,EiLog} 
     } 
    ); 
    set device: set my device information 
      var EiInfo = {"DDN":"","EiOwner":"","EiName":"eiHello","EiType":".mc","EiTag:"#hello","EiLog":""}; 
      var WebMMS = new MMS(‘myapp’,wsurl); 
      WebMMS.StateMMS('set', 'device', EiInfo, 
       function(result) { 
         console.log('set device: result=%s', JSON.stringify(result));
       } 
      );
  ```

#### 

* #### CallMMS

  Input:

  ```js
  target: target device
  func: xRPC function
  args: arguments of function
  cb: callback with return msg
  ```

  Example:

  ```js
  var target = 'jLobby'; 
  var func = 'echo'
  var arg = {"data":"Hello MMS"}; 
  var WebMMS = new MMS(‘myapp’,wsurl); 
  ...
  WebMMS.CallMMS(app, target, func, args,
   function(result){
    console.log('CallMMS: result=%s', JSON.stringify(result));
    }  
  );
  ```




# publisherAndSubscriber

``` javascript
  /**
  * created by Chanphy on 2018.12.3
  */
  window.publisherAndSubscriber = (function() {
    var topics = {};
    // Publisher
    function Publisher() {}
      Publisher.prototype = {

          publish: function(topicName, data) {
              topics[topicName] = topics[topicName] || {};
              topics[topicName]['data'] = topics[topicName]['data'] || [];
              topics[topicName]['data'].push(data);
              if('callback' in topics[topicName]) {
                  var i,
                      j,
                      dataLen = topics[topicName]['data'].length,
                      callbackLen = topics[topicName]['callback'].length;
                  for(i = 0; i < dataLen; ++ i) {
                      for(j = 0; j < callbackLen; ++ j) {
                          topics[topicName]['callback'][j](topics[topicName]['data'][i]);
                      }
                  }
                  // remove message no need
                  topics[topicName]['data'] = [];
              }
          }
      };
      // Subscriber
      function Subscriber() {}

      Subscriber.prototype = {
          subscrib: function(topicName, callback) {

              topics[topicName] = topics[topicName] || {};
              topics[topicName]['callback'] = topics[topicName]['callback'] || [];
              topics[topicName]['data'] = topics[topicName]['data'] || [];

              topics[topicName]['callback'].push(callback);

              var i,
                  j,
                  dataLen = topics[topicName]['data'].length,
                  callbackLen = topics[topicName]['callback'].length;

              for(i = 0; i < dataLen; ++ i) {
                  for(j = 0; j < callbackLen; ++ j) {

                      topics[topicName]['callback'][j](topics[topicName]['data'][i]);

                  }
              }
              // remove message no nedd
              topics[topicName]['data'] = [];
          }
      };

      return {
          Publisher: new Publisher(),
          Subscriber: new Subscriber()
      };
  })();
  
  // how to publish
  window.publisherAndSubscriber.Publisher.publish('sometype', value);
  // how to subscrib
  window.publisherAndSubscriber.Subscriber.subscrib('sometype', (v)=>{
    // your code ...
  });
```

· 2019.1.3更新
```
/**
 *  created by Chanphy on 2019.1.3
 */
export let publisherAndSubscriber = (function() {
  var topics = {};
  // 发布者
  function Publisher() {}
  Publisher.prototype = {
    publish: function(topicName, data) {

      topics[topicName] = topics[topicName] || {};
      topics[topicName]['data'] = topics[topicName]['data'] || [];
      topics[topicName]['data'] = data;

      if ('callback' in topics[topicName]) {
        for(var i = 0; i < topics[topicName]['callback'].length; i++) {
          for(var key in topics[topicName]['callback'][i]) {
            topics[topicName]['callback'][i][key](data);
          }
        }
      }
    },
  };

  // 订阅者
  function Subscriber() {}
  Subscriber.prototype = {
    subscrib: function(topicName, callback) {
      
      topics[topicName] = topics[topicName] || {};
      var ids = (topics[topicName] && topics[topicName]['ids'])|| [];
      var id;
      if(!ids.length) {
        id = 0;
      }else{
        id = ids[ids.length-1]/1 + 1;
      }
      ids.push(id);
      topics[topicName]['ids'] = ids;

      topics[topicName]['callback'] = topics[topicName]['callback'] || [];
      topics[topicName]['data'] = topics[topicName]['data'] || null;
      topics[topicName]['callback'].push({[id]: callback});
      callback(topics[topicName]['data']);

      return id;
    },
  };

  // 获取订阅的消息内容
  function getMessageCenter(topicName) {
    return topics[topicName] && (topics[topicName]['data'] || null);
  }

  // 取消订阅
  function removeSubscriber(topicName, id) {
    for(var i = 0; i < topics[topicName]['callback'].length; i++) {
      if(topics[topicName]['callback'][i][id]) {
        topics[topicName]['callback'].splice(i, 1);
      }
    }
  }

  return {
    Publisher: new Publisher(),
    Subscriber: new Subscriber(),
    getMessageCenter: getMessageCenter,
    removeSubscriber: removeSubscriber,
  };
})();
```

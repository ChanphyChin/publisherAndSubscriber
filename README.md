# publisherAndSubscriber

``` javascript
  window.publisherAndSubscriber = (function() {
    var topics = {};
    // 发布者
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
                  // 移除掉消费完后的消息
                  topics[topicName]['data'] = [];
              }
          }
      };
      // 订阅者
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
              // 移除掉消费完后的消息
              topics[topicName]['data'] = [];
          }
      };

      return {
          Publisher: new Publisher(),
          Subscriber: new Subscriber()
      };
  })();
  
  // how to publish
  window.publisherAndSubscriber.Publisher.publish('type', value);
  // how to subscrib
  window.publisherAndSubscriber.Subscriber.subscrib('headerSelect', (v)=>{
    your code ...
  });
```

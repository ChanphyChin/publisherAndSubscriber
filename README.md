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
                  // remove message no nedd
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

##定义

所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。<br>
Promise对象有以下两个特点:<br>

 （1）对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。<br>

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。<br>

##优点：
将异步操作以同步的操作编程表达出来，避免了层层嵌套的回调函数。<br>
##缺点：
 -  一旦新建他就会立即执行，中途无法取消
-   如果不设置回调函数，promise内部抛出的错误，不会反应到外部
-   当处于pending状态时，无法得知目前进展到哪个阶段（刚刚开始还是即将完成）


##1.什么是promise?使用promise有什么优势?
**promise是异步编程的一种解决方案，优势是可以避免层层嵌套的回调。<br>**
**2.产生背景：**<br>
在promise出现之前，你肯定写过这样的代码：<br>

     $.ajax({
        url1: '......',
         success: function (data1) {
            $.ajax({
               url2: '......',
                success: function (data2) {
                 ... //data2的某些操作依赖于data1
               }
            });
        }
     });

上述代码中，如果还有data3依赖于data2,那么上述代码将会存在更深层次的嵌套，promise的出现就是解决这些嵌套带来的不优雅和低可读性等问题。<br>


##3.promise使用介绍
promise是一个对象，从这个对象中我们可以获取异步操作的结果。<br>
promise代表一个异步操作，有三种状态：<br>
Pending（进行中）、Resolved（已完成，又称 Fulfilled）和Rejected（已失败）。<br>
创建一个promise:<br>

    var promise = new Promise(function(resolve,reject){
        ...some code, such as http request
        if(success){
            resolve(data);   //异步请求成功时，通过resolve向外传递结果
         } else {
             reject(error);   //异步请求失败时，通过reject向外传递结果
         }
     });

简单使用案例：<br>


    var MongoClient = require('mongodb').MongoClient;

    var getData = function(url){
        var promise = new Promise(function(resolve,reject){
           MongoClient.connect(url, function(err, db){
              if(db){
                 var collection = db.collection('users');
                 collection.find({}).toArray(function(err,docs){
                  resolve(docs);
                });
              }
             if(err){
               reject(err);
             }
         });
       });
       return promise;
     }

    getData('mongodb://localhost:27017/zuckjet').then(function(data){
        console.log(data);
    },function(err){
         console.log(err);
    });


上述代码中，首先创建了一个函数getData,该函数返回一个promise实例。在该promise实例中，执行的异步操作代码是访问本地mongodb数据库数据。当请求数据成功时，通过resolve对外传递请求结果。当请求失败时，通过reject对外传递错误信息。<br>

promise.then()接受两个回调函数作为参数，第一个回调函数接收的是请求成功时的数据，即上述代码中resolve(docs)中的doc，第二个回调函数接收的是请求失败时的信息，即上述代码中reject(err)中的err。<br>

promise.all()的使用：
Promise.all方法用于将多个Promise实例，包装成一个新的Promise实例。<br>
Promise.all方法接受一个数组作为参数，数组里的元素都是Promise对象的实例，如果不是，就会先调用下面讲到的Promise.resolve方法，将参数转为Promise实例，再进一步处理。（Promise.all方法的参数可以不是数组，但必须具有Iterator接口，且返回的每个成员都是Promise实例。）<br>

下面是一个用Promise对象实现的 Ajax 操作的例子。<br>

    const getJSON = function(url) {
       const promise = new Promise(function(resolve, reject){
         const handler = function() {
            if (this.readyState !== 4) {
               return;
             }
            if (this.status === 200) {
               resolve(this.response);
            } else {
                 reject(new Error(this.statusText));
             }
          };
          const client = new XMLHttpRequest();
          client.open("GET", url);
          client.onreadystatechange = handler;
          client.responseType = "json";
         client.setRequestHeader("Accept", "application/json");
         client.send();

         });

      return promise;
     };

     getJSON("/posts.json").then(function(json) {
        console.log('Contents: ' + json);
      }, function(error) {
        console.error('出错了', error);
     });





































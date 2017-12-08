## Chapter 1

* TABLE OF CONTENT
    * 重要概念
        * 同步和异步
        * 并发和并行
        
### 概念
* 同步和异步 
    * Synchronous and asychronous are normally refered to function call. 
    * 同步和异步通常用来形容一次方法的调用。
    * Once a synchronous function call starts, caller must wait for the function return, then following work can be processed.
    * 同步方法调用一旦开始，调用者必须等待方法调用返回后，才能继续后续的行为。
    * If we call a asychronous function, it returns immediately. It will not block the following work. The 'true' work will be done in another thread. Once the work is done, it will notify caller.
    * 异步方法调用更像一个消息传递，一旦开始，方法调用就会立即返回，调用者就可以继续后续的操作。而异步方法一般会在另一个线程真实地执行。整个过程不会阻碍调用者工作。如果异步调用需要返回结果，那么当这个异步调用真实完成时，则会通知调用者。
    
* 并发和并行 Concurrency and parallelism
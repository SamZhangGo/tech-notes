# mina框架
Apache Mina Server是一个网络通信应用框架，可以提供JAVA对象的序列化服务、虚拟机管道通信服务等。mina提供了事件驱动、异步操作的编程模型。
* IoService：最底层的是IoService，负责具体的IO相关工作，它隐藏了底层IO实现的细节，对上提供统一的基于事件的异步IO接口。每当有数据到达时，IoService会先调用底层IO接口读取数据，封装成IoBuffer，之后以事件的形式通知上层代码，从而将Java NIO的同步IO接口转化成了异步IO。所以从图上看，进来的low-level IO经过IoService层后变成IO Event。
* IoProcessor：这个接口在另外一个线程上，负责检查是否有数据在通道上读写，也就是说它也有自己的Selector。另外，IoProcessor负责调用注册在IoService上的过滤器，并在过滤器链之后调用IoHandler。
* IoFilter：这个接口定义一组拦截器，这些拦截器可以包括日志输出、黑名单过滤、数据的编码（write方向）和解码（read方向）等功能，其中数据的encode与decode是最为重要的，也是使用mina是最主要关注的地方。
* IoHandler：这个接口负责编写业务逻辑，也就是接收、发送数据的地方。需要由开发者自己来实现这个接口。IoHandler可以看成mina处理流程的终点，每个IoService都需要指定一个IoHandler。
* IoSession： 是对底层连接（服务器与客户端的特定连接，该连接由服务端地址、端口以及客户端地址、端口来决定）的封装，一个IoSession对应于一个底层的IO连接。通过IoSession，可以获取当前连接相关的上下文信息，以及向远程peer发送数据。发送数据其实也是个异步的过程。发送的操作首先会逆向穿过IoFilterChain，到达IoService。但IoService并不会直接调用底层IO接口将数据发送出去，而是会将该次调用封装成一个WriteRequest，放入session的writeRequestQueue中，最后由IoProcessor线程统一调度flush出去。所以发送不会引起上层调用线程的阻塞。

```java
IoAcceptor acceptor = new NioSocketAcceptor();
acceptor.getSessionConfig().setReadBufferSize(2048);
acceptor.getSessionConfig().setIdleTime(IdleStatus.BOTH_IDLE, 10);
//设置过滤器
LoggingFilter lf = new LoggingFilter();
lf.setSessionOperndLogLevel(LogLevel.ERROR);
acceptor.getFilterChain().addLast("logger", lf);
acceptor.getFilterChain().addLast("codec", new ProtocolCodecFilter(new TextLineCodecFactory(Charset.forName("UTF-8"), LineDelimiter.WINDOWS.getValue(), LineDelimiter.WINDOWS.getValue())));
//设置handle
acceptor.setHandler(new CustomServerHandler());

//绑定端口
acceptor.bind(new InetSocketAddress(9124));
```

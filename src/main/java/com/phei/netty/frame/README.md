# 关于粘包拆包问题的发生和解决
1. [默认的使用换行来进行分割](correct/README.md)
2. [自定义分隔符](delimiter/README.md)
3. [自定义长度](fixedLen/README.md)
4. 自定义长度, 但是长度加在了报文信息头部:  

示例
```java
    @Override
    public void initChannel(SocketChannel ch) {
        // 增加解码器 设定 定长的长度
        ch.pipeline().addLast(LengthFieldBasedFrameDecoder(65535, 0, 2, 0, 2));
        ch.pipeline().addLast(new MsgpackDecoder());
        // LengthFieldPrepender 例如 Hello -> oxoo5 Hello
        ch.pipeline().addLast(new LengthFieldPrepender(2));
        ch.pipeline().addLast(new MsgpackEncoder());
        ch.pipeline().addLast(new EchoServerHandler());
    }
    //也就实现了在报文头部添加两个字节的消息长度的字段, 并且使用了MessagePack作为编解码器
```

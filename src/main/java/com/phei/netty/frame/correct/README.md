## 使用 LineBasedFrameDecoder 解决粘包问题

1. 只需要在服务端和客户端的处理器前, 加上两个处理器 LineBasedFrameDecoder  StringDecoder 即可
2. 然后在客户端和服务端的消息的接收方法处,能直接将msg Object对象强转为String, 因为已经解码了

如此简单即可解决问题 

## 原理分析 
LineBasedFrameDecoder 是依次遍历ByteBuf中的可读字节, 判断是否有\n 或者 \r\n 如果有, 就按这个进行分割, 以换行符作为结束符  
同时支持配置单行的最大长度, 如果连续读取到最大长度, 仍然没有发现换行符就会抛出异常, 忽略掉前面读取的异常码流

所以更为方便和强大的是自定义分隔符和定长解码

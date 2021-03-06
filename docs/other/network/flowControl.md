## 什么是流量控制

如果发送者发送数据过快，接收者来不及接收，那么就会有分组丢失。为了避免分组丢失，控制发送者的发送速度，使得接收者来得及接收，这就是流量控制。流量控制根本目的是防止分组丢失，它是构成 TCP 可靠性的一方面。

## 如何实现流量控制

由滑动窗口协议（连续 ARQ 协议）实现。滑动窗口协议既保证了分组无差错、有序接收，也实现了流量控制。主要的方式就是接收方返回的 ACK 中会包含自己的接收窗口的大小，并且利用大小来控制发送方的数据发送，使得**送方的发送窗口数不能超过接收方的接受窗口数**。

## 死锁的产生和解决方式

当发送者收到了一个窗口为 0 的应答，发送者便停止发送，等待接收者的下一个应答。但是如果这个窗口不为 0 的应答在传输过程丢失，发送者一直等待下去，而接收者以为发送者已经收到该应答，等待接收新数据，这样双方就相互等待，从而产生死锁。

为了避免流量控制引发的死锁，TCP 使用了持续计时器。每当发送者收到一个零窗口的应答后就启动该计时器。时间一到便主动发送报文询问接收者的窗口大小。若接收者仍然返回零窗口，则重置该计时器继续等待；若窗口不为 0，则表示应答报文丢失了，此时重置发送窗口后开始发送，这样就避免了死锁的产生。

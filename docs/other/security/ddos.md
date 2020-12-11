拒绝服务攻击（英语：denial-of-service attack，简称 DoS 攻击）亦称洪水攻击，是一种网络攻击手法，其目的在于使目标电脑的网络或系统资源耗尽，使服务暂时中断或停止，导致其正常用户无法访问。

当黑客使用网络上两个或以上被攻陷的电脑作为“僵尸”向特定的目标发动“拒绝服务”式攻击时，称为分布式拒绝服务攻击（distributed denial-of-service attack，简称 DDoS 攻击）。据 2014 年统计，被确认为大规模 DDoS 的攻击已达平均每小时 28 次。DDoS 发起者一般针对重要服务和知名网站进行攻击，如银行、信用卡支付网关、甚至根域名服务器等。

## DDOS 攻击的方式

### SYN Flood

SYN Flood（半开放攻击）是一种拒绝服务（DDoS）攻击，其目的是通过消耗所有可用的服务器资源使服务器不可用于合法流量。通过重复发送初始连接请求（SYN）数据包，攻击者能够压倒目标服务器机器上的所有可用端口，导致目标设备根本不响应合法流量。

为了创建拒绝服务，攻击者利用这样的漏洞，即在接收到初始 SYN 数据包之后，服务器将用一个或多个 SYN / ACK 数据包进行响应，并等待握手中的最后一步。这是它的工作原理：

攻击者向目标服务器发送大量 SYN 数据包，通常会使用欺骗性的 IP 地址。

然后，服务器响应每个连接请求，并留下开放端口准备好接收响应。

当服务器等待从未到达的最终 ACK 数据包时，攻击者继续发送更多的 SYN 数据包。每个新的 SYN 数据包的到达导致服务器暂时维持新的开放端口连接一段时间，一旦所有可用端口被使用，服务器就无法正常工作。

![](/img/other/synFlood.jpg)

### CC 攻击

CC 攻击是目前应用层攻击的主要手段之一，借助代理服务器生成指向目标系统的合法请求，实现伪装和 DDoS。我们都有这样的体验，访问一个静态页面，即使人多也不需要太长时间，但如果在高峰期访问论坛、贴吧等，那就很慢了，因为服务器系统需要到数据库中判断访问者否有读帖、发言等权限。访问的人越多，论坛的页面越多，数据库压力就越大，被访问的频率也越高，占用的系统资源也就相当可观。CC 攻击就充分利用了这个特点，模拟多个正常用户不停地访问如论坛这些需要大量数据操作的页面，造成服务器资源的浪费，CPU 长时间处于 100%，永远都有处理不完的请求，网络拥塞，正常访问被中止。这种攻击技术性含量高，见不到真实源 IP，见不到特别大的异常流量，但服务器就是无法进行正常连接。

![](/img/other/cc.jpg)

CC 攻击是针对 Web 服务在第七层协议发起的攻击，在越上层协议上发动 DDoS 攻击越难以防御，上层协议与业务关联愈加紧密，防御系统面临的情况也会更复杂。比如 CC 攻击中最重要的方式之一 HTTP Flood，不仅会直接导致被攻击的 Web 前端响应缓慢，对承载的业务造成致命的影响，还可能会引起连锁反应，间接攻击到后端的 Java 等业务层逻辑以及更后端的数据库服务。

由于 CC 攻击成本低、威力大，知道创宇安全专家组发现 80%的 DDoS 攻击都是 CC 攻击。带宽资源严重被消耗，网站瘫痪；CPU、内存利用率飙升，主机瘫痪；瞬间快速打击，无法快速响应。

### DNS Query Flood

DNS 作为互联网的核心服务之一，自然也是 DDoS 攻击的一大主要目标。DNS Query Flood 采用的方法是操纵大量傀儡机器，向目标服务器发送大量的域名解析请求。服务器在接收到域名解析请求时，首先会在服务器上查找是否有对应的缓存，若查找不到且该域名无法直接解析时，便向其上层 DNS 服务器递归查询域名信息。

## 防御措施

### 提高服务器性能

扩充服务器带宽, 和硬件性能

### CDN 均衡负载

CDN 是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率，因此 CDN 加速也用到了负载均衡技术。相比高防硬件防火墙不可能扛下无限流量的限制，CDN 则更加理智，多节点分担渗透流量，目前大部分的 CDN 节点都有 200G 的流量防护功能，再加上硬防的防护，可以说能应付目绝大多数的 DDoS 攻击了。

### 验证码

对于频繁的请求要求用户输验证码  
验证码永远滴神

### 限制请求频率

对同一台疯狂请求的 ip 进行限制

### 流量清洗

当流量被送到 DDoS 防护清洗中心时，通过流量清洗技术，将正常流量和恶意流量区分开，正常的流量则回注客户网站。保证高防客户网络的正常运行。那么对于典型的 DDoS 攻击响应中，流量首先进入流量清洗中心，随后将此分类成基础架构攻击流量或者应用层攻击流量。之后还会进行进一步区分，主要通过向量和期待特征确定，通过采用 DDoS 中心的专属技术来处理实现。
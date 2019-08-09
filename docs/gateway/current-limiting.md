# 网关-限流篇
* 目的：

限流可以保障我们的 API 服务对所有用户的可用性，也可以防止网络攻击。

## 限流算法
### 漏桶算法： 
漏桶（Leaky Bucket）算法思路很简单，水（请求）先进入到漏桶里，漏桶以一定的速度出水（接口有响应速率），当水流入速度过大会直接溢出（访问频率超过接口响应速率），然后就拒绝请求，可以看出漏桶算法能强行限制数据的传输速率。

可见这里有两个变量，一个是桶的大小，支持流量突发增多时可以存多少的水（burst），另一个是水桶漏洞的大小（rate）。因为漏桶的漏出速率是固定的参数，所以，即使网络中不存在资源冲突（没有发生拥塞），漏桶算法也不能使流突发（burst）到端口速率。因此，漏桶算法对于存在突发特性的流量来说缺乏效率。
### 令牌桶算法

令牌桶算法（Token Bucket）和 Leaky Bucket 效果一样但方向相反的算法，更加容易理解。随着时间流逝，系统会按恒定 1/QPS 时间间隔（如果 QPS=100，则间隔是 10ms）往桶里加入 Token（想象和漏洞漏水相反，有个水龙头在不断的加水），如果桶已经满了就不再加了。新请求来临时，会各自拿走一个 Token，如果没有 Token 可拿了就阻塞或者拒绝服务。

令牌桶的另外一个好处是可以方便的改变速度。一旦需要提高速率，则按需提高放入桶中的令牌的速率。一般会定时（比如 100 毫秒）往桶中增加一定数量的令牌，有些变种算法则实时的计算应该增加的令牌的数量。

## 限流实现

这里我们使用 Bucket4j，引入它的依赖坐标，为了方便顺便引入 Lombok
```java
//引入jar包
<dependency>
    <groupId>com.github.vladimir-bukhtoyarov</groupId>
    <artifactId>bucket4j-core</artifactId>
    <version>4.0.0</version>
</dependency>

<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.16.20</version>
    <scope>provided</scope>
</dependency>

//Java实现
@CommonsLog
@Builder
@Data
@AllArgsConstructor
@NoArgsConstructor
public class RateLimitByIpGatewayFilter implements GatewayFilter，Ordered {

    int capacity;
    int refillTokens;
    Duration refillDuration;

    private static final Map<String，Bucket> CACHE = new ConcurrentHashMap<>();

    private Bucket createNewBucket() {
        Refill refill = Refill.of(refillTokens，refillDuration);
        Bandwidth limit = Bandwidth.classic(capacity，refill);
        return Bucket4j.builder().addLimit(limit).build();
    }

    @Override
    public Mono<Void> filter(ServerWebExchange exchange，GatewayFilterChain chain) {
        // if (!enableRateLimit){
        //     return chain.filter(exchange);
        // }
        String ip = exchange.getRequest().getRemoteAddress().getAddress().getHostAddress();
        Bucket bucket = CACHE.computeIfAbsent(ip，k -> createNewBucket());

        log.debug("IP: " + ip + "，TokenBucket Available Tokens: " + bucket.getAvailableTokens());
        if (bucket.tryConsume(1)) {
            return chain.filter(exchange);
        } else {
            exchange.getResponse().setStatusCode(HttpStatus.TOO_MANY_REQUESTS);
            return exchange.getResponse().setComplete();
        }
    }

    @Override
    public int getOrder() {
        return -1000;
    }

}

//在 Route 中我们添加这个过滤器，这里指定了 bucket 的容量为 10 且每一秒会补充 1 个 Token。

.route(r -> r.path("/throttle/customer/**")
             .filters(f -> f.stripPrefix(2)
                            .filter(new RateLimitByIpGatewayFilter(10，1，Duration.ofSeconds(1))))
             .uri("lb://CONSUMER")
             .order(0)
             .id("throttle_customer_service")
)
```







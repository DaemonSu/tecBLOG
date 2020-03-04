# Gateway 使用简要说明

## 基本配置文件



```yml
spring:
  cloud:
    gateway:
      routes:
        - id: app
          uri: http://www.dnocm.com
          # lb: Ribbon
          # uri: lb://app-name
          predicates:
            - Path=/app/**
          filters:
            # Strip first path，such base
            - StripPrefix=1
            # - RewritePath=/base/,/
eureka:
  client:
    enabled: false
```

## 断言（predicates）说明

内置断言：

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: example
        uri: http://example.org
        predicates:
        # 匹配在什么时间之后的
        - After=2017-01-20T17:42:47.789-07:00[America/Denver]
        # 匹配在什么时间之前的
        - Before=2017-01-20T17:42:47.789-07:00[America/Denver]
        # 匹配在某段时间的
        - Between=2017-01-20T17:42:47.789-07:00[America/Denver], 2017-01-21T17:42:47.789-07:00[America/Denver]
        # 匹配cookie名称为`chocolate`的值要符合`ch.p`正则.
        - Cookie=chocolate, ch.p
        # 匹配header为`X-Request-Id`的值要符合`\d+`正则.
        - Header=X-Request-Id, \d+
        # 匹配任意符合`**.somehost.org`与`**.anotherhost.org`正则的网址
        - Host=**.somehost.org,**.anotherhost.org
        # Host还支持模版变量，会保存在`ServerWebExchange.getAttributes()`的 ServerWebExchangeUtils.URI_TEMPLATE_VARIABLES_ATTRIBUTE中，以Map形式存储
        - Host={sub}.myhost.org
        # 匹配GET方法
        - Method=GET
        # 路径匹配，与Host一样支持模版变量，存在URI_TEMPLATE_VARIABLES_ATTRIBUTE中。
        - Path=/foo/{segment},/bar/{segment}
        # 匹配存在baz查询参数
        - Query=baz
        # 匹配存在foo且符合`ba.`正则
        - Query=foo, ba.
        # 匹配远程地址
        - RemoteAddr=192.168.1.1/24
```

## filter简要说明

### 内置filter

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: example
        uri: http://example.org
        filters:
          # 先介绍简单的路由器
          # 对header的操作（添加，删除，设置），以及保留原始host的header
          - AddRequestHeader=X-Request-Foo, Bar
          - AddResponseHeader=X-Response-Foo, Bar
          - RemoveRequestHeader=X-Request-Foo
          - RemoveResponseHeader=X-Response-Foo
          - RewriteResponseHeader=X-Response-Foo, , password=[^&]+, password=***
          - SetResponseHeader=X-Response-Foo, Bar
          - PreserveHostHeader
          # 对查询参数的过滤
          - AddRequestParameter=foo, bar
          # 对Uri的过滤（path与status）
          - PrefixPath=/mypath
          - RewritePath=/foo/(?<segment>.*), /$\{segment}
          - SetPath=/{segment}
          - StripPrefix=2
          - SetStatus=BAD_REQUEST
          - SetStatus=401
          - RedirectTo=302, http://acme.org
          # 保留session，默认情况下是不保留的
          - SaveSession
          # 设置最大请求数据大小，这里发现一种新写法，理论上predicates中也能使用
          - name: RequestSize
            args:
              maxSize: 5000000
          # 重试次数设置
          - name: Retry
            args:
              retries: 3
              statuses: BAD_GATEWAY
        
          # 断路器的配置
          # 断路器的配置比较复杂，首先指定断路器命令名即可启用断路器（这块我也不熟，需要HystrixCommand的内容）
          - Hystrix=myCommandName
          # 另外我们可以设置些错误发生后的跳转，当然现在仅支持forward:
          - name: Hystrix
            args:
              name: fallbackcmd
              fallbackUri: forward:/incaseoffailureusethis
          # 我们还可以修改错误信息放置的header
          - name: FallbackHeaders
            args:
              executionExceptionTypeHeaderName: Test-Header
              executionExceptionMessageHeaderName: Test-Header
              rootCauseExceptionTypeHeaderName: Test-Header
              rootCauseExceptionMessageHeaderName: Test-Header

           # 另一块比较困难的是速率限制
          # 它由RateLimiter的实现来完成的，默认只支持redis，需要添加`spring-boot-starter-data-redis-reactive`依赖
          # 我们需要提供KeyResolver的实现，因为默认会使用PrincipalNameKeyResolver，在不使用Spring Security的情况下几乎不会用到Principal
          # 除此外，我们也可以提供自定义的RateLimiter，#{@myRateLimiter}是一个SpEL表达式，用于从Spring上下文内读取bean
          - name: RequestRateLimiter
            args:
              redis-rate-limiter.replenishRate: 10
              redis-rate-limiter.burstCapacity: 20
          - name: RequestRateLimiter
            args:
              rate-limiter: "#{@myRateLimiter}"
              key-resolver: "#{@userKeyResolver}"
```

### 自定义filter




# 内存问题

ios 内存问题 

![](../.gitbook/assets/image%20%28199%29.png)

```text
per-process-limit
```

当我们的App被Jetsam机制杀死的时候，在手机中会生成系统日志，在手机系统设置-隐私-分析中，可以得到JetSamEvent开头的日志。这些日志中就可以获取到一些关于App的内存信息，例如我当前用的iPhone8\(iOS11.4.1\)，在日志中的前部分看到了pageSize，而查找per-process-limit一项\(并不是所有日志都有，可以找有的\)，用该项的rpages \* pageSize即可得到OOM的阈值。 ———————————————— 版权声明：本文为CSDN博主「TuGeLe」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。 原文链接：[https://blog.csdn.net/TuGeLe/article/details/104004692](https://blog.csdn.net/TuGeLe/article/details/104004692)


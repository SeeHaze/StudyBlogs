---
title: c#异常重试机制
date: 2023-02-17 17:53:11
categories: C#
---

```c#
    var policy = Policy
        .Handle<NeedRetryException>()
        .RetryAsync(3, (exception, retryCount) =>
        {
            // Thread.Sleep(3000);
                    
            Console.WriteLine("异常重试:" + exception.Message + $"第{retryCount}次重试");
        });
    var result = policy.ExecuteAsync(() => Test());
```

...

```c#
    private static async Task Test()
    {
        Console.WriteLine("1111111111111");

        throw new NeedRetryException("这是个自定义异常");
    }
```


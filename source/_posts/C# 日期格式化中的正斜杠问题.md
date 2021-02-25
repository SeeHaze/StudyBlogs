---
title: C# 日期格式化中的正斜杠问题
date: 2021-02-25 15:40:50
categories: 日期格式化
---

## C# 日期格式化中的正斜杠问题

```c#
Console.WriteLine(DateTime.Now.ToString("yyyy/MM/dd" ));
```

#### 现象

- 如果系统短日期格式为“2017/04/05”（默认格式）情况时，输出格式是正常的，
- 但是如果更改了短日期格式为`-`或者`.`连接，则会输出 `2021-02-25` 或 `2021.02.25` 

#### 解决方案（三种）

 1. 自定义短日期值的格式

    ```c#
    Console.WriteLine(DateTime.Now.ToString("yyyy/MM/dd HH:mm:ss",
        new DateTimeFormatInfo
        {
        	ShortDatePattern = "yyyy/MM/dd",
        }));
    ```

 2. 转义

    ```c#
    Console.WriteLine(DateTime.Now.ToString("yyyy\\/MM\\/dd HH:mm:ss"));
    ```

3. 字符串(用单引号`'`)

   ```c#
   Console.WriteLine(DateTime.Now.ToString("yyyy'/'MM'/'dd HH:mm:ss"));
   ```

#### 原因解释

​	在DateTimeFormat.Format方法中，正斜杠`/`会被格式化为DateTimeFormatInfo.DateSeparator即短日期分隔符，

```c#
case '/':
    stringBuilder.Append(dtfi.DateSeparator);
    num = 1;
    break;
```

​	使用反斜杠`\`转义或使用单引号`'`将其做为字符串进行处理则可规避这个问题。


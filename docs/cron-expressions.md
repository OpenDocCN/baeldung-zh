# Cron 表达式指南

> 原文：<https://web.archive.org/web/20220930061024/https://www.baeldung.com/cron-expressions>

## 1。概述

简而言之，cron 是基于 Unix 的系统上可用的基本实用程序。它使用户能够安排任务在指定的日期/时间定期运行。它自然是自动化大量流程运行的一个很好的工具，否则将需要人工干预。

Cron 作为守护进程运行。这意味着它只需要启动一次，它将继续在后台运行。这个过程利用`crontab`来读取时间表的条目并启动任务。

随着时间的推移，**cron 表达式格式被广泛采用**，许多其他程序和库也在使用它。

## 延伸阅读:

## [石英简介](/web/20220926190646/https://www.baeldung.com/quartz)

Learn how to schedule jobs with the Quartz API.[Read more](/web/20220926190646/https://www.baeldung.com/quartz) →

## [春季@预定标注](/web/20220926190646/https://www.baeldung.com/spring-scheduled-tasks)

How to use the @Scheduled annotation in Spring, to run tasks after a fixed delay, at a fixed rate or according to a cron expression.[Read more](/web/20220926190646/https://www.baeldung.com/spring-scheduled-tasks) →

## 2。与`Crontab`一起工作

一个`cron`时间表是一个简单的文本文件，位于 Linux 系统的`/var/spool/cron/crontabs`下。**我们不能直接编辑`crontab`文件**，所以我们需要使用`crontab` 命令来访问它。

要打开`crontab`文件，我们需要发出这个命令:

```
crontab -e
```

`crontab` 中的每一行都是一个条目，包含一个表达式和一个要运行的命令:

```
* * * * * /usr/local/ispconfig/server/server.sh
```

这个条目每分钟运行一次提到的脚本。

## 3。Cron 表情

先来理解一下`cron` 的表达。

它由五个字段组成:

```
<minute> <hour> <day-of-month> <month> <day-of-week> <command>
```

### 3.1。表达式中的特殊字符

*   ***(全部)**指定事件应该在每个时间单位发生。例如，< `minute>`字段中的“*”表示“每分钟”
*   **？在< `day-of-month>`和< `day-of -week>` 字段中使用【任意】**来表示任意值，因此忽略字段值。例如，如果我们想在“每月 5 号”启动一个脚本，而不管那天是星期几，我们指定一个“？”在<的`day-of-week>`领域。
*   **–(范围)**决定取值范围。例如，`<hour>`字段中的“10-11”表示“第 10 个和第 11 个小时”
*   **，【值】**指定多个值。例如，< `day-of-week>`字段中的“周一、周三、周五`“`表示“周一、周三和周五”。"
*   **/【增量】**指定增量值。例如，< `minute>` 字段中的“5/15”表示“一小时的 5、20、35 和 50 分钟”
*   **L (last)** 用在各个领域有不同的意思。例如，如果应用于< `day-of-month>` 字段，则表示该月的最后一天，即“1 月 31 日”，以此类推。它可以与偏移值一起使用，如“L-3”，表示“日历月的倒数第三天”在< `day-of-week>`中，指定“一周的最后一天”它也可以和< `day-of-week>`中的另一个值一起使用，比如“6L”，表示“上周五”
*   **W(工作日)**确定最接近一个月中给定日期的工作日(周一至周五)。例如，如果我们在< `day-of-month>` 字段中指定“10W”，则表示“该月 10 日附近的工作日”因此，如果“10 号”是星期六，作业将在“9 号”触发，如果“10 号”是星期天，作业将在“11 号”触发如果我们在< `day-of-month>`中指定“1W”，并且如果“1 日”是星期六，那么作业将在“3 日”触发，也就是星期一，并且不会跳回到上个月。
*   **#** 指定一个月中某个工作日的第 N 次发生，例如，“该月的第三个星期五”可以表示为“6#3”。

### 3.2。Cron 表达式示例

让我们看一些使用字段和特殊字符组合的`cron`表达式的例子:

**每天中午 12 点**:

```
0 12 * * ?
```

**每五分钟一次，从下午 1 点开始，到下午 1 点 55 分结束，然后从下午 6 点开始，到下午 6 点 55 分结束，每天**:

```
0/5 13,18 * * ?
```

**每天从下午 1 点开始到下午 1:05 结束的每一分钟**:

```
0-5 13 * * ?
```

**6 月份每周二下午 1:15 和 1:45**:

```
15,45 13 ? 6 Tue
```

**每周一、二、三、四、五上午 9:30**:

```
30 9 ? * MON-FRI
```

**每月 15 日上午 9:30**:

```
30 9 15 * ?
```

**每月最后一天下午 6 点**:

```
0 18 L * ?
```

**每月倒数第三天下午 6 点**:

```
0 18 L-3 * ?
```

**每月最后一个星期四上午 10:30**:

```
30 10 ? * 5L
```

**每月第三个星期一上午 10 点**:

```
0 10 ? * 2#3
```

**从本月 10 日开始的五天中，每天午夜 12 点**:

```
0 0 10/5 * ?
```

## 4。Cron 特殊字符串

除了 cron 表达式中指定的字段之外，还支持一些特殊的预定义值，我们可以使用这些值来代替字段:

*   **`@reboot`**–启动时运行一次
*   `**@yearly**`或 `**@annualy**`–每年运行一次
*   **`@monthly`**–每月运行一次
*   **`@weekly`**–每周运行一次
*   **`@daily`** 或 **`@midnight`**–每天运行一次
*   **`@hourly`**–每小时运行一次

## 5。结论

在这篇简短的文章中，我们探讨了`cron`工作和`crontab`。

我们还看到了一些可以在日常工作中使用的表达示例，或者简单地从这些示例中推断出其他表达。
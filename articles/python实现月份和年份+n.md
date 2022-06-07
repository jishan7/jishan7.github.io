# python实现月份和年份+n

需求是，要返回xxxx年xx月xx日的下n个月或下n个年的同一天的日期数据结构

如果下n个月没有这一天，则返回下n个月的最后一天

比如2015年1月31日的下个月同一天是2月28日

代码如下：


```python
import datetime
import calendar

def add_months(dt, months):
    month = dt.month - 1 + months
    year = dt.year + month / 12
    month = month % 12 + 1
    day = min(dt.day, calendar.monthrange(year, month)[1])
    return dt.replace(year=year, month=month, day=day)

def add_years(dt, n):
    year = dt.year + n
    month = dt.month
    day = min(dt.day, calendar.monthrange(year, month)[1])
    return dt.replace(year=year, month=month, day=day)

print(add_months(datetime.datetime.now(), 1))
print(add_years(datetime.datetime.now(), 1))

```


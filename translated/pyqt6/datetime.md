# PyQt6 日期和时间
*最后更新于 2021.04.23*

## QDate, QTime, QDateTime

PyQt6 有 `QDate`, `QDateTime`, `QTime` 类处理日期和时间。`QDate` 是用于处理公历中的日期的类。它有获取、比较或操作日期的方法。`QTime` 类用于处理时间。它提供了比较时间、确定时间和其他各种时间操作方法。`QDateTime` 是 `QDate` 和 `QTime` 的组合。

## PyQt 当前日期和时间

PyQt6 有 `currentDate`、`currentTime` 和 `currentDateTime` 方法获取当前的日期或时间。

``` python 
# file: current_date_time.py
#!/usr/bin/python

from PyQt6.QtCore import QDate, QTime, QDateTime, Qt

now = QDate.currentDate()

print(now.toString(Qt.DateFormat.ISODate))
print(now.toString(Qt.DateFormat.RFC2822Date))

datetime = QDateTime.currentDateTime()

print(datetime.toString())

time = QTime.currentTime()
print(time.toString(Qt.DateFormat.ISODate))
```

上面的代码打印出了当前日期，当前日期和时间，不同格式的时间。

``` python
now = QDate.currentDate()
```

`currentDate` 方法返回当前的日期。

``` python
print(now.toString(Qt.DateFormat.ISODate))
print(now.toString(Qt.DateFormat.RFC2822Date))
```
给 `toString` 传入不同的参数： `Qt.DateFormat.ISODate` 和 `Qt.DateFormat.RFC2822Date` 获取不同格式的日期。

```python 
datetime = QDateTime.currentDateTime()
```

`currentDateTime` 方法返回当前的日期和时间。

``` python
time = QTime.currentTime()
```

`currentTime` 方法返回了当前时间。

``` sh
$ ./current_date_time.py 
2021-04-23
23 Apr 2021
Fri Apr 23 11:41:39 2021
11:41:39
```
## PyQt6 UTC 时间
我们的星球是一个球体，绕着它自己的轴旋转。地球向东旋转，所以太阳在不同的时间在不同的地点升起。地球大约每24小时自转一次。因此，世界被划分为24个时区。在每个时区，都有不同的当地时间。当地时间通常会被夏时制进一步修改。

实际上也需要一个标准时间。一个标准时间有助于避免时区和夏令时的混淆。选择UTC(通用协调时间)作为主要的时间标准。UTC时间用于航空、天气预报、飞行计划、空中交通管制许可和地图。与当地时间不同，UTC时间不随季节变化而变化。

``` python
# file: utc_local.py
#!/usr/bin/python

from PyQt6.QtCore import QDateTime, Qt

now = QDateTime.currentDateTime()

print('Local datetime: ', now.toString(Qt.DateFormat.ISODate))
print('Universal datetime: ', now.toUTC().toString(Qt.DateFormat.ISODate))

print(f'The offset from UTC is: {now.offsetFromUtc()} seconds')
本例获取了标准时间和本地时间。

print('Local datetime: ', now.toString(Qt.DateFormat.ISODate))
```
`currentDateTime` 方法返回了本地时间的当前时间。我们可以使用 `toLocalTime` 方法把标准时间转换成本地时间。

``` python
print('Universal datetime: ', now.toUTC().toString(Qt.DateFormat.ISODate))
```

我们使用 `toUTC` 方法从时间对象里获取了标准时间。

``` python
print(f'The offset from UTC is: {now.offsetFromUtc()} seconds')
```

`offsetFromUtc` 方法给出了本地时间与标准时间的差，以秒为单位。

``` sh
$ ./utc_local.py 
Local datetime:  2021-04-23T11:44:15
Universal datetime:  2021-04-23T09:44:15Z
The offset from UTC is: 7200 seconds
```
## PyQt6 天数
`daysInMonth` 方法返回了指定月份的天数，`daysInYear` 方法返回了指定年份的天数。

``` python
# file: n_of_days.py
#!/usr/bin/python

from PyQt6.QtCore import QDate

now = QDate.currentDate()

d = QDate(1945, 5, 7)

print(f'Days in month: {d.daysInMonth()}')
print(f'Days in year: {d.daysInYear()}')
```
本例打印了指定年份和月份的天数。
``` sh
$ ./n_of_days.py 
Days in month: 31
Days in year: 365
```

## PyQt6 天数差
`daysTo` 方法返回了一个日期到另外一个日期的差。

``` python
# file: xmas.py
#!/usr/bin/python

from PyQt6.QtCore import QDate, Qt

now = QDate.currentDate()
y = now.year()

print(f'today is {now.toString(Qt.DateFormat.ISODate)}')

xmas1 = QDate(y-1, 12, 25)
xmas2 = QDate(y, 12, 25)

dayspassed = xmas1.daysTo(now)
print(f'{dayspassed} days have passed since last XMas')

nofdays = now.daysTo(xmas2)
print(f'There are {nofdays} days until next XMas')
```
该示例计算出了从上一个圣诞节到下一个圣诞节的天数。

``` sh
$ ./xmas.py
today is 2021-04-23
119 days have passed since last XMas
There are 246 days until next XMas
```
## PyQt6 时间的计算
我们经常需要对天，秒或者年进行加减等计算。
``` python
# file: arithmetic.py
#!/usr/bin/python

from PyQt6.QtCore import QDateTime, Qt

now = QDateTime.currentDateTime()

print(f'Today: {now.toString(Qt.DateFormat.ISODate)}')
print(f'Adding 12 days: {now.addDays(12).toString(Qt.DateFormat.ISODate)}')
print(f'Subtracting 22 days: {now.addDays(-22).toString(Qt.DateFormat.ISODate)}')

print(f'Adding 50 seconds: {now.addSecs(50).toString(Qt.DateFormat.ISODate)}')
print(f'Adding 3 months: {now.addMonths(3).toString(Qt.DateFormat.ISODate)}')
print(f'Adding 12 years: {now.addYears(12).toString(Qt.DateFormat.ISODate)}')
```
该示例展示了对当前日期时间添加或减去天、秒、月或年。

``` sh
$ ./arithmetic.py 
Today: 2021-04-23T12:06:10
Adding 12 days: 2021-05-05T12:06:10
Subtracting 22 days: 2021-04-01T12:06:10
Adding 50 seconds: 2021-04-23T12:07:00
Adding 3 months: 2021-07-23T12:06:10
Adding 12 years: 2033-04-23T12:06:10
```
## PyQt6 daylight saving time
夏令时 (DST) 是在夏季调快时间，使晚上变得更长。 初春时调前调一小时，秋季时调后调至标准时间。
``` python
# file: daylight_saving.py
#!/usr/bin/python

from PyQt6.QtCore import QDateTime, QTimeZone, Qt

now = QDateTime.currentDateTime()

print(f'Time zone: {now.timeZoneAbbreviation()}')

if now.isDaylightTime():
    print('The current date falls into DST time')
else:
    print('The current date does not fall into DST time')
```    
该例判断一个时间是不是夏令时。

```python 
print(f'Time zone: {now.timeZoneAbbreviation()}')
```

`timeZoneAbbreviation` 方法返回了时区的缩写。

``` python
if now.isDaylightTime():
...
```
`isDaylightTime` 判断日期是不是夏令时。

``` sh
$ ./daylight_saving.py 
Time zone: CEST
The current date falls into DST time
```
当前日期属于夏令时时间，中欧城市布拉迪斯拉发在夏季执行。 中欧夏令时 (CEST) 比世界时间早 2 小时。 该时区是夏令时时区，用于欧洲和南极洲。

## PyQt6 unix 纪元
纪元是被选为特定纪元起源的时间瞬间。 例如，在西方基督教国家，时间纪元从耶稣诞生的第 0 天开始。 另一个例子是使用了十二年的法国共和历。 这个时代是共和时代的开始，1792 年 9 月 22 日宣布第一共和国成立，君主制也被废除。

计算机也有它的时代。 最受欢迎的时代之一是 Unix 时代。 Unix 纪元是 1970 年 1 月 1 日 UTC 时间 00:00:00（或 1970-01-01T00:00:00Z ISO 8601）。 计算机中的日期和时间是根据自该计算机或平台定义的纪元以来经过的秒数或时钟滴答数确定的。

Unix 时间是自 Unix 纪元以来经过的秒数。

``` sh
$ date +%s
1619172620
```
Unix date 命令可用于获取 Unix 时间。 在这个特殊的时刻，自 Unix 时代以来已经过去了 1619172620 秒。

``` python
# file: unix_time.py
#!/usr/bin/python

from PyQt6.QtCore import QDateTime, Qt

now = QDateTime.currentDateTime()

unix_time = now.toSecsSinceEpoch() 
print(unix_time)

d = QDateTime.fromSecsSinceEpoch(unix_time)
print(d.toString(Qt.DateFormat.ISODate))
```
该例展示了 Unix 时间，并把它转换成了 `QDateTime`。

``` python
now = QDateTime.currentDateTime()
```

首先获取当前的日期和时间。

```python 
unix_time = now.toSecsSinceEpoch()
```

`toSecsSinceEpoch` 返回了 Unix 时间。

```python 
d = QDateTime.fromSecsSinceEpoch(unix_time)
```

使用 `fromSecsSinceEpoch` 方法把 Unix 时间转换成 `QDateTime`。

``` sh
$ ./unix_time.py 
1619172679
2021-04-23T12:11:19
```
## PyQt6 Julian day
儒略日是指自儒略时期开始以来的连续天数。 它主要由天文学家使用。 它不应与儒略历混淆。 它开始于公元前 4713 年。第 0 天是公元前 4713 年 1 月 1 日中午的那天。

儒略日数 (JDN) 是自此期间开始以来经过的天数。 任何时刻的儒略日期 (JD) 是前一个中午的儒略日数加上自该时刻起当天的分数。 （Qt 不计算这个分数。）除了天文学，儒略日期经常被军事和大型机程序使用。

``` python
# file: julian_day.py
#!/usr/bin/python

from PyQt6.QtCore import QDate, Qt

now = QDate.currentDate()

print('Gregorian date for today:', now.toString(Qt.DateFormat.ISODate))
print('Julian day for today:', now.toJulianDay())
```
该例中，我们得到了今天在公历中的表达和在儒略日的表达。

``` python 
print('Julian day for today:', now.toJulianDay())
```

`toJulianDay()` 返回了儒略日的日期。

``` sh
$ ./julian_day.py 
Gregorian date for today: 2021-04-23
Julian day for today: 2459328
```
## 历史战役
使用儒略日可以进行跨越几个世纪的计算。

``` python
# file: battles.py
#!/usr/bin/python

from PyQt6.QtCore import QDate, Qt

borodino_battle = QDate(1812, 9, 7)
slavkov_battle = QDate(1805, 12, 2)

now = QDate.currentDate()

j_today = now.toJulianDay()
j_borodino = borodino_battle.toJulianDay()
j_slavkov = slavkov_battle.toJulianDay()

d1 = j_today - j_slavkov
d2 = j_today - j_borodino

print(f'Days since Slavkov battle: {d1}')
print(f'Days since Borodino battle: {d2}')
```
该示例计算自两个历史事件以来经过的天数。

``` python
borodino_battle = QDate(1812, 9, 7)
slavkov_battle = QDate(1805, 12, 2)
```
这是两个拿破仑战斗日期。

```python
j_today = now.toJulianDay()
j_borodino = borodino_battle.toJulianDay()
j_slavkov = slavkov_battle.toJulianDay()
```
这是今天以及斯拉夫科夫和博罗季诺战役的儒略日。

``` python
d1 = j_today - j_slavkov
d2 = j_today - j_borodino
```
这是两次大战的天数差。

``` sh
$ ./battles.py 
Days since Slavkov battle: 78670
Days since Borodino battle: 76199
```
当我们运行这个脚本时，距离斯拉夫科夫战役已经过去了 78670 天，距离博罗季诺战役已经过去了 76199 天。

在 PyQt6 教程的这一部分中，我们使用了日期和时间。
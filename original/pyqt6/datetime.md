# PyQt6 date and time
*last modified April 23, 2021*

This part of the PyQt6 tutorial shows how to work with date and time in PyQt6.

## QDate, QTime, QDateTime

PyQt6 has `QDate`, `QDateTime`, `QTime` classes to work with date and time. The `QDate` is a class for working with a calendar date in the Gregorian calendar. It has methods for determining the date, comparing, or manipulating dates. The `QTime` class works with a clock time. It provides methods for comparing time, determining the time and various other time manipulating methods. The `QDateTime` is a class that combines both `QDate` and `QTime` objects into one object.

## PyQt current date and time

PyQt6 has `currentDate`, `currentTime` and `currentDateTime` methods for determining current date and time.

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

The example prints the current date, date and time, and time in various formats.

``` python
now = QDate.currentDate()
```

The `currentDate` method returns the current date.

``` python
print(now.toString(Qt.DateFormat.ISODate))
print(now.toString(Qt.DateFormat.RFC2822Date))
```
The date is printed in two different formats by passing the values Qt.DateFormat.ISODate and `Qt.DateFormat.RFC2822Date` to the toString method.

```python 
datetime = QDateTime.currentDateTime()
```

The `currentDateTime` returns the current date and time.

``` python
time = QTime.currentTime()
```

Finally, the `currentTime` method returns the current time.

``` sh
$ ./current_date_time.py 
2021-04-23
23 Apr 2021
Fri Apr 23 11:41:39 2021
11:41:39
```
## PyQt6 UTC time
Our planet is a sphere; it revolves round its axis. The Earth rotates towards the east, so the Sun rises at different times in different locations. The Earth rotates once in about 24 hours. Therefore, the world was divided into 24 time zones. In each time zone, there is a different local time. This local time is often further modified by the daylight saving.

There is a pragmatic need for one global time. One global time helps to avoid confusion about time zones and daylight saving time. The UTC (Universal Coordinated time) was chosen to be the primary time standard. UTC is used in aviation, weather forecasts, flight plans, air traffic control clearances, and maps. Unlike local time, UTC does not change with a change of seasons.
``` python
# file: utc_local.py
#!/usr/bin/python

from PyQt6.QtCore import QDateTime, Qt

now = QDateTime.currentDateTime()

print('Local datetime: ', now.toString(Qt.DateFormat.ISODate))
print('Universal datetime: ', now.toUTC().toString(Qt.DateFormat.ISODate))

print(f'The offset from UTC is: {now.offsetFromUtc()} seconds')
The example determines the current universal and local date and time.

print('Local datetime: ', now.toString(Qt.DateFormat.ISODate))
```
The `currentDateTime` method returns the current date and time expressed as local time. We can use the `toLocalTime` to convert a universal time into a local time.

``` python
print('Universal datetime: ', now.toUTC().toString(Qt.DateFormat.ISODate))
```

We get the universal time with the `toUTC` method from the date time object.

``` python
print(f'The offset from UTC is: {now.offsetFromUtc()} seconds')
```

The `offsetFromUtc` gives the difference between universal time and local time in seconds.

``` sh
$ ./utc_local.py 
Local datetime:  2021-04-23T11:44:15
Universal datetime:  2021-04-23T09:44:15Z
The offset from UTC is: 7200 seconds
```
## PyQt6 number of days
The number of days in a particular month is returned by the `daysInMonth` method and the number of days in a year by the `daysInYear` method.

``` python
# file: n_of_days.py
#!/usr/bin/python

from PyQt6.QtCore import QDate

now = QDate.currentDate()

d = QDate(1945, 5, 7)

print(f'Days in month: {d.daysInMonth()}')
print(f'Days in year: {d.daysInYear()}')
```
The example prints the number of days in a month and year for the chosen date.
``` sh
$ ./n_of_days.py 
Days in month: 31
Days in year: 365
```

## PyQt6 difference in days
The `daysTo` method returns the number of days from a date to another date.

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
The example calculates the number of days passed from the last XMas and the number of days until the next XMas.

``` sh
$ ./xmas.py
today is 2021-04-23
119 days have passed since last XMas
There are 246 days until next XMas
```
## PyQt6 datetime arithmetic
We often need to add or subtract days, seconds, or years to a datetime value.
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
The example determines the current datetime and add or subtract days, seconds, months, and years.

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
Daylight saving time (DST) is the practice of advancing clocks during summer months so that evening daylight lasts longer. The time is adjusted forward one hour in the beginning of spring and adjusted backward in the autumn to standard time.

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
The example checks if the datetime is in the daylight saving time.

```python 
print(f'Time zone: {now.timeZoneAbbreviation()}')
```

The `timeZoneAbbreviation` method returns the time zone abbreviation for the datetime.

``` python
if now.isDaylightTime():
...
```
The `isDaylightTime` returns if the datetime falls in daylight saving time.

``` sh
$ ./daylight_saving.py 
Time zone: CEST
The current date falls into DST time
```
The current date falls into DST time The program was executed in Bratislava, which is a city in Central Europe, during summer. Central European Summer Time (CEST) is 2 hours ahead of universtal time. This time zone is a daylight saving time time zone and is used in Europe and Antarctica. The standard time, which is used in winter, is Central European Time (CET).

## PyQt6 unix epoch
An epoch is an instant in time chosen as the origin of a particular era. For example in western Christian countries the time epoch starts from day 0, when Jesus was born. Another example is the French Republican Calendar which was used for twelve years. The epoch was the beginning of the Republican Era which was proclaimed on September 22, 1792, the day the First Republic was declared and the monarchy abolished.

Computers have their epochs too. One of the most popular is the Unix epoch. The Unix epoch is the time 00:00:00 UTC on 1 January 1970 (or 1970- 01-01T00:00:00Z ISO 8601). The date and time in a computer is determined according to the number of seconds or clock ticks that have elapsed since the defined epoch for that computer or platform.

Unix time is the number of seconds elapsed since Unix epoch.

``` sh
$ date +%s
1619172620
```
Unix date command can be used to get the Unix time. At this particular moment, 1619172620 seconds have passed since the Unix epoch.

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
The example prints the Unix time and converts it back to the `QDateTime`.

``` python
now = QDateTime.currentDateTime()
```

First, we retrieve the current date and time.

```python 
unix_time = now.toSecsSinceEpoch()
```

The `toSecsSinceEpoch` returns the Unix time.

```python 
d = QDateTime.fromSecsSinceEpoch(unix_time)
```

With the `fromSecsSinceEpoch` we convert the Unix time to `QDateTime`.

``` sh
$ ./unix_time.py 
1619172679
2021-04-23T12:11:19
```
## PyQt6 Julian day
Julian day refers to a continuous count of days since the beginning of the Julian Period. It is used primarily by astronomers. It should not be confused with the Julian calendar. The Julian Period started in 4713 BC. The Julian day number 0 is assigned to the day starting at noon on January 1, 4713 BC.

The Julian Day Number (JDN) is the number of days elapsed since the beginning of this period. The Julian Date (JD) of any instant is the Julian day number for the preceding noon plus the fraction of the day since that instant. (Qt does not compute this fraction.) Apart from astronomy, Julian dates are often used by military and mainframe programs.

``` python
# file: julian_day.py
#!/usr/bin/python

from PyQt6.QtCore import QDate, Qt

now = QDate.currentDate()

print('Gregorian date for today:', now.toString(Qt.DateFormat.ISODate))
print('Julian day for today:', now.toJulianDay())
```
In the example, we compute the Gregorian date and the Julian day for today.

``` python 
print('Julian day for today:', now.toJulianDay())
```

The Julian day is returned with the toJulianDay() method.

``` sh
$ ./julian_day.py 
Gregorian date for today: 2021-04-23
Julian day for today: 2459328
```
## Historical battles
With Julian day it is possible to do calculations that span centuries.

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
The example counts the number of days passed since two historical events.

``` python
borodino_battle = QDate(1812, 9, 7)
slavkov_battle = QDate(1805, 12, 2)
```
We have two dates of battles of the Napoleonic era.

```python
j_today = now.toJulianDay()
j_borodino = borodino_battle.toJulianDay()
j_slavkov = slavkov_battle.toJulianDay()
```
We compute the Julian days for today and for the Battles of Slavkov and Borodino.

``` python
d1 = j_today - j_slavkov
d2 = j_today - j_borodino
```
We compute the number of days passed since the two battles.

``` sh
$ ./battles.py 
Days since Slavkov battle: 78670
Days since Borodino battle: 76199
```
When we run this script, 78670 days have passed since the Slavkov battle, and 76199 since the Borodino battle.

In this part of the PyQt6 tutorial, we have worked with date and time.
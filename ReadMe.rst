`timez` --- Time package supporting chain call
==================================================

timez is a time package supporting chain call.

install and use:

::

    pip install timez


Example
-------

::

    from timez import timez

    # get datetime str of previous day with specified hours
    >>> timez(f'{timez.now().delta(days=-1).date_str()} 10:00:00').datetime_str()
    '2021-04-04 10:00:00'

    # get unix timestamp of previous day
    >>> timez.now().delta(days=-1).uts(is_ms=True)
    1627211818635

    # get corresponding unix timestamp with specified datetime str
    >>> timez('2021-04-04 18:23:12').uts()
    1617531792

    # get datetime str of UTC time which is converted and down sampled by local time
    >>> timez.now().delta(hours=8).ds(minutes=5).datetime_str()
    '2021-04-04 08:05:00'

Initialize
----------
There are 5 ways to initialize an timez object.
::

    # 1. init with time str, format '%Y{}%m{}%d', '%Y{}%m{}%d %H{}%M{}%S' date_sep could be '','-','/', time_sep could be '',':'.
    >>> timez('20210404')
    >>> timez('2021-04-04 18:23:12')
    >>> timez('2021/04/04 18:23:12')

    # 2. init with unix timestamp, support second and milliseconds, default as second
    >>> timez(1617531792)
    >>> timez(1617531792000, is_ms=True)    # if is milliseconds, is_ms=True
    >>> timez(1617531792.123)               # float is also supported

    # 3. init with custom datetime str, use timez.strp(time: str, fmt: str)
    >>> timez.strp('2021-04-04 18:23', fmt='%Y-%m-%d %H:%M')

    # 4. init datetime.datetime object
    >>> dt = datetime.datetime.now()
    >>> timez(dt)

    # 5. init with timetuple
    >>> timez((2021, 4, 4, 18, 23, 12))
    >>> timez(['2021', '04', '04', '18', '23', '12'])


class timez
---------------


* timez.now() -> timez
    get current local time.
* timez.today() -> timez
    get current date, hour minute second is 00:00:00.
* timez.strp(time: str, fmt: str) -> timez
    init timez from custom time str format.
* timez.uts(is_ms=False) -> int
    get unix timestamp, if is_ms=True, get milliseconds.
* timez.date_str(date_sep='-') -> str
    get date str, sep include '', '-', '/'.
* time_str(time_sep=':') -> str
    get time str, 'time_sep' is sep include '', ':'.
* datetime_str(date_sep='-', time_sep=':') -> str
    get datetime str, date_sep and time_sep same to above.
* join(datetime_str: str, fmt: str) -> timez
    join timez self with given time str.
    Notice: There is no date or time range checking, be careful

::
    >>> timez('2021-04-04 18:23:12').join('23:59:59').__str__()
    '2021-04-04 23:59:59'
    >>> timez('2021-04-04 23:59:59').join('10 235959', fmt='%d %H%M%S').__str__()
    '2021-04-10 23:59:59'
    >>> timez('2021-04-04 18:23:12').join('10', fmt='%d').__str__()
    '2021-04-10 18:23:12'


* strf(fmt) -> str
    get custom time str with given fmt.
* pop() -> datetime.datetime
    get datetime.datetime object from timez instance
* delta(days=0, seconds=0, minutes=0, hours=0) -> timez
    get offset time.
* ds(hours=None, minutes=None, seconds=None) -> timez
    down sample time, example as follows.

::
    >>> timez('2021-04-04 18:23:12').ds(hours=5).__str__()
    '2021-04-04 15:23:12'
    >>> timez('2021-04-04 18:23:12').ds(minutes=5).__str__()
    '2021-04-04 18:20:12'
    >>> timez('2021-04-04 18:23:12').ds(seconds=5).__str__()
    '2021-04-04 18:23:10'
    >>> timez('2021-04-04 18:23:12').ds(minutes=5, seconds=0).__str__()
    '2021-04-04 18:20:00'
    >>> timez('2021-04-04 18:23:12').ds(hours=0, minutes=0, seconds=0).__str__()
    '2021-04-04 00:00:00'
    >>> timez('2021-04-04 18:23:12').ds(hours=17, minutes=5, seconds=5).__str__()
    '2021-04-04 17:20:10'

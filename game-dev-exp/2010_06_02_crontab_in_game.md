[game-dev] 游戏中的 crontab

最近给游戏中加了个 crontab 的功能，可以让每个写特定逻辑的人撰写一份 crontab spec，然后注册之。然后 crontab daemon 保证到时调用特定模块的特定函数。

crontab 其实是个实用的功能，比如：一个玩法活动，要在 2010.06.01 08:00 - 18:00 进行。08:00 活动开始，刷出一些NPC；18:00 结束，回收NPC。则到点调用两个对应的函数，非常方便地控制了触发逻辑。

今天参考了下 FreeBSD 下 crond / crontab 的实现（/usr/src/usr.sbin/cron），就是把所有 event 丢到 linked list 里面，然后每分钟遍历一次。通过 localtime( curr_time ) 得到当前时间的 year, month, mday, week, wday, hour, sec，并与 event 匹配，满足条件，则触发对应的 event。如果有用户执行了 crontab，增加了 event，则重新加载一下 crontab spec。

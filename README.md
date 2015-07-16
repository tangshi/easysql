# Easy SQL

抽取自[廖雪峰的Python2.7教程-实战](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001397616003925a3d157284cd24bc0952d6c4a7c9d8c55000)中数据库操作相关模块。

原有代码是基于mysql的python驱动写成的，使用方法戳[这里](mysql.md)。

我对代码稍作改动将之转换为对 sqlite3 的操作，主要基于以下原因：

1. python 自带 sqlite3 驱动，不需要额外安装
2. Mac OS X 自带 sqlite3，不需要安装，mysql则需要自行下载并安装
3. sqlite3 非常轻便，相比较而言，mysql还是太笨重了，做点小应用 sqlite3 足够了

sqlite3 版的使用方法，请戳[这里](sqlite3.md)


# oksqls
oksql项目可以进行sql语句的拼接，可以做到不写sql语句进行sql查询。
该项目使用kotlin编写，可以衔接java和kotlin项目，内置中文注释但是不多，类似于mybites但又完全不一样
编写该库主要为了方便sql查询，底层采用jdbc，第一次编写工具库，由衷的希望各位大佬提出意见和修改方案，不甚感激

首次使用oksql首先将oksql下载下来，可以直接依赖oksql：1.0.0，这个目前只能手动添加依赖，在依赖好后在资源文件夹resource创建一个sqls.xml
将这些复制进去

<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE sqls>
<sqls>
    <sql id="1"
         dbdriver="com.mysql.cj.jdbc.Driver"
         url="jdbc:mysql://你数据库的ip地址或者本地localhost之类的:3306/数据库名"
         name="用户名"
         password="密码"/>
    <sql id="2"
         dbdriver="com.mysql.cj.jdbc.Driver"
         url="jdbc:mysql://和上面一样:3306/数据库名"
         name="用户名"
         password="密码"/>
</sqls>


我这里默认用的是mysql，java的jdbc技术应该可以换数据库的，这个需要改dbdriver那一栏，当然如果本来就用mysql的同志就不用改了
url当然是填写您自己的数据库url，mysql的端口一般是3306当然有一些同志改了端口那这个也要改
用户名密码都是您数据库的用户名和密码

大家要注意不是只能写两个，您有几个数据库就写几个，id这个区分开就像了，也不需要一定是1，2，3，4因为后续转成spring类型的进行判断，id要记住，一会要用

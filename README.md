# oksqls
oksql项目可以进行sql语句的拼接，可以做到不写sql语句进行sql查询。
该项目使用kotlin编写，可以衔接java和kotlin项目，内置中文注释但是不多，类似于mybites但又完全不一样
编写该库主要为了方便sql查询，底层采用jdbc，第一次编写工具库，由衷的希望各位大佬提出意见和修改方案，不甚感激

首次使用oksql首先将oksql的jar包和sqls.xml下载下来，可以直接依赖oksql：1.0.0，这个目前只能手动添加依赖，在依赖好后创建资源文件夹resource将其标记为资源文件后将sqls.xml复制进去

1.更改sqls.xml
我这里默认用的是mysql，java的jdbc技术应该可以换数据库的，这个需要改dbdriver那一栏，当然如果本来就用mysql的同志就不用改了
url当然是填写您自己的数据库url，mysql的端口一般是3306当然有一些同志改了端口那这个也要改
用户名密码都是您数据库的用户名和密码

大家要注意不是只能写两个，您有几个数据库就写几个，id这个区分开就像了，也不需要一定是1，2，3，4因为后续转成spring类型的进行判断，id要记住，一会要用


2.开始查询，首先oksql默认调用sqls.xml中id为“1”的数据库，所以如果你不是和我一样id是用1234来划分的话就要
（1）首先更改SQLID这个全局变量，因为是全局变量在什么地方都可以改，如果是kotlin的项目，随便找个地方SQLID=“你自己的id，注意是string类型的”
如果是java项目的话就SQL_setsqlcontextKt.setSQLID("你自己的id");
（2）

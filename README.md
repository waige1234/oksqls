# oksqls
oksql项目可以进行sql语句的拼接，可以做到不写sql语句进行sql查询。
该项目使用kotlin编写，可以衔接java和kotlin项目，内置中文注释但是不多，类似于mybites但又完全不一样
编写该库主要为了方便sql查询，底层采用jdbc，第一次编写工具库，由衷的希望各位大佬提出意见和修改方案，不甚感激

首次使用oksql首先将oksql的jar包和sqls.xml下载下来，可以直接依赖oksql：1.0.0，这个目前只能手动添加依赖，在依赖好后创建资源文件夹resource将其标记为资源文件后将sqls.xml复制进去

在将oksql添加入库之后，先在项目结构/工件/把你当前的项目工件建立好然后，双击右侧的oksql：1.0.0，点击确定，不这样的话运行时会报错

不需要额外依赖josnarry相关的依赖，有内置的已经依赖好的

一.更改sqls.xml
我这里默认用的是mysql，java的jdbc技术应该可以换数据库的，这个需要改dbdriver那一栏，当然如果本来就用mysql的同志就不用改了
url当然是填写您自己的数据库url，mysql的端口一般是3306当然有一些同志改了端口那这个也要改
用户名密码都是您数据库的用户名和密码

大家要注意不是只能写两个，您有几个数据库就写几个，id这个区分开就像了，也不需要一定是1，2，3，4因为后续转成spring类型的进行判断，id要记住，一会要用

二.更改配置文件，首先oksql默认调用sqls.xml中id为“1”的数据库，所以如果你不是和我一样id是用1234来划分的话就要

三.更改全局变量（这个不一定要改）
1.首先更改SQLID这个全局变量，因为是全局变量在什么地方都可以改，如果是kotlin的项目，随便找个地方SQLID=“你自己的id，注意是string类型的”
如果是java项目的话就SQL_setsqlcontextKt.setSQLID("你自己的id");

2.ANDorOR，这个参数也有默认值，一般不改的话是“and”，要么改的话也就只能改成“or”，用于拼接sql语句的时候where xxx=xxx and xxx=xxx，如果ANDorOR的值为“or”
那这个sql语句救会变成where xxx=xxx or xxx=xxx。kotlin项目就直接改就行，java项目的话JC_SQLEXEKt.setANDorOR("");

3.LIKEorEQUIRorIS这个参数默认值是“=”，和上一个参数一样用于sql语句拼接，可以改成“is”或“like”，用于拼接sql语句的时候where xxx=xxx and xxx=xxx，
如果LIKEorEQUIRorIS的值为“is”或“like”，那这个sql语句救会变成where xxx is xxx and xxx is xxx或where xxx like xxx 


三.方法执行的参数。比如查询条件，查询内容和到底要查什么

1.EXE_tiaojian[]和EXE_neirong[]这两个数组是数据库操作的条件和内容，从0开始，如果EXE_tiaojian[0]=“id”和EXE_neirong[0]=“1”，那就是where id=1，当然
不是只能使用0，如果加上EXE_tiaojian[1]=“name”和EXE_neirong[1]=“崴哥”那就是where id=1 and name=崴哥，两个参数的索引树最多20个，当然空的没写的如果
不写会跳过像EXE_tiaojian[2]，EXE_neirong[2]不写的话也没有关系，不是说非得写满20个

kotlin的话可以直接EXE_tiaojian[索引值]=“xxx”，但如果是java就得先定义两个长度为20的string数组，然后先将条件和内容分别赋值进去然后
JC_SQLEXEKt.setEXE_neirong(内容数组的数组名);JC_SQLEXEKt.setEXE_tiaojian(条件数组的数组名名);

2.JS_zhi[]这个参数只有在做查询操作的时候用的到，他用来记录你要查的字段，用法和上面两个参数一样，如果是进行查询操作，这个不能不写，如果不是查询操作的
话就无所谓，kotlin用法JS_zhi[0]="xxx",java用法JC_SQLEXEKt.JS_zhi(条件数组的数组名名);

四.调用方法进行sql操作
1.查询方法
（1）kotlin的写法chasql.cha("要查询的表名")，java的写法chasql.INSTANCE.cha("要查询的表名");这个方法有返回值，是jsonArray
此查询方法会将第三步用到的参数拼接进入sql语句中进行查询，查询结果存入josnarray，想要调出来的话kotlin是chasql.JsonArray，java是chasql.INSTANCE.getJsonArray()

（2）kotlin的写法chasql.ChaOne("要查询的表名"),java是chasql.INSTANCE.ChaOne("要查询的表名");这个方法没有返回值
此查询方法针对的是只需要查出一行数据的需求，默认查询结果输出为string数组jieguo[]

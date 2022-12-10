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
此查询方法针对的是只需要查出一行数据的需求，默认查询结果输出为string数组jieguo[]，如果想让其输出为josn需要将RETURNTYPE的参数变更为“Json”请
一定要注意大小写区分kotlin写法RETURNTYPE="Json",java为ChasqlKt.setRETURNTYPE("Json");，调出查询结果时只需要chasql.jieguo[xxx]正常调出数组即可
如果是java，那就需要调用chasql.INSTANCE.getJieguo();返回值一个string数组

2.增加方法
kotlin写法zengsql.sqlzeng("表名")，java写法 zengsql.INSTANCE.sqlzeng("表名");这个方法执行完后会返回一个string类型的“1”
该方法会将EXE_tiaojian[]和EXE_neirong[]这两个数组里的值添加到对应的表里面

3.删除方法
kotlin写法shansql.sqlshan("表名")，java写法shansql.INSTANCE.sqlshan("表名");这个方法执行完后会返回一个int类型的1
该方法会按照EXE_tiaojian[]和EXE_neirong[]这两个数组里定义的条件删除对应的数据行

4.更改方法
kotlin写法gaisql.sqlgai("表名","要改的字段","改成什么")，java写法gaisql.INSTANCE.sqlgai("表名","要改的字段","改成什么");
该方法还有一个只有一个string参数的，那个不建议用，如果需要改的字段很多那就多写几次这个方法，EXE_tiaojian[]和EXE_neirong[]这两个数组的东西
不用每次都重写一遍，想清空里面的东西有一个方法待会说

5.数组清空
kotlin写法chasql.CSH(),java写法chasql.INSTANCE.CSH();
该方法会将JS_zhi[]，jieguo[]，EXE_tiaojian[]，EXE_neirong[]，和查询出的josn数组一块清空，直接就没了，用之前请确保需要的内容已经存在别的变量
里了，不然运行时一定会说“卧槽，怎么没了”

其实以上方法已经可以适用于大部分场景了，但是在项目开发中往往会遇到一些很变态的需求，需要各种各样的奇怪sql语句，为了方便各位的开发需求
接下来的内容就是为了应对各种场景，在原有基础上添加的扩展方法

五.复杂sql应用（基本都是没有测试过的但应该不会有问题，有问题的话一定要跟我说，我来改）
（1）自定义查询ChaDiy()，kotlin用法chasql.ChaDiy("自定义的sql语句"),java用法chasql.INSTANCE.ChaDiy("自定义的sql语句");
这个方法什么数组都不用去写，EXE_tiaojian[]，EXE_neirong[]里面有的在你自定义的sql语句中就有了，JS_zhi[]的内容会帮你自动填进去，就是没有测试过
同时默认结果存进json数组，想提出来一样是chasql.JsonArray和chasql.INSTANCE.getJsonArray()

（2）自定义查询ChaOneDiy(),kotlin用法chasql.ChaOneDiy("自定义的sql语句"),java用法chasql.INSTANCE.ChaOneDiy("自定义的sql语句");
和上面一个方法使用方式一样，但通常用于只查询一行数据的情况，默认存入jieguo[]，如果想让其输出为josn需要将RETURNTYPE的参数变更为“Json”这个和上面
一样，只要参数变更查询到的参数救会输出到JsonArray

（3）自定义更改gaiDIY()，kotlin用法gaisql.gaiDIY("自定义的sql语句"),java用法gaisql.INSTANCE.gaiDIY("自定义的sql语句");

（4）自定义增加zengDIY()，kotlin用法zengsql.zengDIY("自定义的sql语句"),java用法zengsql.INSTANCE.zengDIY("自定义的sql语句");

（5）自定义删除shanDIY(),kotlin用法shansql.shanDIY("自定义的sql语句"),java用法shansql.INSTANCE.shanDIY("自定义的sql语句");
这几个没什么好介绍的，只要给sql语句就能干活

（6）事务处理SWexe()，首先这个事务处理不能包含查询语句，另外使用事务处理之前需要提前准备好需要处理的sql语句，并将这些语句导入SW_sqls[]
这个string数组，上面说到oksql里有着EXE_tiaojian[]和EXE_neirong[]两个数组用于拼接sql语句，有的同志就要问了，那你怎么把拼接好的sql语句
拿出来呢？其实在sql操作的各个类里面有一个内置的pingjie方法可以将拼接的sql语句输出，
kotlin用法shansql.pingjie("表名")，zengsql.pingjie("表名")，gaisql.pingjie("表名","要改的字段","要改成什么")，
Java用法为shansql.INSTANCE.pingjie("表名");，zengsql.INSTANCE.pingjie("表名");，gaisql.INSTANCE.pingjie("表名","要改的字段","要改成什么");
，这些方法返回值为一个string字符串，查询的那个类也是有这样一个方法的，但是查询语句就算拿出来也没有什么用，就private了

接下来将得到的sql语句添加到SW_sqls[]数组中，kotlin的写法SW_sqls[0]="xxx",java的话就要先创建一个string数组，然后先将sql语句添加进那个数组再
JC_SQLEXEKt.setSW_sqls(刚刚新建的那个数组的名字);

然后运行gaisql.SWexe(),应为这是一个写在父类的方法所以chasql.SWexe()也是可以的java的写法就是shansql.INSTANCE.SWexe();没有返回值

gaisql的这个pingjie方法只有oksql的1.0.1版本有（因为写这段话的时候发现有点不太对劲，然后改了一下）


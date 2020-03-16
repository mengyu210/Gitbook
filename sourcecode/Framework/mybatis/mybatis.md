
## #{}和${}的区别是什么？
   ${} 是Properties文件中的变量占位符，它可以用于标签属性值和sql内部，属于静态文本替换，比如${driver}会被静态替换为com.mysql.jdbc.Driver。#{}是sql的参数占位符，Mybatis会将sql中的#{}替换为?号，在sql执行前会使用PreparedStatement的参数设置方法，按序给sql的?号占位符设置参数值，比如ps.setInt(0, parameterValue)，#{item.name}的取值方式为使用反射从参数对象中获取item对象的name属性值，相当于param.getItem().getName()
   #{}是预编译处理，$ {}是字符串替换
  - Mybatis在处理时，就是把 {}时，就是把时，就是把{}替换成变量的值。
  - #{}解析传递进来的参数数据
  - ${}对传递进来的参数原样拼接在SQL中
  - 使用#{}可以有效的防止SQL注入，提高系统安全性。

## Mybatis是如何进行分页的？分页插件的原理是什么？
   Mybatis使用RowBounds对象进行分页，它是针对ResultSet结果集执行的内存分页，而非物理分页，可以在sql内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。
分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql，然后重写sql，根据dialect方言，添加对应的物理分页语句和物理分页参数。
举例：select * from student，拦截sql后重写为：select t.* from （select * from student）t limit 0，10

## Mybatis动态sql是做什么的？都有哪些动态sql？能简述一下动态sql的执行原理不？
   Mybatis动态sql可以让我们在Xml映射文件内，以标签的形式编写动态sql，完成逻辑判断和动态拼接sql的功能，Mybatis提供了9种动态sql标签trim|where|set|foreach|if|choose|when|otherwise|bind。
其执行原理为，使用OGNL从sql参数对象中计算表达式的值，根据表达式的值动态拼接sql，以此来完成动态sql的功能


## Xml映射文件中，除了常见的select|insert|updae|delete标签之外，还有哪些标签？`   
##  Mybatis是否支持延迟加载？如果支持，它的实现原理是什么？
   Mybatis仅支持association关联对象和collection关联集合对象的延迟加载，association指的就是一对一，collection指的就是一对多查询。在Mybatis配置文件中，可以配置是否启用延迟加载lazyLoadingEnabled=true|false。
它的原理是，使用CGLIB创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用a.getB().getName()，拦截器invoke()方法发现a.getB()是null值，那么就会单独发送事先保存好的查询关联B对象的sql，把B查询上来，然后调用a.setB(b)，于是a的对象b属性就有值了，接着完成a.getB().getName()方法的调用。这就是延迟加载的基本原理。
当然了，不光是Mybatis，几乎所有的包括Hibernate，支持延迟加载的原理都是一样的。

## MyBatis编程步骤是什么样的?
   - ① 创建SqlSessionFactory
   - ② 通过SqlSessionFactory创建SqlSession
   - ③ 通过sqlsession执行数据库操作
   - ④ 调用session.commit()提交事务
   - ⑤ 调用session.close()关闭会话
## 为什么说Mybatis是半自动ORM映射工具？它与全自动的区别在哪里？
   Hibernate属于全自动ORM映射工具，使用Hibernate查询关联对象或者关联集合对象时，可以根据对象关系模型直接获取，所以它是全自动的。而Mybatis在查询关联对象或关联集合对象时，需要手动编写sql来完成，所以，称之为半自动ORM映射工具。
一对一、一对多的关联查询
association 一对一关联查询
collection 一对多关联查询

## MyBatis实现一对一有几种方式?具体怎么操作的？
   有联合查询和嵌套查询,联合查询是几个表联合查询,只查询一次, 通过在resultMap里面配置association节点配置一对一的类就可以完成; 嵌套查询是先查一个表,根据这个表里面 的结果的外键id,去再另外一个表里面查询数据,也是通过association配置,但另外一个表的查询通过select属性配置。

## MyBatis实现一对多有几种方式,怎么操作的？
   有联合查询和嵌套查询,联合查询是几个表联合查询,只查询一次,通过在resultMap里面配 置collection节点配置一对多的类就可以完成; 嵌套查询是先查一个表,根据这个表里面的 结果的外键id,去再另外一个表里面查询数据,也是通过配置collection,但另外一个表的查询通过select节点配置。

## IBatis和MyBatis在核心处理类分别叫什么？
   IBatis里面的核心处理类交SqlMapClient, MyBatis里面的核心处理类叫做SqlSession

## Mybatis比IBatis比较大的几个改进是什么？
   - 有接口绑定,包括注解绑定sql和xml绑定Sql ,
   - 动态sql由原来的节点配置变成OGNL表达式,
   - 在一对一,一对多的时候引进了association,在一对多的时候引入了collection 节点,不过都是在resultMap里面配置。

## mybatis一级缓存二级缓存 
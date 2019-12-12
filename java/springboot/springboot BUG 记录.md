# springboot BUG 记录 #

1.多模块引用，无法扫描到指定的自定义包

问题描述：

cn.booking 是我自定义的一个包，属于booking模块，其中有些controller接口，我需要在启动类中指定要扫描的包，否则spring扫不到，那我就调不到接口了

项目结构：

![image-20191212160236263](C:\Users\king_zl\AppData\Roaming\Typora\typora-user-images\image-20191212160236263.png)

目前报错：![image-20191212160315770](C:\Users\king_zl\AppData\Roaming\Typora\typora-user-images\image-20191212160315770.png)

启动类 JeecgApplication是在 module-system这个module中的，scanBasepackages 中无论我如何尝试 引入 `cn.booking.controller`都不能正确扫描到包

分析：base-common是基础模块，module-system是启动类所在的模块，booking是自定义的业务模块，其中 base和system的包名都是以org.jeecg开始的，

-------------------------------------

以这个多模块项目来分析，说一下个人见解，除system(启动类所在模块)，其他模块打包后应都是jar包，这样才能调用到对应方法，那么在idea中点击`run`之后这些个模块之间是怎么互相调用的呢？要知道这些个模块`run`之后是编译到了各自的target目录的，那jvm/idea是怎么知道应该调用哪个类的哪个方法的？答案就是`pom文件`

> system作为启动类模块，需要在pom中引入booking的依赖，并且需要指明booking模块版本号，这样idea/jvm就能找到对应的方法了。
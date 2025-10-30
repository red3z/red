# Maven

POM.xml (Project Object Model), Maven认为每个项目都是一个对象, 对象之间有依赖, 继承, 聚合关系.

默认的远程仓库是Apache的中央仓库.





# Maven工程类型

| Maven工程类型 |                                                       |
| ------------- | ----------------------------------------------------- |
| POM工程       | 逻辑工程, 用在父工程或聚合工程, 用来对Jar进行版本控制 |
| JAR工程       | 打包为Jar.                                            |
| WAR工程       | 打包为War.                                            |

SpringBoot 提供了一个插件spring-boot-maven-plugin用于把程序打包成一个可执行Jar包, SpringBoot有了这个插件后将项目打包为Fat jar(包含了所有依赖的jar包), java -jar命令会去执行jar中manifest文件写的启动类Start-Class.



# Maven依赖冲突

最短路径优先, 项目依赖树中路径短的被使用; 当路径长度相同时, 按声明顺序决定.

可以依赖排除.



# Maven依赖范围

| Maven依赖范围 |                                                           |
| ------------- | --------------------------------------------------------- |
| compile(默认) | 编译和运行都生效                                          |
| provided      | 表示该依赖已提供, Tomcat中有servlet, 所以不需要重复导入.  |
| runtime       | 仅运行时生效, JDBC编译时只需要接口, 运行时才需要具体实现. |
| system        | 使用指定路径的Jar.                                        |
| test          | 仅在编译和运行测试代码时生效                              |
| import        | 表示该依赖必须使用\<dependencyManagement>中的依赖.        |



# Maven插件

| 常用Maven插件                                            |                                                         |
| -------------------------------------------------------- | ------------------------------------------------------- |
| Tomcat插件 tomcat7-maven-plugin 运行web项目              |                                                         |
| 编译器插件maven-compiler-plugin 指定JDK版本              |                                                         |
| 资源拷贝插件 将<u>非resources</u>中的文件打包到classes中 | 配置文件一般放在resources中, 打包后放在target/classes中 |



# Maven命令

| Maven生命周期 |                                                              |
| ------------- | ------------------------------------------------------------ |
| clean         | 删除tartget                                                  |
| compile       | 编译, 相当于javac                                            |
| package       | javac编译 + jar打Jar包,                                      |
| install       | javac编译 + jar打Jar包, 并且将打好的包放入本地仓库和远程私服仓库 |
| site          |                                                              |
| deploy        |                                                              |








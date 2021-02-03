# maven常用命令集合（收藏大全）

## maven常用命令集合（收藏大全）

ydlmlh [java思维导图](maven-chang-yong-ming-ling-ji-he-shou-cang-da-quan.md)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/其它/Git:Maven/maven常用命令集合（收藏大全）/640.jpg) 作者：ydlmlh 原文：[http://ydlmlh.iteye.com/blog/2158973](http://ydlmlh.iteye.com/blog/2158973)

抽了点时间，整理了一些maven常用命令参数，以便参考；参考了maven官网和网上其他一些maven追随者的文件，不在此一一列举，但表示感谢！

**mvn命令参数**

mvn -v, --version 显示版本信息;

mvn -V, --show-version 显示版本信息后继续执行Maven其他目标;

mvn -h, --help 显示帮助信息;

mvn -e, --errors 控制Maven的日志级别,产生执行错误相关消息; mvn -X, --debug 控制Maven的日志级别,产生执行调试信息; mvn -q, --quiet 控制Maven的日志级别,仅仅显示错误;

mvn -Pxxx 激活 id 为 xxx的profile \(如有多个，用逗号隔开\);

mvn -Dxxx=yyy 指定java全局属性;

mvn -o , --offline 运行offline模式,不联网更新依赖;

mvn -N, --non-recursive 仅在当前项目模块执行命令,不构建子模块;

mvn -pl, --module\_name 在指定模块上执行命令;

mvn -ff, --fail-fast 遇到构建失败就直接退出;

mvn -fn, --fail-never 无论项目结果如何,构建从不失败;

mvn -fae, --fail-at-end 仅影响构建结果,允许不受影响的构建继续;

mvn -C, --strict-checksums 如果校验码不匹配的话,构建失败; mvn -c, --lax-checksums 如果校验码不匹配的话,产生告警;

mvn -U 强制更新snapshot类型的插件或依赖库\(否则maven一天只会更新一次snapshot依赖\);

mvn -npu, --no-plugin-updates 对任何相关的注册插件,不进行最新检查\(使用该选项使Maven表现出稳定行为，该稳定行为基于本地仓库当前可用的所有插件版本\);

mvn -cpu, --check-plugin-updates 对任何相关的注册插件,强制进行最新检查\(即使项目POM里明确规定了Maven插件版本,还是会强制更新\);

mvn -up, --update-plugins \[mvn -cpu\]的同义词;

mvn -B, --batch-mode 在非交互（批处理）模式下运行\(该模式下,当Mven需要输入时,它不会停下来接受用户的输入,而是使用合理的默认值\);

mvn -f, --file  强制使用备用的POM文件; mvn -s, --settings  用户配置文件的备用路径; mvn -gs, --global-settings  全局配置文件的备用路径;

mvn -emp, --encrypt-master-password  加密主安全密码,存储到Maven settings文件里;

mvn -ep, --encrypt-password  加密服务器密码,存储到Maven settings文件里;

mvn -npr, --no-plugin-registry 对插件版本不使用~/.m2/plugin-registry.xml\(插件注册表\)里的配置;

**mvn常用命令**

1. 创建Maven的普通java项目：

   mvn archetype:create

   -DgroupId=packageName

   -DartifactId=projectName

2. 创建Maven的Web项目：

   mvn archetype:create

   -DgroupId=packageName

   -DartifactId=webappName

   -DarchetypeArtifactId=maven-archetype-webapp

3. 编译源代码： mvn compile
4. 编译测试代码：mvn test-compile
5. 运行测试：mvn test
6. 产生site：mvn site
7. 打包：mvn package
8. 在本地Repository中安装jar：mvn install
9. 清除产生的项目：mvn clean
10. 生成eclipse项目：mvn eclipse:eclipse
11. 生成idea项目：mvn idea:idea
12. 组合使用goal命令，如只打包不测试：mvn -Dtest package
13. 编译测试的内容：mvn test-compile
14. 只打jar包: mvn jar:jar
15. 只测试而不编译，也不测试编译：mvn test -skipping compile -skipping test-compile

    \( -skipping 的灵活运用，当然也可以用于其他组合命令\)

16. 清除eclipse的一些系统设置:mvn eclipse:clean

**ps：**

一般使用情况是这样，首先通过cvs或svn下载代码到本机，然后执行mvn eclipse:eclipse生成ecllipse项目文件，然后导入到eclipse就行了；修改代码后执行mvn compile或mvn test检验，也可以下载eclipse的maven插件。

mvn -version/-v 显示版本信息 mvn archetype:generate 创建mvn项目 mvn archetype:create -DgroupId=com.oreilly -DartifactId=my-app 创建mvn项目

mvn package 生成target目录，编译、测试代码，生成测试报告，生成jar/war文件 mvn jetty:run 运行项目于jetty上, mvn compile 编译 mvn test 编译并测试 mvn clean 清空生成的文件 mvn site 生成项目相关信息的网站 mvn -Dwtpversion=1.0 eclipse:eclipse 生成Wtp插件的Web项目 mvn -Dwtpversion=1.0 eclipse:clean 清除Eclipse项目的配置信息\(Web项目\) mvn eclipse:eclipse 将项目转化为Eclipse项目

在应用程序用使用多个存储库

IbiblioIbibliohttp://www.ibiblio.org/maven/PlanetMirrorPlanet Mirrorhttp://public.planetmirror.com/pub/maven/

mvn deploy:deploy-file -DgroupId=com -DartifactId=client -Dversion=0.1.0 -Dpackaging=jar -Dfile=d:\client-0.1.0.jar -DrepositoryId=maven-repository-inner -Durl=ftp://xxxxxxx/opt/maven/repository/

发布第三方Jar到本地库中： mvn install:install-file -DgroupId=com -DartifactId=client -Dversion=0.1.0 -Dpackaging=jar -Dfile=d:\client-0.1.0.jar -DdownloadSources=true -DdownloadJavadocs=true

mvn -e 显示详细错误 信息. mvn validate 验证工程是否正确，所有需要的资源是否可用。 mvn test-compile 编译项目测试代码。 。 mvn integration-test 在集成测试可以运行的环境中处理和发布包。 mvn verify 运行任何检查，验证包是否有效且达到质量标准。 mvn generate-sources 产生应用需要的任何额外的源代码，如xdoclet。

**mvn常用命令2**

mvn -v 显示版本 mvn help:describe -Dplugin=help 使用 help 插件的 describe 目标来输出 Maven Help 插件的信息。 mvn help:describe -Dplugin=help -Dfull 使用Help 插件输出完整的带有参数的目标列

mvn help:describe -Dplugin=compiler -Dmojo=compile -Dfull 获取单个目标的信息,设置 mojo 参数和 plugin 参数。此命令列出了Compiler 插件的compile 目标的所有信息 mvn help:describe -Dplugin=exec -Dfull 列出所有 Maven Exec 插件可用的目标 mvn help:effective-pom 看这个“有效的 \(effective\)”POM，它暴露了 Maven的默认设置

mvn archetype:create -DgroupId=org.sonatype.mavenbook.ch03 -DartifactId=simple -DpackageName=org.sonatype.mavenbook 创建Maven的普通java项目，在命令行使用Maven Archetype 插件 mvn exec:java -Dexec.mainClass=org.sonatype.mavenbook.weather.Main Exec 插件让我们能够在不往 classpath 载入适当的依赖的情况下，运行这个程序 mvn dependency:resolve 打印出已解决依赖的列表 mvn dependency:tree 打印整个依赖树

mvn install -X 想要查看完整的依赖踪迹，包含那些因为冲突或者其它原因而被拒绝引入的构件，打开 Maven 的调试标记运行 mvn install -Dmaven.test.skip=true 给任何目标添加maven.test.skip 属性就能跳过测试 mvn install assembly:assembly 构建装配Maven Assembly 插件是一个用来创建你应用程序特有分发包的插件

mvn jetty:run 调用 Jetty 插件的 Run 目标在 Jetty Servlet 容器中启动 web 应用 mvn compile 编译你的项目 mvn clean install 删除再编译

mvn hibernate3:hbm2ddl 使用 Hibernate3 插件构造数据库

-- 完 --

【推荐阅读】

[MySQL函数及用法示例（收藏大全）](http://mp.weixin.qq.com/s?__biz=MzI4OTA3NDQ0Nw==&amp;mid=2455545114&amp;idx=1&amp;sn=92c0e122125ce23183a8b6f9a810cfc9&amp;chksm=fb9cb97acceb306c4f3135037c2969e4ce27855ec92a85e385f64f7b15f5e2dd62b6a58b053f&amp;scene=21#wechat_redirect)

[Linux 基础命令（收藏大全）](http://mp.weixin.qq.com/s?__biz=MzI4OTA3NDQ0Nw==&amp;mid=2455545100&amp;idx=1&amp;sn=7c14d862ca5b60e0c9d2478962979ee8&amp;chksm=fb9cb96ccceb307aedb8180c41808ba383dcb86afed816ebaf962ede5c5b4140e9b2c9dea4ea&amp;scene=21#wechat_redirect)

[Git常用命令速查表（收藏大全）](http://mp.weixin.qq.com/s?__biz=MzI4OTA3NDQ0Nw==&amp;mid=2455545096&amp;idx=1&amp;sn=48e93e726e86d7d56aea80f83b442ca9&amp;chksm=fb9cb968cceb307e830d6917787d98dd4a5ec159a919d8cb84d48dab2fcf3cc96d244fbb62fb&amp;scene=21#wechat_redirect)

[Vim 命令、操作、快捷键（收藏大全）](http://mp.weixin.qq.com/s?__biz=MzI4OTA3NDQ0Nw==&amp;mid=2455545105&amp;idx=1&amp;sn=a43f6b9c976b2898e92707f961a83974&amp;chksm=fb9cb971cceb30677d380401acaabb18cb22694a4d98100b6ba5776707975801dd2f4059bb4b&amp;scene=21#wechat_redirect)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/其它/Git:Maven/maven常用命令集合（收藏大全）/640.png)

[http://mp.weixin.qq.com/s?\_\_biz=MzI4OTA3NDQ0Nw==&mid=2455545120&idx=1&sn=a7c3f1244c93917a15b68234d4a08b1b&chksm=fb9cb940cceb3056e3d6f99edec29c3265595789ed67c7f748cf8e3e4676ff50ba40b23ad08b&mpshare=1&scene=1&srcid=08128ca6xc5Vf7VVAId2TOSf\#rd](http://mp.weixin.qq.com/s?__biz=MzI4OTA3NDQ0Nw==&mid=2455545120&idx=1&sn=a7c3f1244c93917a15b68234d4a08b1b&chksm=fb9cb940cceb3056e3d6f99edec29c3265595789ed67c7f748cf8e3e4676ff50ba40b23ad08b&mpshare=1&scene=1&srcid=08128ca6xc5Vf7VVAId2TOSf#rd)


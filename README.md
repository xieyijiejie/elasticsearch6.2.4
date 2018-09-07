# elasticsearch6.2.4
修改elasticsearch6.2.4源码
准备条件，每个版本的代码都有对应的版本的jdk和gradle，可以在文档里面找到6.2.4的版本需要jdk9以上，由于jdk9被归并到jdk10里面了，所以我直接安装了jdk10,在一台win10机器上可以安装多个版本的jdk10
IDEA8才能把jdk10配置进去，这一点要注意,gradle4.5,  从官方网站下载解压后 ，在环境变量里面配置一个home和path即可

下载elasticsearch源码：
最新版本源码：https://github.com/elastic/elasticsearch
往期版本的源码：https://github.com/elastic/elasticsearch/releases
我下载的是往期的6.2.4,每个版本的差距比较大
解压后，在我们下载的 Elasticsearch 根目录下执行命令：(执行已经写好的脚本 gradlew)    
./gradlew idea

导入 IDEA
idea 中 File -> New Project From Existing Sources 选择你下载的 Elasticsearch 根目录，然后点 open ，之后 Import project from external model -> Gradle , 选中 Use auto-import, 然后就可以了。

在elasticsearch源码的根目录下，执行下面的命令
./gradlew run --debug-jvm

在idea中创建一个远程调节
图片: https://images-cdn.shimo.im/KZfAMhTUnGY2QMzU/image.png

为了让调试时候不适用127.0.0.1或者localhost的host，需要在源码ClusterFormationTasks
里面修改调试自动生成的node的config参数，找到configureWriteConfigTask方法，在里面添加下面几行代码：
esConfig['network.host'] = '192.168.13.10'  //修改network.host为局域网ip
esConfig['http.cors.enabled'] = true   //下面两行是为了在es-head里面可以访问192.168.13.10:9200,使用可视化界面，如果没有这两行，则es-headl连不上集群
esConfig['http.cors.allow-origin'] = '"*"'

然后debug上面的es-debug application，就会得到下面的结果，说明集群启动成功了
图片: https://images-cdn.shimo.im/vKmEK18cidULa5ot/image.png

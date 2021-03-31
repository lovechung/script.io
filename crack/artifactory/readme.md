- 使用镜像：registry.cn-hangzhou.aliyuncs.com/lovechung/artifactory-pro
- 拷贝破解包（artifactory-injector-1.1.jar）至容器根目录
- 进入容器：`docker exec -it artifactory bash`
- 执行命令：`/opt/jfrog/artifactory/app/third-party/java/bin/java -jar artifactory-injector-1.1.jar`
  
  得到以下输出：
  ```shell
    What do you want to do?
    1 - generate License String
    2 - inject artifactory
    exit - exit
  ```
  
  先选择2，输入artifactory目录：`/opt/jfrog/artifactory/app/artifactory/tomcat/` （注意：必须是 webapps/artifactory.war 所在的目录）
  再选择1，生成license code，退出
- 拷贝license code

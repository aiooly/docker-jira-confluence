# setup jira and confluence use docker-compose

确保docker和docker-compose已经安装

## 启动所有容器

```
docker-compose up -d
```

## 配置jira

### 创建jira_db数据库

```
create database jira_db character set utf8 collate utf8_bin;
```

### 打开页面进行向导配置


## 配置confluence

### 创建confluence数据库

```
create database confluence character set utf8 collate utf8_bin;
```

### 破解confluence
记录Service ID
#### 复制并备份文件
```
docker cp confluence:/opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.2.jar ./atlassian-extras-2.4.jar
```
#### 进入容器 并备份文件
```
 docker exec -u root -it confluence /bin/sh
 mv /opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.2.jar  /opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.2.jar.bak
```
#### 使用破解工具生成新的jar文件

输入Service ID，选择复制出来的jar文件，点击patch，点击gen，记录生成key重启要用
```
#linux需要GUI,也可以在windows使用keygen.bat
rackfile/iNViSiBLE/keygen.sh
#将一开始记录的Service ID 输入，name可以随意填写
#点击patch选择刚改名的文件点击.gen！生成key，则atlassian-extras-2.4.jar已经破解完成
```
#### 将atlassian-extras-2.4.jar改回atlassian-extras-decoder-v2-3.2.jar并复制回原先目录
```
docker cp atlassian-extras-decoder-v2-3.2.jar  confluence:/opt/atlassian/confluence/confluence/WEB-INF/lib/
```

#### 重启confluence容器
```
docker restart confluence
```

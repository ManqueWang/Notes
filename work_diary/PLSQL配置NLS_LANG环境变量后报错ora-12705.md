# 问题描述
1. 已经安装oracle客户端+plsql，且可以成功远程连接数据库。但出现乱码问题。
2. 用 ``` select userenv('language') from dual ``` 语句查询服务端的语言，新增环境变量NLS_LANG后，重启plsql，发现不能连接远程数据库了，报错信息为**ora-12705**
# 问题排查
综合网上查到的信息，ora-12705错误是由于NLS_LANG变量未正确配置导致的。因此按照如下步骤排查

1. 排查是否由于之前安装过oracleClient，导致注册表中有多个NLS_LANG配置，且其值不正确。因此在注册表中查找是否存在其他NLS_LANG配置。（打开注册表方式 win+R->regedit）
    
    *结果发现，并没有*
2. 排查是否是其他配置变量未正确配置导致的。
    
    *通过安装教程得知，我们需要配置的环境变量有 NLS_lANG、TNS_ADMIN、Path。有些教程中提到了ORACLE_HOME，但是之前的安装中并没有配置过该环境变量也没有问题，为了保险起见找了一个可以正常使用的环境在注册表中搜索了一下ORACLE_HOME果然在HKEY_LOCAL_MACHINE->SOFTWARE->Wow6432Node->ORACLE下找到了对应的oracleClient节点，并且该节点下有ORACLE_HOME配置。再去有问题的环境下找到响应的OracleClient节点，但是节点下并没有ORACLE_HOME配置，那么问题应该就是在ORACLE_HOME上*

# 解决方式

在问题环境上手动添加ORACLE_HOME环境变量，其值为OracleClient的根目录，我这边环境如下
ORACLE_HOME=D:\oracle\product\10.2.0\client_1(仅做参考)

重启plsql可以成功连接远端数据库且不乱码。
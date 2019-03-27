# 注意事项
- dra-init.sql: 系统mysql数据库初始化脚本
- nginx ：该目录为nginx自定义配置文件，覆盖容器默认配置
- my.cnf : 主要设置mysql编码问题，防止中文乱码
- 若修改了相关文件的名字，请检查docker-compose.yml内容
- 注意此处还需要放置应用打包后的jar（springboot），同样注意名字修改
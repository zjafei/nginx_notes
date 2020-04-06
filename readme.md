1. nginx 的配置文件是 nginx.conf
2. nginx.conf 配置的基本结构是
```config
## http的配置
http {
    ## 域名 ip的配置
    server {
        ## uri的配置
        location {
        }
    }
}
``` 
```config
## 除去 http 外的基本配置
server {
        ##  监听端口
        listen 8888;
        
        ## 服务名称域名或者 ip
        server_name _;
        
        ## 访问日志 键名 日志路径 日志格式
        access_log /mnt/d/log/8888.log main

        ## uri的规则
        location / {
                ## 文件夹目录
                root /mnt/d;
                ## 入口文件
                index index.html;
        }

}
```

```bash
## 日志备份脚本
BASE_DIR=/usr/local/nginx
BASE_FILE_NAME=bhz.com.access.log

CURRENT_PATH=$BASE_DIR/logs
BANK_PATH=$BASE_DIR/data_logs

CURRENT_FILE=$BANK_PATH/$BASE_FILE_NAME
BANK_TIME=`/bin/date -d yesterday + %Y_%m_%d_%H:%M`
BANK_FILE=$BANK_PATH/$BANK_TIME-$BASE_FILE_NAME

echo $BANK_FILE

$BASE_DIR/sbin/nginx -s stop
mv $CURRENT_FILE $BANK_FILE
$BASE_DIR/sbin/nginx
 
## 计划任务 crontab -e
```
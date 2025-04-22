# https  websocket协议

var socket = new WebSocket("wss://www.aabb.cn/socket/");
注意点：

如果网站使用HTTPS，WebSocket必须要使用wss协议；
使用wss协议的连接请求必须只能写域名，而非IP+端口；
建议在URL域名后面为websocket定义一个路径，本例中是/socket/；
 nginx配置

只需要在HTTPS配置的server内加一个location即可

# websockets

location /socket/ {
    proxy_pass http://127.0.0.1:3000;<br />    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Real-IP $remote_addr;
}
注意：

　　1、location /socket/ {...}这里要格外注意！

　　　　html中的url是 wss://www.aabb.com/socket/，所以Nginx配置中一定要是 /socket/

　　　　如果前端是 wss://www.aabb.com/socket，Nginx对应是 /socket

　　2、proxy_pass对应的最好是公网IP加端口号， 'localhost'，'127.0.0.1'

　　3、proxy_http_version 1.1 版本号必须是1.1，这条配置必需

说明：

　　Nginx反向代理，无论是HTTP/S或是WebSocket都会走443端口，由Nginx分发给各个项目服务器。
————————————————
版权声明：本文为CSDN博主「梦夏夜」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/jmkweb/article/details/97531336

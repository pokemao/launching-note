# 服务器

---

## 购买服务器

> 1. 随意购买阿里云和腾讯云的服务器
> 2. 使用官方配置(镜像等等)
> 3. CentOS系统

## 配置防火墙

> * 保持默认开放的端口不变
> * 如果可以的话直接开放100~1000的端口, 这样比较方便
> * 开放888端口为后续宝塔面板的php数据库做准备
> * 开放8888端口为宝塔面板开放
> * 开放自己服务器设置的端口号

# 域名

---

## 购买域名

> * 随意购买自己心仪的域名
> * 最好是.com结尾的

## 域名解析

> * "快速解析" 就好

## 域名备案

> * 按照流行进行即可
> * 备案完成就可以通过宝塔面板绑定到服务器的IP地址进行访问了

## SSL证书

> * 用于将HTTP升级为HTTPS

# 宝塔面板

---

## 注意重点

> 如果不想把自己的电脑变成一台服务器的话就不要下载windows版的宝塔面板
>
> 而是在购买的服务器的位置执行linux命令

## 服务器配置宝塔面板的方式

> 在服务器控制台页面中点击远程登录中的快捷登录
>
> 在"黑窗口中"输入对应的宝塔面板语句
>
> ```shell
> sudo su root
> yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
> ```
>
> 三条命令执行完毕后保存 宝塔面板url、username、password

# Vue.js+Node.js(koa)+MySQL利用宝塔面板上线实例

---

## 起始配置

> 1. ==重置服务器== —— 删除服务器上所有的内容
> 2. 在服务器配置宝塔面板
> 3. 利用配置好的宝塔面板的url, username和password登录宝塔面板页面
> 4. 下载宝塔面板推荐的 ==LNMP环境==
> 5. 进入"==安全=="页面, 放行端口==1000~10000==, 备注写"项目"

##绑定域名同时创建项目

> * 下载LNMP完毕后进入"==网站==页面," 点击"==添加站点==", 输入域名, 点击提交, ==不创建ftp和数据库==

## 前端

> 1. 打开Vue项目, 找到==创建axios实例==的文件(我常常在==src/utils/service.js==文件中创建实例)
> 2. 将"==baseURL=="改写成 ==http://服务器IP地址:后端开放的端口号/api/==
> 3. 删除项目中的==vue.config.js文件==(如果没有不用删除)
> 4. 在控制台中使用==npm run build==命令生成dist文件夹
> 5. 进入宝塔面板的"==文件=="页面, 在地址栏中输入==/www/wwwroot/自己购买的域名==, 进入到相应的文件夹中
> 6. ==删除==文件夹中原有的==index.html文件==
> 7. 将刚刚Vue项目生成的==dist文件夹中的全部内容==[^注意]导入到宝塔面板的文件夹中
> 8. 导入之后就可以通过域名访问到你的前端网页了
>
> [^注意]: 这里要导入的是dist文件夹里面的全部内容不是dist文件夹, 里面的全部内容指的是index.html, css文件夹, js文件夹, img文件夹, ...

## 数据库

> 1. 进入"==软件商店=="界面, 观察MySQL版本, 检查本次==项目==使用的MySQL版本和==宝塔面板==中安装的MySQL的版本==是否一致==
>
>    > * 一致
>    >
>    >   > 不做任何操作
>    >
>    > * 不一致
>    >
>    >   > 删除宝塔面板中的MySQL, 下载与项目中MySQL版本一致的MySQL
>
> 2. 进入本机的==Navicat==, 选择本次项目使用的数据库, 右键导出"==结构和数据=="
>
> 3. 进入宝塔面板的"==数据库=="页面, 点击"==root密码==", 将root密码改为root, 点击"==添加数据库==", 输入"数据库名", "用户名", "密码", 要求 ==数据库名\=\==用户名\=\==项目导出的数据库的名字==, 输入"密码"(随意自定义)
>
> 4. 创建完毕之后点击"==导入==", 将刚刚从Navicat中导出的.sql文件导入到宝塔面板中
>
> 5. 打开自己的后端项目, 在连接数据库的位置, 更改配置信息和刚才在宝塔面板中创建的数据库信息一致
>
>    > ```env
>    > host: localhost
>    > port: 3306
>    > database: 刚刚创建的数据库名
>    > user: 刚刚创建的用户名
>    > password: 刚刚创建的密码
>    > ```

## 后端

> 1. 进入宝塔面板的"==软件商店=="页面, 下载==PM2管理器==
>
> 2. 进入宝塔面板的"==文件=="页面, 在地址栏中输入==/www/wwwroot/自己购买的域名==, 进入到相应的文件夹中
>
> 3. 将本地后端代码的文件夹整个上传到刚刚宝塔面板中的那个文件夹中
>
> 4. 下载PM2管理器完毕后, 点击执行PM2管理器, 点击"==添加项目=="
>
>    > * "==启动文件=="选择刚刚导入到宝塔面板的后端文件夹中的项目主入口文件(main.js或者app.js)
>    > * "==运行目录=="选择在VS Code的终端中运行后端项目时对应的目录[^提醒]
>    > * "项目名称"随便填一个心仪的名字
>    > * 其他数据不更改, 点击"==提交=="
>
> 5. 点击刚刚创建的项目的"==管理=="选项, 点击右上角"==一键安装依赖=="
>
> 6. 安装完成后, 点击"==启动=="启动项目
>
> 7. 点击"==端口==", 输入本项目中后端设置的==端口号==
>
> 8. 这时就可以通过访问 http://服务器IP地址:前端的端口号/请求路由, 发送get请求获得数据了, 但是还没有和前端完成连接
>
> [^提醒]: 比如说平时在VS Code中是将地址cd到backend文件夹中使用npm start启动项目的, 那么这时填写的运行目录也是到backend为止; 如果是cd到/backend再cd到src文件中使用npm start启动项目的那么就要将运行目录填写到backend/src为止

## 代理

> 1. 进入宝塔面板的"==软件商店=="页面, 点击打开Nginx
>
> 2. 进入Nginx的"==修改配置=="页面
>
> 3. 找到"==server==", 配置server
>
>    > * 修改最上方的几条内容
>    >
>    >   > ```nginx
>    >   > listen 80;
>    >   > server_name 服务器的IP地址(例如: 82.157.68.204);
>    >   > root /www/wwwroot/自己购买的域名
>    >   > ```
>    >
>    > * 添加一条内容
>    >
>    >   > ```nginx
>    >   > location /api/ {
>    >   >           proxy_read_timeout 60s;
>    >   >           proxy_http_version 1.1;
>    >   >           proxy_set_header Upgrade $http_upgrade;
>    >   >           proxy_set_header Connection 'Upgrade';
>    >   >           
>    >   >           proxy_set_header    Host    $http_host;
>    >   >           proxy_set_header    X-Real-IP   $remote_addr;
>    >   >           proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
>    >   >           proxy_set_header X-Forwarded-For $remote_addr;
>    >   >           add_header Access-Control-Allow-Credentials true;
>    >   >           #add_header Access-Control-Allow-Headers 'version, access-token, user-token, Accept, apiAuth, User-Agent, Keep-Alive, Origin, No-Cache, X-Requested-With, If-Modified-Since, Pragma, Last-Modified, Cache-Control, Expires, Content-Type, X-E4M-With, DNT, X-Mx-ReqToken, Authorization'; 
>    >   >           add_header Access-Control-Allow-Headers '*'; 
>    >   >           if ($request_method = 'OPTIONS') 
>    >   >           {
>    >   >             return 204;
>    >   >           }
>    >   >           add_header Cache-Control no-cache;
>    >   >           add_header Access-Control-Allow-Methods 'GET, POST, PUT, DELETE, PATCH, OPTIONS';
>    >   >           add_header Access-Control-Allow-Origin '*';
>    >   >           proxy_pass http://localhost:本项目后台开启的端口号/;
>    >   >         }
>    >   > ```
>    >   >
>    >   > * ==这条内容中的proxy_pass要根据自己开放的服务器端口进行修改==
>    >   >
>    >   > * 其余部分根据项目要求进行修改

#IP地址为82.157.68.204的服务器

---

## 对应的宝塔面板信息

> ```shell
> ==================================================================
> Congratulations! Installed successfully!
> ==================================================================
> 外网面板地址: http://82.157.68.204:8888/213172a3
> 内网面板地址: http://10.0.24.8:8888/213172a3
> username: dgi5ldsr
> password: f96ae899
> If you cannot access the panel,
> release the following panel port [8888] in the security group
> 若无法访问面板，请检查防火墙/安全组是否有放行面板[8888]端口
> ==================================================================
> ```

## 绑定的域名

> pokemao.plus

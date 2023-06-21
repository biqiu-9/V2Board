# V2Board

使用aaPanel手动部件档

aaPanel是宝塔( http://bt.cn/ )的国际版

1.配置aaPanel

您需要在https://forum.aapanel.com/d/9-aapanel-linux-panel-6-5-4-installation-tutorial中选择您的系统获得安装方式。这里以 CentOS 7+ 作为系统环境进行安装。

请务必使用CentOS 7+安装aaPanel，其他系统可能会有未知问题。

最新脚本可以在aaPanel官网获取 yum install -y wget && wget -O install.sh http://www.aapanel.com/script/install_6.0_en.sh && bash install.sh aapanel 

安装完成后我们登录aaPanel进行环境的安装。

选择使用LNMP的环境安装方式勾选如下信息

☑️ Nginx 1.17

☑️ MySQL 5.6

☑️ PHP 7.4

选择 Fast 快速编译后进行安装。

2.安装Redis、fileinfo

aaPanel 面板 > App Store > 找到 PHP 7.4 点击设置 > 安装扩展 > redis,fileinfo 进行安装。

3.解除被禁止的函数

aaPanel 面板 > App Store > 找到 PHP 7.4 点击设置 > 禁用功能putenv proc_open pcntl_alarm pcntl_signal将从列表中删除。

4.添加站点

aaPanel 面板 > 网站 > 添加网站。

在域中填入你指向服务器的域名称

在数据库中选择MySQL

在PHP版本中选择PHP-74

5.安装V2Board

通过SSH登录到服务器后访问站点路径如下：/www/wwwroot/你的站点域名称。

以下命令都需要在站点目录进行操作。

删除目录下文件chattr -i .user.inirm -rf .htaccess 404.html index.html .user.ini

执行命令从Github克隆到当前目录。

git clone https://github.com/v2board/v2board.git ./

执行命令安装依赖包以及V2board

sh init.sh

根据提示完成安装

6.配置站点目录及伪静态

添加完成后编辑添加的站点>站点目录>运行目录选择/public保存。

添加完成后编辑添加的站点 > URL rewrite 填写伪静态信息。

location /downloads {}location / {      try_files $uri $uri/ /index.php$is_args$query_string;  }location ~ .*\.(js|css)?${    expires      1h;    error_log off;    access_log /dev/null; }

7.配置定时任务

aaPanel 面板 > Cron。

在任务类型中选择 Shell 脚本

在任务名称中填写 v2board

在周期中选择 N 分钟 1 分钟

在脚本内容中填写 php /www/wwwroot/路径/artisan schedule:run

根据上面描述的信息添加每1分钟执行一次的定时任务。

8.启动团队服务

V2board的系统强大依靠团队服务，正常使用V2Board必须启动团队服务。下面以aaPanel中supervisor服务来守护团队服务为表演示。

aaPanel 面板 > App Store > 工具

找到主管进行安装，安装完成后点击设置 > Add Daemon按照如下填写

在名称填写V2board

在运行用户选择www

在运行目录选择站点目录在启动命令填写php artisan horizo  n在进程填写1

填写后点击确认添加即可运行。

常见问题

Q：500错误

A：检查站点根目录权限，递归755，保证目录有可写文件的权限，也有可能是Redis扩展没有安装或Redis没有照片创建的。你可以通过查看storage/logs下面的日志来排查错误或开启调试模式、站点设置中关防护跨站。


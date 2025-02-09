# 东方木经过亲自测试以后的笔记
## 安装

SSPanel UIM 的需要以下程序才能正常的安装和运行：

- Git
- MySQL
- PHP 7.2+
- Composer

SSPanel UIM 支持安装在 LNMP、宝塔面板、Plesk 面板、oneinstack 等集成环境中。安装教程请参阅 [文档](https://wiki.sspanel.host)。

## 演示安装
### 购买服务器
略过系统centos7
### ssh连接
root用户登陆
### 安装宝塔面板
宝塔面板安装

```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```
### 登陆面板选择安装需要的东西
登陆选择如下
选择左边
mysql5.5
php7.2
phpmyadmin4.7
其他默认
安装了
### 网站
网站新建一个
域名写解析的或者ip都行
ftp创建
mysql 选择长的那个，四个都一样。
### 配置PHP展开
在软件商店里找到 PHP， 点击设置。
在禁用函数一栏删除 system proc_open proc_get_status putenv。其实这里找不全，后面还要删。
### 安装网站程序
下载压缩包上传解压到根目录
权限755
找到config目录下的.config.php.example文件，将它重命名为.config.php并点开“编辑”。修改一下数据库的信息，其他的根据自己需要修改，修改完成后保存。
只有四项必须改的是前后端对接密码（mukey），数据库名；数据库用户名；与对应的密码。
以上也可以用以下代码执行。ps我不太喜欢用代码，因为一开始看不懂，后来发现都一样
```
cd /www/wwwroot/你的文件夹名
git clone -b master https://github.com/Anankke/SSPanel-Uim.git tmp && mv tmp/.git . && rm -rf tmp && git reset --hard
git config core.filemode false
cd ../
chmod -R 755 你的文件夹名/
chown -R www:www 你的文件夹名/
```
然后找到 网站 - 设置 - 网站目录，将网站运行目录改为public，取消勾选“防跨站攻击”
### 依赖的安装
在网站目录下（cd /www/wwwroot/你的文件夹名字）输入如下命令：
```
wget https://getcomposer.org/installer -O composer.phar
php composer.phar
php composer.phar install
```
### 复制数据库到备份
```
ln -s /www/wwwroot/你的文件夹名/sql/glzjin_all.sql /www/backup/database/
```
### 数据库的导入
找到数据库，点导入，点从本地上传，上传我们解压的sspanel前端的文件夹内的sql文件夹中的glzjin_all.sql
覆盖就行
### 伪静态
在 伪静态 中填入下面内容：
```
location / {
    try_files $uri /index.php$is_args$args;
}
```
### 不知道叫啥的一步
然后找到软件商店 - PHP7.1 - 设置。切换到 配置文件，搜索 display_errors = 并把On改为Off后保存。大概在470多行。
在 性能调整 中，将 运行模式 改为 静态 30并发。并保存。

### 这个时候可能会有依赖安装不了，反正我的一定有。
这个时候应该这样子
php设置，把禁用函数里的putenv删除
ssh连接服务器
依次执行以下命令
```
free -m
mkdir -p /var/_swap_
cd /var/_swap_
#Here, 1M * 2000 ~= 2GB of swap memory
dd if=/dev/zero of=swapfile bs=1M count=2000
mkswap swapfile
swapon swapfile
echo “/var/_swap_/swapfile none swap sw 0 0” >> /etc/fstab
#cat /proc/meminfo
free -m
```
 
然后cd到网站目录
执行
```
php composer.phar install
```
出现一堆绿的黄的，等待执行好
这个时候网站已经可以打开了。
然后创建管理员
### 创建管理用户
网站目录执行
```
php xcat createAdmin
php xcat syncusers
php xcat initQQWry
php xcat resetTraffic
php xcat initdownload
```
OK了
这个时候打开时是俩套模版
改为旧的
这里我们需要修改一个文件，那就是网站config目录下的routes.php，将 $app->get('/', 'App\Controllers\HomeController:index'); 改为 $app->get('/', 'App\Controllers\HomeController:indexold');即可
基本就是这样。

## 文档

> 我们安装，我们更新，我们开发

[SSPanel UIM 的文档](https://wiki.sspanel.host)，在这里你可以找到大部分问题的解答。

## 贡献

[提出新想法 & 提交 Bug](https://github.com/Anankke/SSPanel-Uim/issues/new) | [改善文档 & 投稿](https://github.com/sspanel-uim/Wiki) | [Fork & Pull Request](https://github.com/Anankke/SSPanel-Uim/fork)

SSPanel UIM 欢迎各种贡献，包括但不限于改进，新功能，文档和代码改进，问题和错误报告。

## 协议

SSPanel UIM 使用 MIT License 开源、不提供任何担保。使用 SSPanel UIM 即表明，您知情并同意：

- 您在使用 SSPanel UIM 时，必须保留 Staff 页面（该页面包含了 MIT License）和页脚的 Staff 入口
- SSPanel UIM 不会对您的任何损失负责，包括但不限于服务中断、Kernel Panic、机器无法开机或正常使用、数据丢失或硬件损坏、原子弹爆炸、第三次世界大战、SCP 基金会无法阻止 SCP-3125 引发的全球 MK 级现实重构等


## 鸣谢

### [HKServerSolution](https://www.hkserversolution.com/cart.php)

Demo 演示站服务器赞助。

### [贡献者](https://github.com/Anankke/SSPanel-Uim/graphs/contributors)

SSPanel UIM 离不开所有 [贡献代码](https://github.com/Anankke/SSPanel-Uim/graphs/contributors) 和提交 Issue 的人。

<details>
<summary>查看贡献者</summary>

#### [Anankke](https://github.com/Anankke)

- 面板现 **维护者**

#### [galaxychuck](https://github.com/galaxychuck)

- 面板 **原作者**

##### [dumplin](https://github.com/dumplin233)

- 码支付对接 + 码支付当面付二合一
- 为面板加入 AFF 链接功能
- 商品增加限速和限制 ip 属性
- 多端口订阅
- 解决用户列表加载缓慢历史遗留问题

##### [RinSAMA](https://github.com/mxihan)

- 整理分类 config.php
- 美观性调整


# Linux基础及网新运维

### 新建用户及简单地处理文件（第二周实操笔记）

![1-1](https://images.gitee.com/uploads/images/2019/0316/204732_b4468699_1922224.png "1-1.png")

1.新建用户为171013034hyh，设置密码为Aa111111

![1-2](https://images.gitee.com/uploads/images/2019/0316/204748_499a3d36_1922224.png "1-2.png")


2.新建目录和文件

![1-3](https://images.gitee.com/uploads/images/2019/0316/204801_ea2924e5_1922224.png "1-3.png")


3.拷贝、更名与删除文件

![1-4](https://images.gitee.com/uploads/images/2019/0316/204814_9c96e7b2_1922224.png "1-4.png")

### FTP服务器配置

1.通过YUM安装VSFTP<br>
yum install vsftpd -y

![2-1](https://images.gitee.com/uploads/images/2019/0316/204834_ebe16369_1922224.png "2-1.png")


2.启动FTP服务<br>
service vsftpd start

![2-2](https://images.gitee.com/uploads/images/2019/0316/204849_956b3f6c_1922224.png "2-2.png")


3.查看端口状态<br>
netstat -anp|grep 21

![2-3](https://images.gitee.com/uploads/images/2019/0316/204903_5a85333f_1922224.png "2-3.png")

此时，在cmd用匿名用户登录



会登陆成功！


4.修改FTP服务配置文件（/etc/vsftpd/vsftpd.conf）不允许匿名用户登录<br>
使用vim编辑器

![2-5](https://images.gitee.com/uploads/images/2019/0316/202722_a4c9dcd9_1922224.png "屏幕截图.png")

`#禁用匿名用户

anonymous_enable=NO

#设置所有的本地用户都不能切换到主目录以外的目录

chroot_local_user=YES`

![2-6](https://images.gitee.com/uploads/images/2019/0316/203016_d2f35ca9_1922224.png "屏幕截图.png")

5.重启FTP服务<br>
service vsftpd restart

此时，验证匿名用户能否登录

![2-7](https://images.gitee.com/uploads/images/2019/0316/203056_1a032165_1922224.png "屏幕截图.png")

结果 失败！

6．创建FTP用户<br>
useradd 171013034hyh<br>
passwd 171013034hyh<br>

![2-8](https://images.gitee.com/uploads/images/2019/0316/203151_df12ab82_1922224.png "屏幕截图.png")

此时，如果用刚创建的FTP用户在cmd登录，会出现如下报错。

![2-9](https://images.gitee.com/uploads/images/2019/0316/203213_d0ac6b1b_1922224.png "屏幕截图.png")

因为我们限定了用户不能跳出其主目录。所以我把第4步在vim编辑的chroot_local_user=YES 先删掉。<br>
重启FTP服务 service vsftpd restart<br>
然后就可以了。

![2-10](https://images.gitee.com/uploads/images/2019/0316/203252_66bb4a50_1922224.png "屏幕截图.png")

7.限制用户只能通过FTP访问服务器，而不能直接登录服务器<br>
usermod -s /sbin/nologin 171013034hyh

![2-11](https://images.gitee.com/uploads/images/2019/0316/203321_fae622e5_1922224.png "屏幕截图.png")

![2-12](https://images.gitee.com/uploads/images/2019/0316/203337_e24696f8_1922224.png "屏幕截图.png")

8.为用户分配主目录
“使用171013034hyh用户登录FTP服务器,为用户 171013034hyh  创建主目录<br>
并约定：/data/ftp 为主目录, 该目录不可上传文件<br>
/data/ftp/pub 文件只能上传到该目录下<br>
mkdir -p /data/ftp/pub<br>
创建登录欢迎文件 ：<br>
echo "Welcome to use FTP service." > /data/ftp/welcome.txt”

先在PuTTy   设置访问权限：<br>
chmod a-w /data/ftp && chmod 777 -R /data/ftp/pub<br>
设置为用户的主目录：<br>
usermod -d /data/ftp 171013034hyh


![2-13](https://images.gitee.com/uploads/images/2019/0316/203501_aa5fa967_1922224.png "屏幕截图.png")

然后，在cmd新建：<br>
mkdir 171013034hyhtry

![2-14](https://images.gitee.com/uploads/images/2019/0316/203544_2fc9fb9b_1922224.png "屏幕截图.png")

查看：

![2-15](https://images.gitee.com/uploads/images/2019/0316/203605_e57bc0cf_1922224.png "屏幕截图.png")

![2-16](https://images.gitee.com/uploads/images/2019/0316/203921_1e30ca7f_1922224.png "1111.png")

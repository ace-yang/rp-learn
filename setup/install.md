## 常用操作

### 1. 设置proxy

为系统增加proxy:

```shell
# using below command
pi@raspberry:~/Downloads $ sudo vi /etc/environment
# add below text
export http_proxy="http://usrName:password@Proxy.my.com:10108"
export https_proxy="http://usrName:password@Proxy.my.com:10108"
export no_proxy="localhost, 127.0.0.1, 10.*.*.*"
```

在使用cyrl时使用proxy

```shell
# sample，-k 参数是忽略https的证书
curl -x usrname:password@Proxy.my.com:10938 -fksSL https://get.docker.com -o get-docker.sh
```

### 2. 安装Docker

```shell
# 以下方法最方便，但无法在Herp-V的虚拟上运行，原因是虚拟机是i386架构，而raspberry的docker package
# 是ARM32的架构
# refer to: https://yeasy.gitbook.io/docker_practice/install/raspberry-pi
# refer to: https://phoenixnap.com/kb/docker-on-raspberry-pi
# 更新并升级系统
sudo apt-get update && sudo apt-get upgrade
# 下载安装脚本（如果是通过proxy下载，可以使用：-x usrname:password@HKPriProxy.aia.biz:10938）
curl -fsSL https://get.docker.com -o get-docker.sh
# 执行安装脚本，该脚本支持使用国内的阿里云(Aliyun)和微软云(AzureChinaCloud)的镜像站点
sudo sh get-docker.sh --mirror Aliyun

# 将docker设置为自启动服务并启动
sudo systemctl enable docker
sudo systemctl start docker

# 将当前用户加入到新增的docker group，使得当前用户可以直接使用docker(无需sudo)
sudo groupadd docker
sudo usermod -aG docker $USER


docker run --name myweb -d -p 80:80  -v /home/pi/www/conf/nginx.conf:/etc/nginx/nginx.conf  -v /home/pi/www/logs:/var/log/nginx  -v /home/pi/www/html:/usr/share/nginx/html nginx

```

### 3 安装汉字输入

```shell
sudo apt-get install fonts-wqy-zenhei
sudo apt-get install fcitx fcitx-googlepinyin fcitx-module-cloudpinyin fcitx-sunpinyin
# 输入reboot重启(后面步骤不能忽略）
# 鼠标放在右上角的键盘并右击，选择Configure Curren Input Method,点击左下角加号，取消Only Show Current 
# Language的√，输入Googlepinyin并搜索，点击确定添加进，接下来你就可以愉快地在树莓派用中文输入法了
```



## 参考文献

1. https://shumeipai.nxez.com/ 中文网站，大量的树莓派的实战案例
2. https://www.raspberrypi.org/ 官方网站
3. https://shumeipai.nxez.com/2019/01/02/hc-sr04-ultrasonic-ranging-module-on-raspberry-pi.html 超声波测距仪案例
4. https://shumeipai.nxez.com/2016/05/14/equipped-with-electronic-paper-screens-pumping-tray.html 电子屏幕案例
5. https://shumeipai.nxez.com/2019/08/27/add-the-open-and-shutdown-keys-to-the-raspberry-pi.html 开关机案例

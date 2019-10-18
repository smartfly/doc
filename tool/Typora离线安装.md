# Typora离线安装

Typora是一个兼容Win/mac/ubuntu系统markdown编辑器，它提供了一种所见即所得的全新的Markdown写作体验。

Typora有两种安装方式，一种是线上命令安装(官方推荐)，一种是离线包安装。

## [命令安装步骤](http://support.typora.io/Typora-on-Linux/)

```shell
# or use
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -

# add Typora's repository
sudo add-apt-repository 'deb https://typora.io/linux ./'
sudo apt-get update

# install typora
sudo apt-get install typora
```

## 离线安装步骤

1. 下载离线包:

   下载```Typora-linux-x64.tar.gz```链接[Typora-linux-x64.tar.gz](https://typora.io/#linux)

2. 解压安装:

   ```shell
   sudo tar -xvf Typora-linux-x64.tar.gz  -C /opt/
   ```

3. 创建ubuntu快捷键:

   ```shell
   sudo gedit /usr/share/applications/Typora.desktop
   ```

   编辑Typora.desktop文件

   ```shell
   [Desktop Entry]
   Type=Application
   Encoding=UTF-8
   Name=Typora
   Comment=markdown editor
   Exec=/opt/soft/Typora-linux-x64/Typora
   Icon=/opt/soft/Typora-linux-x64/resources/app/asserts/icon/icon_256x256.png
   Terminal=false
   ```

   字段解释：

   - Type: desktop类型, 此处应该填写Application
   - Encoding: 文件编码方式，一般使用UTF-8
   - Name: 应用文件名，本例中此处填写Typora
   - Comment: 应用描述，可以随意编写
   - Exec: 应用执行路径，必须准确填写
   - Icon: 图标路径
   - Terminal: 是否需要打开命令行模式，一般写false


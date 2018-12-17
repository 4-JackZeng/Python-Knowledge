#### `解决linux下面桌面无软件启动快捷方式问题`

 在终端输入以下命令行：

sudo gedit /usr/share/applications/Pycharm.desktop

进入gedit文档界面

然后将里面的内容复制成：

[Desktop Entry]
Type=Application
Name=Pycharm
GenericName=Pycharm3
Comment=Pycharm3:The Python IDE
Exec=sh /home/sundy/pycharm-community-2018.2.4/bin/pycharm.sh
Icon=/home/sundy/pycharm-community-2018.2.4/bin/pycharm.png
Terminal=pycharm
Categories=Pycharm;

此两处需要替换为自己的安装目录

Exec=sh /home/sundy/pycharm-community-2018.2.4/bin/pycharm.sh
Icon=/home/sundy/pycharm-community-2018.2.4/bin/pycharm.png

最后，在启动器上就可以看到图标了，然后点右键发送至桌面就行。

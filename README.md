# service.multimedia.plexmediaserver
Plex Media Server addon for OpenELEC ON N1
斐讯N1 刷了OpenELEC 后
再安装这个插件实现Plex Media Server功能
下载zip文件后，解压缩到.kodi/addon文件夹
其中/lib中的内容可以根据自己的相关平台到plex下载压缩包，然后解压其中的lib文件到这里即可，这里有个升级版本的命令供参考：
```
#### Update Plex latest version
curl -L -o /tmp/plexserver.deb "https://plex.tv/downloads/latest/1?channel=8&build=linux-ubuntu-x86_64&distro=ubuntu&X-Plex-Token=Uk9xht7VEWU7VbPV3Fpq"
cd /tmp
rm -rf /tmp/usr/ /tmp/etc/ /tmp/lib/
ar -vx plexserver.deb
tar -xvzf data.tar.gz
rm -rf /storage/.kodi/addons/service.multimedia.plexmediaserver/lib.old
systemctl stop service.multimedia.plexmediaserver.service
mv /storage/.kodi/addons/service.multimedia.plexmediaserver/lib /storage/.kodi/addons/service.multimedia.plexmediaserver/lib.old
mv /tmp/usr/lib/plexmediaserver /storage/.kodi/addons/service.multimedia.plexmediaserver/lib
systemctl start service.multimedia.plexmediaserver.service
```
再到kodi界面开启服务即可

# 自动挂载nfs或者samba文件夹
参考nfs.mount.sample和cifs.mount.sample
其中需要注意的是文件最后的命名必须是 文件名的路径.mount('/'用'-'代替)
举个例子
```
[Unit]
Description=test nfs mount script
Requires=network-online.service
After=network-online.service
Before=kodi.service

[Mount]
What=192.168.1.1:/movies #根据自己的实际ip填写
Where=/storage/movies2
Options=
Type=nfs

[Install]
WantedBy=multi-user.target
```
上面例子中挂载到了 "/storage/movies2" ( "Where=/storage/movies2")
那么这个文件必须命名成storage-movies2.mount
让后放到/storage/.config/system.d/ 文件夹下
随机启动命令
```
systemctl enable storage-movies2.mount
```

# 剪切板应用样本


可以在这里下载，有Koodous帐号的话也可以到[Koodous](https://koodous.com/apks/2f8a01035e0409d1a44c5d658bac0ba4e900df6f017556ce07b33a6c5c9ffa99)下载，还可以从一加官网下载相关ROM，比如[这里](http://download.h2os.com/OnePlus%203T/OPEN/OnePlus3THydrogen_28_OTA_042_all_1801181105_27ae1c3d35234b93.zip)，再从中提取应用。


## 从ROM中提取应用

解压ROM，得到下面的文件：

```
boot.img(内核)
firmware-update/
META-INF/
RADIO/
system.new.dat(压缩后的系统分区)
system.patch.dat(用于OTA)
system.transfer.list
```


使用[sdat2img.py](https://github.com/xpirt/sdat2img)把解压后的文件生成一个ext4文件系统的文件。

```
sdat2img.py system.transfer.list system.new.dat system.img
mkdir tmp
sudo mount -t ext4 system.img tmp/
```

剪切板应用位于`./app/OPClipBoardManager/OPClipBoardManager.apk`。







注：也可以使用`tools`目录下的[sdat2img.py](https://github.com/explorerlxz/OPClipBoardManager/blob/master/tools/sdat2img.py)










## 参考链接

 - [XDA developer Unpack/re-pack android DAT files](https://forum.xda-developers.com/android/software-hacking/how-to-conver-lollipop-dat-files-to-t2978952)

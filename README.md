# Grub2 学习与实现

## 目录结构

- `boot`: 在 `ubunt` 下使用 `sudo grub-install --root-directory=/media/u /dev/sdb` 生成的文件；
- `config`: 放置 grub 的配置文件：
> - `config/grub-learn.cfg`: 主要用于学习；
> - `config/grub-iso-installer.cfg`: 主要用于使用各种系统的 iso 文件进行系统安装;

## Grub 讲解

### 设置变量

在 grub 中设置变量使用如下命令：

```sh
set var_name=var_value
```

> - `var_name` 是变量名；
> - `var_value` 是变量名对于的值；


```conf
# 当开机时选中某个菜单项启动时，该菜单的title将被赋值给chosen变量。该变量一般只用于引用，而不用于修改。
# #set chosen=

# grub2加载的core.img的目录路径，是绝对路径，即包括了设备名的路径，如(hd0,gpt1)/boot/grub2/。该变量值不应该修改
# #set cmdpath=

# default指定默认菜单时，可使用菜单的title，也可以使用菜单的id，或者数值顺序，当使用数值顺序指定default时，从0开始计算
set default=0

# 设置菜单等待超时时间，设置为0时将直接启动默认菜单项而不显示菜单，设置为"-1"时将永久等待手动选择。
set timeout=-1

# 当默认菜单项启动失败，则使用该变量指定的菜单项启动，指定方式同default，可使用数值(从0开始计算)、title或id指定。
# #set fallback=1

# 指定该平台是"pc"还是"efi"，pc表示的就是传统的bios平台。该变量不应该被修改，而应该被引用，例如用于if判断语句中。
# #set grub_platform=

# 在grub启动的时候，grub自动将/boot/grub2目录的绝对路径赋值给该变量
# #set prefix=(hd0,gpt1)/boot/grub2/

# 该变量指定根设备的名称，使得后续使用从"/"开始的相对路径引用文件时将从该root变量指定的路径开始。一般该变量是grub启动的时候由grub根据prefix变量设置而来的。
# set root='hd0,msdos1'
```

## insmod

用于加载模块程序，对应的模块文件可以在项目中的 `boot/grub/i386-pc` 中找到；

```conf
# 加载 fat 模块，U盘使用 fat32 格式，需要该模块
insmod fat
# loopback 有点类似 chroot，改变根目录，主要是将 root 重新定位到 iso 的镜像文件方便安装
insmod loopback
# 解析 iso 文件
insmod iso9660
```

参考：
- [https://www.cnblogs.com/f-ck-need-u/p/7094693.html](https://www.cnblogs.com/f-ck-need-u/p/7094693.html)
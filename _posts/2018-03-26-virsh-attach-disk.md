# CentOS 7上使用virsh为kvm虚拟机添加磁盘
-----

### 1. 前言

日常使用KVM的过程中会碰到磁盘空间不够需要扩容的问题。

本文档主要说明在CentOS 7系统上使用virsh命令为KVM虚拟机添加一个新的磁盘或者设备。

本文档只在CentOS 7系统上测试通过，其他系统请自行测试。

使用virsh命令添加磁盘有两种方式。一种是通过virsh attach-disk来完成，另一种是通过virsh attach-device来完成。

attach-disk添加完磁盘后是需要重新启动系统才能使用此磁盘；attach-device添加完磁盘不用重新启动系统就可以使用。

### 2. 创建磁盘文件

在CentOS 7系统上通过qemu-img创建一个新的磁盘文件。文件格式使用qcow2，文件最大占用10GB磁盘空间。

	qemu-img create -f qcow2 /data/storage/data01.qcow2 10G



### 3. 动态添加

virsh attach-device命令可以动态的添加和卸载设备，由于此命令识别xml配置文件，所以挂载之前需要先创建一个xml文件。

	<disk type='file' device='disk'>
	  <driver name='qemu' type='qcow2'/>
	  <source file='/data/storage/data01.qcow2'/>
	  <target dev='vdb' bus='virtio'/>
	  <address type='pci' domain='0x0000' bus='0x00' slot='0x10' function='0x0' />
	</disk>

此文件中的address标签可以不用指定，由系统自动分配。

文件编辑完成后保存成一个xml文件，本文档中起名为disk01.xml。

假设我们要把这个disk01.xml中指定的设备挂载到domain name为 centos7的虚拟机上则挂载命令为：

	# virsh attach-device centos7 ./disk01.xml
	Device attached successfully

此时从虚拟机的操作系统(CentOS7)内用fdisk -l就能看到系统新识别了一个磁盘。

### 4. 动态卸载

执行如下命令动态卸载章节3中挂载的磁盘

	# virsh detach-device centos7 ./disk02.xml
	Device detached successfully

卸载完成。	

## 3. 静态添加

使用attach-disk命令添加完磁盘后操作系统不能使用，还要使用virsh destroy命令重新加载此虚拟机。命令格式如下

	# virsh attach-disk --domain centos7 --source /data/storage/data01.qcow2 --target sdb --live --current --address scsi:1.0.0

	# virsh destroy centos7

	# virsh start centos7
	


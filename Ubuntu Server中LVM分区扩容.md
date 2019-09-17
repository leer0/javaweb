#### Ubuntu Server中的LVM分区调整大小

UbuntuServer在安装的时候启用了LVM，导致其中的根目录只有3.9G

需要利用LVM命令调整根目录分区的大小。

```
lvextend -L 120G /dev/mapper/ubuntu--vg-ubuntu--lv     //增大至120G
lvextend -L +20G /dev/mapper/ubuntu--vg-ubuntu--lv     //增加20G
lvreduce -L 50G /dev/mapper/ubuntu--vg-ubuntu--lv      //减小至50G
lvreduce -L -8G /dev/mapper/ubuntu--vg-ubuntu--lv      //减小8G
lvresize -L  30G /dev/mapper/ubuntu--vg-ubuntu--lv     //调整为30G
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv            //执行调整
```

调整根目录分区之后，使用df -h查看磁盘容量
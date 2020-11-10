# Linux IO

## IO中断原理

[](https://github.com/christophe-liwei/k8s/blob/master/Linux%20IO/%E4%BC%A0%E7%BB%9FIO%E4%B8%AD%E6%96%AD.png)

## DMA

[](https://github.com/christophe-liwei/k8s/blob/master/Linux%20IO/DMA.png)

DMA 可以替换cpu copy，减轻CPU的压力。

## 传统IO

[](https://github.com/christophe-liwei/k8s/blob/master/Linux%20IO/%E4%BC%A0%E7%BB%9FIO%E5%8E%9F%E7%90%86.png)



传统 I/O 操作的数据读写流程，整个过程涉及 2 次 CPU 拷贝、2 次 DMA 拷贝总共 4 次拷贝，以及 4 次上下文切换。

- 上下文切换：当用户程序向内核发起系统调用时，CPU 将用户进程从用户态切换到内核态；当系统调用返回时，CPU 将用户进程从内核态切换回用户态。
- CPU拷贝：由 CPU 直接处理数据的传送，数据拷贝时会一直占用 CPU 的资源。
- DMA拷贝：由 CPU 向DMA磁盘控制器下达指令，让 DMA 控制器来处理数据的传送，数据传送完毕再把信息反馈给 CPU，从而减轻了 CPU 资源的占有率。


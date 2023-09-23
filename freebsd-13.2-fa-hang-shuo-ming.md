# FreeBSD 13.2 发行说明

> 原文链接 [FreeBSD 13.2-RELEASE Release Notes](https://www.freebsd.org/releases/13.2R/relnotes/)

#### 摘要

FreeBSD 13.2-RELEASE 的发行说明包含了在 13-STABLE 开发线上对 FreeBSD 基本系统所做修改的摘要。这份文件列出了自上次发布以来所发布的相关安全公告， 以及对 FreeBSD 内核和用户区的重大修改。同时还介绍了一些关于升级的简要说明。

#### 简介

这份文件包含了 FreeBSD 13.2-RELEASE 的发行说明。它说明了 FreeBSD 最近新增、变化或删除的功能。它还提供了一些关于从旧版 FreeBSD 的升级说明。

这些发行说明所适用的发行版， 代表了自 13-STABLE 创建以来沿 13-STABLE 开发分支的最新进展。有关这个分支的预编译二进制发行版的信息， 可以在 [https://www.FreeBSD.org/releases/](https://www.freebsd.org/releases/) 找到。

这些发行说明所适用的发行版本代表了 13-STABLE 开发分支中介于 13.1-RELEASE 和日后 13.3-RELEASE 之间的一个点。有关这个分支的预编译二进制发行版的信息，可以在 [https://www.FreeBSD.org/releases/](https://www.freebsd.org/releases/) 找到。

这个 FreeBSD 13.2-RELEASE 的发行版是一个 RELEASE。它可以在 [https://www.FreeBSD.org/releases/](https://www.freebsd.org/releases/) 或其任何一个镜像中找到。关于获得这个(或其他) FreeBSD 发行版的更多信息，可以在 FreeBSD 手册的附录中找到。

我们鼓励所有用户在安装 FreeBSD 之前参考发行勘误表。勘误表文件会根据发布周期晚期或发布后发现的“迟发”信息进行更新。通常情况下， 它包括已知的错误， 安全建议， 以及对文档的修正。FreeBSD 13.2-RELEASE 的勘误表的最新版本可以在 FreeBSD 网站上找到。

这份文件描述了自 13.1-RELEASE 以来 FreeBSD 中最容易被用户看到的新功能或变化。一般来说， 这里描述的变化都是 13-STABLE 分支所特有的， 除非特别标注为合并的特性。

典型的发布说明记录了在 13.1-RELEASE 之后发布的安全公告， 新的驱动或硬件支持，新的命令或选项，主要的错误修正，或贡献的软件升级。它们也可能列出主要的 port/包的变化或发布工程实践。显然，发行说明不可能列出 FreeBSD 在不同版本之间的每一个变化；这份文件主要关注安全公告、用户可见的变化， 以及主要的架构改进。

#### 从旧版 FreeBSD 升级

使用 freebsd-update(8) 工具可以在 RELEASE 版本 (以及各种安全分支的快照) 之间进行二进制升级。二进制升级过程将更新未修改的用户区工具， 以及作为官方 FreeBSD 发行版一部分的未修改的 GENERIC 内核。freebsd-update(8) 工具要求被升级的主机有互联网连接。

根据 /usr/src/UPDATING 中的说明，可以支持基于源代码的升级 (那些基于从源代码重新编译的 FreeBSD 基本系统)。

所有 PowerPC 架构的用户，在成功安装内核和 world 之后，必须手动运行 `kldxref /boot/kernel`。

> _只有在备份了 **所有的** 数据和配置文件之后，才可以尝试升级 FreeBSD。_

> _在安装了新的用户级软件之后，运行的守护程序仍然是以前的版本。在通过第二次调用 freebsd-update 来安装用户级组件后，或者通过 `installworld` 从源代码升级后，系统应该重新启动，以便用新的软件启动一切。例如，旧版本的 sshd 在安装了新的 /usr/sbin/sshd 后，无法正确处理传入的连接；重启后会启动新的 sshd 和其他守护程序。_

#### 用户空间

这部分介绍了一些对用户应用程序、贡献软件和系统工具的更改和添加。

* 在用户配置更改方面，growfs(7) 的启动脚本现在可以在扩展根分区的同时添加交换分区（如果之前没有交换分区）。这在 SD 卡上使用 RAW 镜像安装系统时非常有用。新增了一个 rc.conf(5) 变量——growfs\_swap\_size，可以在需要时控制添加交换分区的大小。详情请参考 growfs(7)。
* 新增了一个 RC 脚本 zpoolreguid，可为一个或多个 zpool 分配新的 GUID，这在共享数据集的虚拟化环境中非常有用。
* hostid 启动脚本现在可以在没有 /etc/hostid 文件和来自硬件的有效 UUID 的情况下生成一个随机的（第 4 版）的 UUID。此外，如果没有 /etc/machine-id 文件，hostid\_save 脚本将在 /etc/machine-id 中存储一个紧凑版本的 hostid（不带连字符）。该文件被诸如 GLib 之类的库使用。
* 现在可以使用 defaultrouter\_fibN 和 ipv6\_defaultrouter\_fibN rc.conf(5) 变量为非主要的 FIB 添加默认路由。 （由 ScaleEngine Inc. 赞助）

以下是一些常用的用户应用程序改进:

* 工具 bhyve(8) 现在支持 virtio-input 设备仿真。这将用于向客户机注入键盘/鼠标输入事件。命令行语法为: `-s <slot>,virtio-input,/dev/input/eventX`。
* 工具 kdump(1) 现在支持解码 Linux 系统调用。
* 工具 killall(1) 现在允许使用语法 `-t pts/N` 向具有其控制终端在 pts(4) 上的进程发送信号。
* 添加了软件 nproc(1)，与同名的 Linux 程序兼容。
* 软件 timeout(1) 已从 /usr/bin 移动到 /bin。
* 软件 pciconf(8) 添加了对 ACS 解码扩展功能的支持。(由 Chelsio Communications 赞助)
* 软件 procstat(1) 现在可以使用新添加的 advlock 命令打印关于文件上的 advisory locks 的信息。
* pwd\_mkdb(8) 不再将 /etc/master.passwd 中的注释复制到 /etc/passwd。
* ppp(8) 的 MSS clamp 已得到改进。
* prometheus\_sysctl\_exporter(8) 中的指标别名已更改，以避免由于度量名称冲突而混淆 Prometheus 服务器。tcp\_log\_bucket UMA 区已重命名为 tcp\_log\_id\_bucket，并且 tcp\_log\_node 已重命名为 tcp\_log\_id\_node 以保持一致性。不再导出带有描述中的（LEGACY）的 Sysctl 变量，这些变量由已被其他变量替换的 ZFS sysctl 使用，其中许多变量别名为相同的 Prometheus 度量名称(例如 vfs.zfs.arc\_max 和 vfs.zfs.arc.max)。(由 Axcient 赞助)
* uuidgen(1)实用程序具有一个新选项 `-r`，可以生成一个随机的 UUID，版本为 4。
* 在由 inetd(8) 调用时，`ctlstat -P` 现在会生成适合于将其摄入 Prometheus 的输出；请参阅 ctlstat(8)。(由 Axcient 赞助)

以下是一些软件的升级信息：

* 软件 bc 已经升级到了 6.2.4 版本，由 Gavin Howard 维护。
* 软件 expat (libbsdxml) 已经升级到了 2.5.0 版本。
* 软件 file 已经升级到了 5.43 版本。
* 软件 less 已经升级到了 608 版本。
* 软件 libarchive 已经升级到了 3.6.2 版本，并进行了多项可靠性修复。请参阅发行说明：[https://github.com/libarchive/libarchive/releases](https://github.com/libarchive/libarchive/releases)。
* 软件 libedit 已经升级到了 2022-04-11 版本。
* LLVM 和 clang 编译器已经升级到了 14.0.5 版本。
* 现在在 powerpc64 和其它架构上启用了支持的 LLVM sanitizer。
* 软件 mandoc 已经升级到了 1.14.6 版本。
* 软件 OpenSSH 已经升级到了 9.2p1 版本。
* 软件 OpenSSL 已经升级到了 1.1.1t 版本。
* 软件 sendmail 已经升级到了 8.17.1 版本。
* 软件 sqlite3 已经升级到了 3.40.1 版本。
* 软件 tzcode 已经升级到了 2022g 版本，并提高了时区更改检测和可靠性修复。
* 软件 tzdata 已经升级到了 2023b 版本。
* 软件 unbound 已经升级到了 1.17.1 版本。
* 软件 xz 软件已经升级到了 5.4.1 版本。
* 软件 xz-embedded 软件已经升级到了 3f438e15109229bb14ab45f285f4bff5412a9542 版本。
* 软件 zlib 已经升级到了 1.2.13 版本。

#### 运行时库和 API

libmd 现在支持 SHA-512/224。此功能由 Klara, Inc. 赞助。

现在，sysdecode(3)和 kdump(1) 支持 Linux 风格的系统调用跟踪。

本机 pthread 库函数现在可以支持 Linux 语义。

#### 内核

此部分包括了对内核配置、系统调整和系统控制参数的更改，这些更改没有分类到其他部分。

**通用内核变更：**

现在 bhyve(8) 虚拟化管理程序和内核模块 vmm(4) 支持在虚拟机中使用超过 16 个虚拟 CPU。在默认情况下，bhyve 允许每个虚拟机创建的虚拟 CPU 数量与主机上的物理 CPU 数量相同。可通过 loader 可调整参数 `hw.vmm.maxcpu` 来更改此限制。这个变更的提交哈希值为 3e02f8809aec。

64 位可执行文件的地址空间布局随机化 (ASLR) 已默认启用。如果应用程序出现意外故障（例如段错误），则可以根据需要对其禁用。要禁用单个调用的 ASLR，请使用 proccontrol(1) 命令：`proccontrol -m aslr -s disable` 命令。要禁用二进制文件的所有调用的 ASLR，请使用 elfctl(1) 命令：`elfctl -e +noaslr` 文件名。如果有问题，请通过问题报告系统 [https://bugs.freebsd.org](https://bugs.freebsd.org) 或在 freebsd-stable@FreeBSD.org 邮件列表中发布问题。这个变更的提交哈希值为 10192e77cfac，由 Stormshield 赞助。

针对英特尔 Alder Lake（第十二代）和可能的在 Raptor Lake（第十三代）混合 CPU 上的一些硬件页面失效问题，实现了一种解决方法。该 bug 可导致 UFS 和 MSDOSFS 文件系统损坏，并可能导致其他内存损坏。慢速核心 (E-cores) 将自动使用更慢的页面失效方法来解决此问题。这个变更的提交哈希值为 567cc4e6bfd9，由 FreeBSD 基金会赞助。

现在有一个新的内核配置选项 SPLIT\_KERNEL\_DEBUG，可以控制将内核和模块的调试数据分别拆分为单独的文件。该选项与 WITHOUT\_KERNEL\_SYMBOLS 选项交互，其行为与 13.0-RELEASE 和 13.1-RELEASE 不同，但与之前的版本相似。它现在仅控制调试数据的安装。默认值为 WITH\_KERNEL\_SYMBOLS 和 WITH\_SPLIT\_KERNEL\_DEBUG，允许在 /boot 中安装没有调试数据的内核和模块，并在 /usr/lib/debug 中安装独立的调试文件，这是在 13.0-RELEASE 之前的版本中默认情况下所做的。使用 WITHOUT\_KERNEL\_SYMBOLS 和 WITH\_SPLIT\_KERNEL\_DEBUG，将生成独立的调试文件，但不会安装，这与在 13.0-RELEASE 之前的版本中使用 WITHOUT\_KERNEL\_SYMBOLS 时一样。最后，使用 WITHOUT\_KERNEL\_SYMBOLS 和 WITHOUT\_SPLIT\_KERNEL\_DEBUG，在 /boot 中安装具有内置调试信息的内核和模块，就像在 13.1-RELEASE 中使用 WITHOUT\_KERNEL\_SYMBOLS 一样。 哈希值：0c4d13c521aa（由 FreeBSD 基金会赞助）

在 PowerPC 上，pseries 中支持 ISA 3.0 的基数 pmap。这应该会使在 POWER9 实例上的 pseries 显着加快，因为现在需要较少的超级调用来管理 pmap。哈希值：c74c77531248

在 arm64 上现在支持对 L inux 进程的 ptrace（2）的支持。哈希值：99950e8beb72

为了促进稳定分支的 ABI 兼容性，CPU 亲和性系统调用现在更加容忍 CPU 集比内核使用的小。这将有助于增加内核集 MAXCPU 的大小。哈希值：72bc1e6806cc

添加了保存 CPU 浮点状态以在信号传递期间跨越的 64 位 linux（4）ABI 支持。哈希值：0b82c544de58，20d601714206

linux（4）ABI 中的 vDSO（虚拟动态共享对象）支持已经接近完成。哈希值：a340b5b4bd48

将 arm64 linux（4）ABI 的状态与 amd64 linux（4）ABI 保持一致。哈希值：0b82c544de58，a340b5b4bd48

**设备驱动程序**

这部分介绍自 13.1-RELEASE 以来设备和设备驱动程序的变化和增加。

* 驱动程序 em(4) 现在正确支持较新芯片 82580 和 i350 上可用的完整接收缓冲区大小范围。 哈希值：3f8306cf8e2d
* 驱动程序 ena(4) 已升级到了 2.6.2 版本。（由 Amazon，Inc.赞助）
* 已为 hwpmc(4) 实现对 Intel Alder Lake CPU 的基本支持。 哈希值：b8ef2ca9eae9
* 驱动程序 ice(4) 已更新到了 1.37.7-k 版本。
* 引入了驱动程序 irdma(4) RDMA，用于 Intel E810 以太网控制器，以 per-PF 方式支持 RoCEv2 和 iWARP 协议，其中 RoCEv2 是默认协议，并升级到了 1.1.5-k 版本。哈希值： 42bad04a2156（由 Intel Corporation 赞助）
* 现在提供 DPAA2（第二代数据路径加速架构-在一些 NXP SoC 中发现的硬件级网络架构）的初始支持。它运行由 NXP 提供的固件，该固件提供 DPAA2 对象作为抽象层，并提供 dpni 网络接口。 哈希值：d5a64a935bc9（由 Bare Enthusiasm :)和 Traverse Technologies 赞助）
* 更新了 Intel 无线接口的驱动程序 iwlwifi(4)。（由 FreeBSD 基金会赞助）
* 添加了驱动程序 rtw88(4)，以支持多个 Realtek 无线 PCI 接口。目前仅限于 802.11 a / b / g 操作。请参阅 [https://wiki.freebsd.org/WiFi/Rtw88](https://wiki.freebsd.org/WiFi/Rtw88) 获取更多信息。
* 对于支持 Linux 设备驱动程序的 KPI 进行了许多添加和改进。（由 FreeBSD 基金会赞助）

#### 支持的平台

#### 存储

本节介绍了本地和网络文件系统以及其他存储子系统的更改和添加。

#### 通用存储

**ZFS 更改**

ZFS 已升级到 OpenZFS release 2.1.9。可以在 [https://github.com/openzfs/zfs/releases](https://github.com/openzfs/zfs/releases) 找到 OpenZFS 的发行说明。

**NFS 更改**

修复了导致 NFS 服务器挂起的问题。该问题是由 TCP 中 SACK 处理的错误引起的。

**UFS 更改**

现在在运行日志软更新时可以在 UFS 文件系统上创建快照。因此，现在可以在使用日志软更新运行的活动文件系统上执行后台转储。通过在 dump（8）中使用 `-L` 标志请求后台转储。哈希值： 3f908eed27b4（由 FreeBSD 基金会赞助）

**引导加载程序更改**

本节包括了引导加载程序、引导菜单和其他与引导相关的更改。

**引导加载程序更改**

teken.fg\_color 和 teken.bg\_color loader.conf（5）变量现在接受亮或淡色前缀（和颜色号 8 到 15）来选择亮色。1dcb6002c500（由 FreeBSD 基金会赞助）。另请参见哈希值： 233ab015c0d7

已修复了 loader（8）中的几个错误，导致视频控制台输出消失。这些错误似乎是在引导加载程序启动内核后出现的挂起。（由 Netflix 赞助）

#### 其他启动更改

**网络**

本节说明了影响 FreeBSD 中网络的更改。

**一般网络**

内核已经重新集成了驱动程序 wg(4) WireGuard，它提供使用了 WireGuard 协议的虚拟专用网络（VPN）接口。（由 Rubicon Communications，LLC（“Netgate”）和 FreeBSD 基金会赞助）。

内核 TLS 实现（KTLS）已添加了对 TLS 1.3 的接收卸载支持。现在，TLS 1.1 到 1.3 都支持接收卸载；TLS 1.0 到 1.3 支持发送卸载。（由 Netflix 赞助）

现在可以使用 netlink(4) 网络配置协议。它是在 RFC 3549 中定义的通信协议，并使用原始套接字在用户空间和内核之间交换配置信息。第三方路由程序和 linux(4) ABI 都使用它。netlink(4) 协议未被包括在 13.2-RELEASE 的 GENERIC（译者注：通用内核）配置中，但可用作内核模块。

现在可以在 ipfw(4) 中支持基数表和查找 MAC 地址。这可以构建和使用 MAC 地址表进行过滤。

内核模块 dpdk\_lpm4 和 dpdk\_lpm6 现已可用，可以通过 loader.conf(5) 进行加载。它们为具有大量路由表的主机提供了优化的路由功能。它们可以通过 route(8) 进行配置，是模块化 FIB 查找机制的一部分。

TCP 和 SCTP 中有大量的 bug 修复。

#### 通用注释——后续 FreeBSD 版本的注意事项：

OPIE 已弃用并将在 FreeBSD 14.0 中删除。

ce(4) 和 cp(4) 同步串行驱动程序已被弃用，并将在 FreeBSD 14.0 中删除。

ISA 声卡的驱动程序已被弃用，并将在 FreeBSD 14.0 中删除。d7620b6ec941 (由 FreeBSD 基金会赞助)

工具 mergemaster(8) 已被弃用，并将在 FreeBSD 14.0 中删除。它的替代品是 etcupdate(8)。5fa16e3c50c5 (由 FreeBSD 基金会赞助)

工具 minigzip(1) 已被弃用，并将在 FreeBSD 14.0 中删除。84d3fc26e3a2

netgraph 中的 ATM 的其余组件（NgATM）已被弃用，并将在 FreeBSD 14.0 中删除。对 ATM NIC 的支持先前已被删除。

Telnet 的守护程序 telnetd(8) 已被弃用，并将在 FreeBSD 14.0 中删除。Telnet 客户端不受其影响。

geom(8) 中的 VINUM 类已被弃用，并将在以后的版本中删除。

**默认的 CPUTYPE 更改**

从 FreeBSD-13.0 开始，i386 架构的默认 CPUTYPE 将从 486 更改为 686。

这意味着，通过默认设置生成的二进制文件将需要 686 级别的 CPU，包括但不限于 FreeBSD Release Engineering 团队提供的二进制文件。FreeBSD 13.x 将继续支持旧的 CPU，但使用者需要自行构建其官方支持的发行版。

由于 i486 和 i586 CPU 的主要用途通常是嵌入式市场，因此对一般的终端用户的影响预计将很小，因为拥有这些 CPU 类型的新硬件早已消失，而大部分这类系统的已部署基础正处于退役年龄，这是统计数据所显示的。

这个改变考虑了几个因素。例如，i486 没有 64 位原子操作，虽然它们可以在内核中进行仿真，但不能在用户空间中进行仿真。此外，32 位的 amd64 库自其问世以来就是 i686。

由于大部分 32 位测试是由开发人员使用内核中的 COMPAT\_FREEBSD32 选项在 64 位硬件上使用 lib32 库来完成的，因此这个变化确保了更好的覆盖率和用户体验。这也与大部分 Linux® 发行版长期以来所做的行为相一致。

这被认为是 i386 默认 CPUTYPE 的最后一次提升。

> **此更改不影响 FreeBSD 12.x 系列的发行版。**

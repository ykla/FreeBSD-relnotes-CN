# FreeBSD 14.1-RELEASE 发行说明

## 摘要

FreeBSD 14.1-RELEASE 的发行说明是对由 14-STABLE 开发线衍生的 FreeBSD 基本系统所做更改的摘要。本文件列出了自上次发布以来发布的适用安全公告，以及对 FreeBSD 内核和用户空间的重大变更。还概述了部分升级备注事宜。

## 介绍

本文档涉及 FreeBSD 14.1-RELEASE 的发布说明。它说明了最近增加、更改、删除的 FreeBSD 特性。还提供了从旧版本升级的一些相关注意事项。

这些发布说明适用于“RELEASE”分行版，代表了自 14-STABLE 分支创建以来的最新状态。有关基于此分支的预构建二进制“RELEASE”的分发信息，请访问 <https://www.FreeBSD.org/releases/>。

这些发布说明适用于“RELEASE”分发版本，代表的节点位于 14.0-RELEASE 与后续 14.2-RELEASE 之间的 14-STABLE 开发分支。有关基于此分支的预构建二进制“RELEASE”的分发信息，请访问 <https://www.FreeBSD.org/releases/>。

该 FreeBSD 14.1-RELEASE 版本是个“RELEASE”版本。可以在 <https://www.FreeBSD.org/releases/> 和其镜像站中找到它。有关下载该（及其他）FreeBSD“RELEASE”版本的更多信息，请参阅 FreeBSD 手册的附录《获取 FreeBSD》。

建议所有用户在安装 FreeBSD 之前查阅发行错误。错误文档会在发布周期后期，随发布后发现的“最新”信息进行更新。通常涉及已知错误、安全公告和文档更正的相关信息。可在 FreeBSD 网站上找到 FreeBSD 14.1-RELEASE 的最新错误信息。

本文档说明了自 14.0-RELEASE 以降，在 FreeBSD 中最具用户可见性的新特性和更改。一般来说，除非其明确标记为 MERGED 特性，否则此处所述的更改是独立于 14-STABLE 分支的。

典型的发布说明项目记录了在 14.0-RELEASE 之后发布的最新安全公告，新的驱动程序或硬件支持，新的命令或选项，重大错误修复或贡献的软件升级。它们还可能列出对主要软件/包或发行工程实践的更改。显然，发布说明无法列出在不同版本之间对 FreeBSD 所做的每一项更改；该文档主要关注安全公告，用户可见的更改以及重大架构改进。

## 从以前的 FreeBSD 版本升级

RELEASE 版本之间（以及各种安全分支的快照）之间使用 freebsd-update(8)实用程序支持二进制升级。请参阅特定于版本的升级过程，FreeBSD 14.1-RELEASE 升级信息，在 FreeBSD 手册的二进制升级过程中有更多细节。这将更新未修改的用户空间实用程序，以及作为官方 FreeBSD 发行版的一部分分发的未修改的 GENERIC 内核。freebsd-update(8)实用程序要求进行升级的主机具有互联网连接。

根据/usr/src/UPDATING 中的说明，支持基于源代码重新编译 FreeBSD 基本系统的源代码升级。

|  | 仅在备份所有数据和配置文件后才应尝试升级 FreeBSD。 |
| -- | ---------------------------------------------------- |

## 安全和勘误

此部分列出了自 14.0-RELEASE 以来的各种安全公告和勘误通知。

### 安全公告

| 咨询       | 日期 | 主题 |
| ------------ | ------ | ------ |
| 没有警告。 |      |      |

### 勘误通知

| 勘误     | 日期 | 主题 |
| ---------- | ------ | ------ |
| 无通知。 |      |      |

## 用户空间

这一部分涵盖了对用户空间应用程序、贡献软件和系统实用程序的更改和添加。

### 用户空间配置更改

现在有一个新的 kdc_restart 变量可用来管理 kdc(8)（或 krb5kdc ）在 daemon(8)下。在 rc.conf(5)中设置 kdc_restart="YES" 以自动重新启动 kdc 以解决异常终止问题。将 kdc_restart_delay="N" 设置为延迟重新启动 kdc 之前的秒数。abc4b3088941

默認情況下，電子郵件中由週期性(8)工具顯示的更改，通過 daily 腳本比以前顯示的上下文要少，以減少輸出大小。此行為可以通過 periodic.conf(5)中的 daily_diff_flags 變量進行控制。同樣，安全腳本顯示的更改比以前少，由 periodic.conf(5)中的 security_status_diff_flags 變量控制。538994626b9f, 37dc394170a5, 128e78ffb084

### 用戶空間應用程序更改

adduser(8)實用程序，被 bsdinstall(8)使用，現在將為位於 ZFS 數據集上的父目錄的新用戶的家目錄創建一個 ZFS 數據集。命令行選項可用於禁用使用單獨的數據集。ZFS 加密 also 可用。516009ce8d38

date(1) 程序现在支持纳秒。例如： date -Ins 打印 "2024-04-22T12:20:28,763742224+02:00"， date +%N 打印 "415050400"。eeb04a736cb9

dtrace(1) 实用程序现在可以使用 libxo(3)生成 JSON、XML 和 HTML 格式的机器可读输出。aef4504139a4 (由 Innovate UK 赞助)

lastcomm(1) 实用程序现在以秒为精度显示时间戳。692c0a2e80c1 (由 DSS Gmbh 赞助)

ldconfig(8) 实用程序现在支持字节顺序为任一字节顺序的提示文件。默认格式为主机的本地字节顺序。fa7b31166ddb

OpenSSH 已升级到 9.7p1 版本。完整的发布说明请参见 https://www.openssh.com/txt/release-9.7 和 https://www.openssh.com/txt/release-9.6 。a25789646d71, 464fa66f639b（由 FreeBSD 基金会赞助）

usbconfig(8) 实用程序现在在可用时从 /usr/share/misc/usb_vendors 读取 usb 供应商和产品的描述，类似于 pciconf(8) 的操作。7b9a772f9f64

### 贡献软件

One True Awk（awk(1)）已更新到第二版，新增了-csv 支持和 UTF-8 支持。daf917daba9c

Clang/LLVM 已升级到 18.1.5 版本。90a5e985e5f4

libarchive(3) 库已升级到版本 3.7.4。8774c92e32b2

sendmail(8) 套件已升级到版本 8.18.1，解决了 CVE-2023-51765 漏洞。58ae50f31e95

unbound(8) 解析器已升级到版本 1.20.0，并修复了“DNSBomb”漏洞，CVE-2024-33655。dcde37c4170b

### 运行时库和 API

在 libutil 中的 setusercontext(3)例程现在将根据适当条件从家目录下的.login.conf 文件以及系统 login.conf(5)设置进程优先级（nice）。优先级现在可以具有值 inherit ，表示优先级应保持与父进程相同。类似地，umask 可以具有值 inherit 。6f6186e19fe5, a8c273b3c97f, d2d66fedc418（由 Kumacom SAS 赞助）

C 库中的许多字符串和内存操作现在在 amd64 系统上可用时使用 SIMD（单指令多数据）扩展以提高性能；请参阅 simd(7)。（由 FreeBSD 基金会赞助）

现在，在支持的平台上, math(3)库中的 128 位 tgammal 函数实现有了更好的实现。8df6c930c151

## 云支持

本节涵盖了对云环境支持的更改。

14.1-RELEASE 支持 cloudinit，包括 nuageinit 启动脚本和对 config-drive 分区的支持。它与 OpenStack 和许多托管设施兼容。请参阅 cloud-init 网站和提交消息，16a6da44e28d 227e7a205edf。（由 OVHCloud 赞助）

## 内核

本节涵盖了对内核配置、系统调优和系统控制参数的更改，这些更改没有其他分类。

### 通用内核更改

已为 powerpc 实现了 fpu_kern_enter 和 fpu_kern_leave 例程，允许在使用浮点和矢量寄存器的内核中使用 ossl(4) 加密函数。91e53779b4fc

## 设备和驱动程序

本节内容涵盖自 14.0-RELEASE 以来的设备和设备驱动程序的更改和添加。

### 设备驱动程序

Intel E800 系列中的 ice(4) 以太网网络控制器现已提供驱动程序，支持 100 Gb/s 操作。该驱动程序已升级到版本 1.39.13-k. 71d104536b51 f6de0a7c94e9（由英特尔公司赞助）。

英特尔 Wi-Fi 设备的 iwlwifi(4) 驱动程序已经进行了许多稳定性改进。(由 FreeBSD 基金会赞助)

现在在 amd64 和 i386 上支持多个 PCI MCFG 区域，允许对除 0 之外的域（段）进行 PCI 配置空间访问。4b5f64408804

smsc(4) 以太网驱动程序现在可以获取一些树莓派型号传递的 smsc95xx.macaddr 值，并将其用于 MAC 地址。即使 EEPROM 中没有地址，它也始终使用稳定的 MAC 地址。028e4c6548e4

从声音子系统中移除了 snd_clone 框架，包括相关的 sysctl，简化了系统。不再创建每个通道节点（/dev/dspX.Y），只创建主设备（/dev/dspX）。e6c51f6db8d7（由 FreeBSD 基金会赞助）

现在音频支持异步设备分离。这极大地简化了诸如 USB 耳机之类的热插拔操作，并且在需要操作系统休眠和唤醒（挂起和恢复）的情况下，使用 PulseAudio 变得更加容易。d692c314d29a（由 FreeBSD 基金会赞助）

## 存储

此部分涵盖了对文件系统和其他存储子系统（包括本地和网络存储）的更改和添加。

### NFS

mountd(8) 服务器已修改为使用 strunvis(3) 解码 exports(5) 文件中的目录名称。这允许特殊字符（如空格）嵌入到目录名称中。可以使用 vis -M 对这类目录名称进行编码；请参阅 vis(1)。2c83f1ada435

新的 sysctl（8）变量已添加到 kern.rpc.unenc 和 kern.rpc.tls 下，允许 NFS 服务器管理员确定正在使用多少 NFS-over-TLS。大量失败的握手可能表明 NFS 配置问题。b8e137d8d32d

### UFS

使用 newfs（8）创建新的 UFS 文件系统时，现在默认启用软更新。6b2af2d88ffd

### ZFS

OpenZFS 已升级至版本 2.2.4. 78c9d8f1ce65

## 引导加载程序更改

本节涵盖了引导加载程序、引导菜单和其他与引导相关的更改。

### 引导加载程序更改

现在 loader(8) 在其他配置文件之后读取列在变量 local_loader_conf_files 中的本地配置文件， 默认为 /boot/loader.conf.local。a25531db0fc2

装载程序(8)现在可以根据来自 SMBIOS 的平面制造商、平面产品、系统产品和 uboot m_product 变量读取特定配置文件进行配置。目前，最好的文档是 git 提交消息，3eb3a802a31b。

在 EFI 系统中，装载程序(8)中的控制台检测得到了改进。如果没有 ConOut 变量，会检查 ConIn。如果找到多个设备，则首选串行。20a6f4779ac6（由 Netflix 赞助）

装载程序(8)中的帧缓冲区支持现在可以使用仅文本视频驱动程序，从而节省空间。57ca2848c0aa（由 Netflix 赞助）

ACPI 的检测现在在 arm64 系统的 loader.efi(8)中更早完成。在使用 ACPI 的 arm64 系统上，应更新 EFI 分区上 loader.efi 的副本。05cf4dda599a 16c09de80135

LinuxBoot 加载程序可用于在 aarch64 系统和 amd64 系统上从 Linux 引导 FreeBSD。46010641267（由 Netflix 赞助）

## 网络

本节描述了影响 FreeBSD 网络的变更。

### 通用网络

ARP（arp（4））已经为 802 标准网络恢复支持；它曾经在 FDDI 支持中被意外移除。（这与以太网标准封装不同。）d776dd5fbd48

可以构建一个带有 IPv6 支持（INET6）但不带有 IPv4（INET）的内核。6df9fa1c6b83 和其他

netgraph ng_ipfw(4) 模块不再将 cookie 截断为 16 位，允许使用完整的 32 位。dadf64c5586e

## 硬件支持

本节涵盖了物理机器、虚拟化环境的一般硬件支持，以及不适用于本文档其他部分的硬件更改和更新。

请查看 14.1-RELEASE 支持的硬件列表，以及支持的 CPU 架构完整列表页面。

## 文档

本节涵盖了与基本系统一起提供的手册（man(1)）页面和其他文档的更改。

### 手册页面

新的网络(7)手册页面提供了连接系统到网络（包括 Wi-Fi）的快速入门指南，并链接到其他手册页面和手册。39f92a4c4c49

## 未来 FreeBSD 版本发布的一般注意事项

FreeBSD 15.0 预计不会支持 armv7 以外的 32 位平台。 armv6、i386 和 powerpc 平台已被弃用并将被移除。 64 位系统仍可以运行旧的 32 位二进制文件。

我们预计将在 FreeBSD 15.0 及 stable/15 中支持 armv7 作为第二级架构。然而，我们也预计 armv7 可能会在 FreeBSD 16.0 中被移除。我们将在 15.0 发布时提供有关 armv7 在 15.x 和 16.x 中状态的更新。

对于通过 COMPAT_FREEBSD32 选项在 64 位平台上执行 32 位二进制文件的支持将至少在稳定/15 和稳定/16 分支上继续。至少在稳定/15 分支上，还将继续支持通过 cc -m32 编译单个 32 位应用程序，其中包括/usr/include 中的适当头文件和/usr/lib32 中的库。

对于 FreeBSD 15.0 及以后的版本，Ports将不包括对废弃的 32 位平台的支持。这些未来版本将不包括废弃的 32 位平台的二进制软件包或从ports构建软件包的支持。

FreeBSD 稳定/14 和更早版本分支将保留现有的 32 位内核和系统支持。只要这些分支仍受ports系统支持，Ports将保留对在稳定/14 和早期版本分支上构建ports和软件包的支持。但是，所有 32 位平台都是二级或三级，应该预期支持个别ports将随着上游废弃 32 位平台而降级。

根据当前的支持计划，stable/14 将在 FreeBSD 14.0-RELEASE 发布后的 5 年达到生命周期终点（EOL）。stable/14 的生命周期终点将标志着对包括源代码发布、预构建软件包以及从ports构建应用程序的弃用 32 位平台的支持结束。随着 2023 年 11 月发布 14.0-RELEASE，对弃用 32 位平台的支持将在 2028 年 11 月结束。

当 FreeBSD 15.0 发布时，项目可能选择通过在 15.0 或以后的版本中延长对一个或多个弃用平台的某种程度支持来改变这种方法。任何变更将受到社区反馈和致力于支持这些平台的努力驱动。使用 FreeBSD 14.0-RELEASE 和随后的小版本发布，或者 stable/14 分支，来迁移离开 32 位平台。

---

最后修改于：2024 年 6 月 4 日，作者 Alexander Ziaee

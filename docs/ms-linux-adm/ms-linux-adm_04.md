# 第五章：存储管理

## 磁盘管理和分区

Linux 提供了各种用于磁盘管理和分区的工具和命令。以下是一些常用的工具和技术：

+   fdisk：fdisk 是用于分区磁盘的命令行工具。它允许您在存储设备上创建、删除和管理分区。例如，您可以运行 sudo fdisk -l 来列出可用的磁盘和分区，以及 sudo fdisk /dev/sdX（将“X”替换为适当的磁盘标识符）来开始分区特定磁盘。

+   parted：parted 是另一个用于磁盘分区的命令行实用程序。它提供了比 fdisk 更用户友好的界面。使用 parted，您可以创建、删除、调整大小和管理分区。语法与 fdisk 类似。例如，sudo parted /dev/sdX 将启动特定磁盘的交互式 parted 会话。

+   GParted：GParted 是适用于 Linux 发行版的图形分区编辑器。它提供了一个用户友好的界面来管理分区。GParted 允许您轻松地创建、调整大小、移动和删除分区。您可以使用发行版的软件包管理器安装 GParted，例如在基于 Debian 的系统上使用 sudo apt install gparted。

+   lsblk：lsblk 是一个命令行工具，列出所有可用的块设备的信息，包括磁盘和分区。运行 lsblk 将显示您的设备及其分区的树状视图，以及诸如大小和挂载点等其他详细信息。

+   mkfs：mkfs 用于在分区上创建文件系统。它后面跟着文件系统类型和设备名称。例如，sudo mkfs.ext4 /dev/sdXY 将在分区/dev/sdXY 上创建一个 ext4 文件系统。

+   mount：mount 用于将文件系统附加到 Linux 文件系统的目录树中。例如，sudo mount /dev/sdXY /mnt 将分区/dev/sdXY 挂载到/mnt 目录。

+   blkid：blkid 命令显示有关块设备的信息，包括它们的 UUID（通用唯一标识符）和文件系统类型。它可用于唯一标识设备和分区。

+   LVM（逻辑卷管理器）：LVM 是 Linux 中管理逻辑卷的灵活系统。它允许您动态创建、调整大小和管理逻辑卷，可以跨多个磁盘。关键的 LVM 命令包括 pvcreate、vgcreate、lvcreate 以及它们相应的修改或删除逻辑卷的命令。

在使用磁盘管理工具时要小心，因为操纵分区可能会导致数据丢失，如果操作不正确的话。在执行任何磁盘操作之前，一定要确保重要数据有备份。

## 文件系统类型和管理

Linux 支持各种文件系统类型，每种类型都有其自己的特点和特性。以下是一些常用的 Linux 文件系统类型：

+   ext4：这是 Linux 的默认和最广泛使用的文件系统。它具有良好的性能、可伸缩性和可靠性。它支持大文件大小，并且可以处理高达 1 艾字节大小的文件系统。

+   ext3：ext4 的前身，ext3 是一种日志文件系统，提供了改进的可靠性和在非正常关闭后更快的文件系统检查。它仍然被广泛使用，但正在逐渐被 ext4 取代。

+   XFS：XFS 是一种专为可伸缩性和并行性设计的高性能文件系统。它以高效处理大文件和文件系统而闻名，适用于数据密集型应用程序。

+   Btrfs：Btrfs（B 树文件系统）是一种现代的写时复制文件系统，提供数据完整性、快照和对高级存储管理技术（如 RAID 和子卷）的支持。它旨在成为一种通用文件系统，内置对固态驱动器（SSD）和硬盘驱动器（HDD）的支持。

+   ZFS：虽然不是由主流 Linux 内核原生支持，但 ZFS 是一种流行的高级文件系统，提供数据完整性、卷管理、快照和简单的管理等功能。它通常用于存储服务器，并以其稳健性和可伸缩性而闻名。

+   JFS：JFS（日志文件系统）是最初由 IBM 开发的高性能文件系统。它提供良好的可伸缩性，崩溃后快速恢复和高效的存储分配。JFS 今天使用较少，但仍然可用和受支持。

+   ReiserFS：ReiserFS 是一种使用 B+树结构进行高效文件存储的文件系统。它旨在优化小文件性能，并且在历史上曾用于一些特定用例。然而，近年来它的使用量有所下降。

这些只是 Linux 上可用的文件系统类型的几个例子。其他文件系统如 NILFS、F2FS 等也可以用于特定目的或专业环境中。

## RAID 和逻辑卷管理（LVM）

Linux RAID（独立磁盘冗余阵列）和 LVM（逻辑卷管理器）是两种可以在 Linux 系统中一起使用的分开的技术，以提供灵活和可靠的存储管理。

+   Linux RAID：

Linux RAID 允许您将多个物理磁盘组合成一个逻辑单元，以提高性能、数据冗余或两者兼而有之。在 Linux 中使用的最常见的 RAID 级别是 RAID 0、RAID 1、RAID 5 和 RAID 6，尽管还有其他级别。

+   RAID 0：这个级别提供了无冗余的条带化，这意味着数据分布在多个磁盘上以提高性能，但不提供数据保护。

+   RAID 1：提供镜像，数据在两个或更多磁盘上复制。这个级别提供数据冗余，但不提供性能改进。

+   RAID 5：使用分布式奇偶校验的条带化。数据分布在多个磁盘上，奇偶校验信息也分布在它们之间。它在性能和冗余之间提供了平衡。

+   RAID 6：类似于 RAID 5，但它使用双重奇偶校验，即使两个磁盘同时失败也能提供冗余。

Linux RAID 可以使用硬件 RAID 控制器或软件 RAID 进行配置。软件 RAID 通常更灵活，常用于 Linux 系统。mdadm 实用程序被广泛用于管理 Linux 中的软件 RAID 阵列。

+   LVM（逻辑卷管理器）：

LVM 是一种灵活的磁盘管理系统，允许您独立于底层物理磁盘管理存储卷。它提供逻辑卷、卷组和物理卷等功能。

+   物理卷（PV）：表示物理磁盘或分区，并初始化为 LVM 物理卷。

+   卷组（VG）：通过组合一个或多个物理卷创建。它充当一个磁盘空间池，从中创建逻辑卷。

+   逻辑卷（LV）：表示从卷组的空闲空间创建的灵活可调整大小的块设备。逻辑卷可以很容易地调整大小并在物理卷之间移动。

LVM 提供了诸如动态卷调整大小、快照和跨多个磁盘的能力等优势。它允许更灵活地管理存储，例如在不卸载文件系统的情况下动态调整卷的大小。

Linux RAID 和 LVM 可以通过使用 Linux RAID 创建 RAID 阵列，然后在 RAID 阵列上创建逻辑卷来一起使用。这种组合提供了管理逻辑卷的冗余和灵活性。

## 网络文件系统（NFS）和文件共享

Linux NFS（网络文件系统）是一种分布式文件系统协议，允许客户端计算机上的用户通过网络访问文件，就像它们存储在本地一样。NFS 通常用于在类 Unix 系统之间共享文件和目录。

以下是在 Linux 中设置 NFS 和文件共享涉及的基本步骤：

+   安装 NFS：首先在服务器和客户端机器上安装 NFS 软件包。软件包名称可能会根据您的 Linux 发行版而有所不同。例如，在 Ubuntu 上，您可以使用以下命令安装 NFS 服务器软件包：sudo apt-get install nfs-kernel-server，以及 NFS 客户端软件包：sudo apt-get install nfs-common。

+   配置 NFS 服务器：在服务器机器上，您需要定义要共享的目录并配置 NFS 服务器。配置文件通常位于/etc/exports。用文本编辑器打开文件，并添加条目，指定要共享的目录，以及权限和访问选项。例如，要将/shared 目录与特定客户端共享读写访问权限，可以添加类似这样的一行：/shared client_ip(rw)。在进行必要更改后保存文件。

+   导出目录：配置 NFS 服务器后，您需要导出目录以使其对客户端可用。使用以下命令导出/etc/exports 文件中指定的目录：sudo exportfs -a。此命令读取 exports 文件，并使指定的目录可供远程访问。

+   启动 NFS 服务器：在服务器机器上启用并启动 NFS 服务器。确切的方法因 Linux 发行版而异。在 Ubuntu 上，您可以使用以下命令：sudo systemctl enable nfs-kernel-server，然后是 sudo systemctl start nfs-kernel-server。

+   配置 NFS 客户端：在客户端机器上，您需要挂载 NFS 共享的目录。创建一个挂载点目录，用于访问共享文件。例如，sudo mkdir /mnt/nfs。

+   挂载 NFS 共享：使用 mount 命令在客户端机器上挂载 NFS 共享。命令语法如下：sudo mount server_ip:/shared /mnt/nfs，其中 server_ip 是 NFS 服务器的 IP 地址，/shared 是您要访问的目录。运行 mount 命令后，您应该能够访问/mnt/nfs 中的共享文件。

+   测试文件共享：通过在客户端机器上从 NFS 共享创建、修改或删除文件，验证文件共享是否正常工作。

这些是设置 NFS 并在 Linux 机器之间启用文件共享的基本步骤。请记住，根据您的特定要求和网络环境，可能需要考虑其他配置选项和安全注意事项。
<p align = "center" style="font-size: 34px;" > <strong>「Si je ne suis en la grâce de Dieu, qu'il m'y mette; et si j'y suis, qu'il m'y garde.」</strong> </p>

<p align = "right"> THE REAL GIRL'S POWER </p>

docker 本身更像是一个高级的脚本合集，用于管理 Linux Api 的封装实现，其调用层次如下：

```bash
Docker CLI → Docker Engine (dockerd) → containerd → runc → Linux Kernel APIs
```

podman 的层次如下：

```bash
CLI → libpod → runc/crun → Linux Kernel
```

podman 不如 docker 热门的原因，个人认为主要是以下几个方面：

| element  | Docker                                                | Podman    |
| -------- | ----------------------------------------------------- | --------- |
| 时间窗口 | 2013 年发布，遇上云技术发展，建立了庞大的生态、用户基础 | 2018 年发布 |
| 生态门槛 | docker hub、ci/cd 优先支持、大量教程文档              |           |
| 使用惯性 | 公司基于 docker 构建了 workflow，迁移成本高           |           |

但是我欣赏 podman 的设计哲学：

1. 无 daemon 设计，避免单点故障，符合 unix 美学
2. rootless 容器，基于用户，减少攻击面

另外，现在的 k8s 逐渐成为标准，pod 的概念很重要，podman 对 pod 的支持更加原生

仓库就是基于 podman 的设计路径，写一个自己的 podwoman，也是熟悉 Linux Api 的过程：`alias podwoman=docker`

## virtual machine layout

一个比较浅显的分层情况如下，并非完全严谨，但大体上没有太大问题（主要是因为一些产品内部同时包含多个层级，这里仅表示其最主流的大众认知情况）

```markdown
虚拟化
├── 用户态/
│   ├── 管理层/
│   │   ├── pve
│   │   ├── libvirt
│   │   ├── virsh
│   │   ├── virt-manager
│   │   ├── docker
│   │   └── k8s
│   ├── 监视、调度层（Virtual Machine Monitor / VMM）/
│   │   ├── qemu
│   │   ├── vmware workstation
│   │   └── virtualbox
│   └── 容器运行时/
│       ├── lxc
│       ├── containerd
│       ├── runc
│       └── crun
├── 内核态/
│   ├── linux kernel/
│   │   ├── KVM
│   │   ├── kvm.ko
│   │   ├── kvm_amd.ko
│   │   └── kvm_intel.ko
│   ├── windows kernel/
│   │   └── hyper-v hypervisor
│   ├── xen hypervisor
│   └── esxi vmkernel
└── 硬件层/
    ├── Intel VT-x
    └── AMD-V
```

## refer

1. [os-network-bridge](https://raw.githubusercontent.com/sparkuru/genshin/refs/heads/main/online/45-os-network-bridge.md)
2. 
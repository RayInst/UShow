# USHOW 3D UWB 定位与舞台追踪解算系统 (RTLS Host Engine)
<img width="2560" height="1033" alt="e17d499f481153979912759d132d7e48" src="https://github.com/user-attachments/assets/9dca03ea-ef84-4766-9c22-a43659723318" />

[中文说明](#chinese-version) | [English](#english-version)

---

<a name="chinese-version"></a>
# 【中文版本】USHOW 系统与硬件固件说明

### USHOW 工业级 3D UWB 实时定位与舞台追踪解算系统

  高精度 3D UWB 空间定位解算与可视化上位机系统，原生集成 PSN 协议，专为舞台灯光自动跟光、多媒体互动交互及工业移动机器人导航打造。

---

### ⚠️ 系统硬件搭配与出厂固件说明

    1. 软硬件搭配关系
        • 本软件为 USHOW 3D UWB 定位解算与可视化上位机系统。
        • 定位功能需搭配 UWB 定位硬件（基站 + 定位手环/标签）共同工作，由硬件完成物理脉冲测距，上位机负责实时几何三维解算与数据推流。

    2. 默认固件说明 (DS-TWR)
        • 出厂硬件默认烧录为【DS-TWR 双向双边测距固件】（舞台追光与工业级高精度最稳定方案）。
        • 默认开箱即用：只需将 A0 主基站通过 USB 插在电脑上，其余从基站独立通电即可，完全免去配置复杂路由器或局域网网段的烦恼。

    3. 双连接方式支持 (USB 直连 / ESP32 UDP 无线透传)
        • 方式一 (USB 串口直连)：A0 主基站直接通过 USB 数据线连接电脑，即插即用，零网络配置。
        • 方式二 (ESP32 Wi-Fi / UDP 无线透传)：基站可通过 ESP32 模块将测距数据经 Wi-Fi / 局域网 UDP 协议高速无线透传至上位机，实现全场完全免拉线、分布式独立算距，彻底摆脱 USB 线缆束缚，单标签刷新率可达 250Hz ~ 350Hz！

    4. 多固件模式自由切换
        • 若您的项目需要海量并发（如数十到数百人考勤盘点），可使用仓库中提供的【UTDOA】或【RTDOA】单片机源码固件进行烧录切换。
        • 上位机内置智能协议探测引擎，更换固件后会自动无缝识别并平滑切换定位模式，无需更换上位机软件。

---

### 一、 GitHub 托管资源与下载说明

    1. 上位机软件安装包 (Release 产物)
        • 安装版程序 (Setup.exe)：集成了完整的运行时与安装向导，双击秒级完成安装，自动建立桌面快捷方式。
        • 下载路径：请在仓库右侧点击 【Releases】 选项卡，下载最新的 Release 发布包。

    2. 硬件单片机固件源码与镜像 (Firmware)
        • DS-TWR 双向双边测距固件 (出厂默认)：
            - 文件夹路径：`Firmware/DS-TWR/`
            - 适配硬件：基站 (Anchor) 与 定位手环/标签 (Tag)。
            - 特点说明：出厂预烧录固件，10~20cm 工业级绝对精度，舞台跟光追光与高精度定位最稳首选。
        • UTDOA 免布线上行时差固件：
            - 文件夹路径：`Firmware/UTDOA/`
            - 特点说明：适合数十到数百人的大容量考勤盘点，手环极低功耗待机 1~2 年，从基站免网线。
        • RTDOA 下行广播高刷时差固件：
            - 文件夹路径：`Firmware/RTDOA/`
            - 特点说明：适合仓储物流高动态叉车与 AGV 追踪，单标签刷新率达 100Hz~400Hz。

---

### 二、 三大定位方案核心特性与选型规划

#### 1. 方案一：DS-TWR (双边双向飞行时间测距) —— 出厂默认固件 / 舞台演艺追光首选 (最稳定)
    • 定位机理：标签与各基站依次执行 POLL-RESP-FINAL 双向对称往返握手，代数消除晶振温漂与频偏。
    • 测距精度：10 ~ 20 cm (工业级硬件绝对物理极限)。
    • 通信与连接：支持 USB 串口直连 或 ESP32 Wi-Fi/UDP 无线透传。
    • 核心优势：
        - 抗多径与遮挡极强：双向对称机制消除多径相位扰动，复杂演艺现场不跳点。
        - 外围精度衰减平缓：基于球面距离交汇，即使演员走至基站包围圈外侧，误差依然平稳受控。
        - 零单点故障风险：各基站本地独立解算距离并通过 ESP32/UDP 透传，无全网失步风险。
    • 物理时隙与刷新率：
        - 4 基站单槽时间 4.0ms，单标签最高刷新率达 ~250 Hz (ESP32 分布式透传可达 350 Hz)。
        - 4 标签并发下每标签刷新率可达 50.0Hz ~ 62.5Hz；单区典型承载 10 ~ 30 个并发标签。
    • 400mAh 手环续航：
        - 10Hz 连续工作约 4 天 (90 小时)；结合加速度计动态休眠综合待机 15 ~ 30 天 (官方标称 5 天)。
    • 最佳适用场景：
        - 舞台灯光自动化精准追光 (GrandMA2/MA3 控台联动)。
        - 工业移动机器人 (AGV/AMR) 高精度导航与防撞。
        - 室内无人机高动态空间动捕编队飞行。
        - TouchDesigner / UE5 沉浸式多媒体互动展项。

#### 2. 方案二：UTDOA (上行集中到达时间差) —— 智慧工厂人员与资产定位首选
    • 定位机理：手环单向广播微秒级短脉冲 (Blink)，从基站无线同步提取时间戳并汇总至 A0 主基站统一输出。
    • 测距精度：15 ~ 30 cm。
    • 核心优势：
        - 现场极简免布线：仅 A0 主基站需连接电脑 (USB 或 UDP)，从基站独立通电即可，无需网线或时钟同步线。
        - 手环极致超低功耗：标签仅单向发射无接收开销，大幅延长电池寿命。
        - 大容量并发：单时隙仅 400µs (0.40ms)，轻松承载数百标签。
    • 物理时隙与容量规划 (6.8Mbps 场景 0)：
        - 4 基站单标签最高刷新率 ~180 Hz。
        - 60Hz 推荐刷新率下支持 26 个并发标签；10Hz 工业标准下支持 235 个并发标签。
    • 400mAh 手环续航：
        - 10Hz 纯连续工作长达 ~50 天 (1.7 个月，均流仅 0.33mA)；结合静止休眠综合续航长达 6 ~ 12 个月 (甚至 1 ~ 2 年以上)。
    • 最佳适用场景：
        - 智慧工厂车间、施工工地人员手环安全考勤与物资定位。
        - 医院、养老院、监所防走失与安全管控；仓储大批量托盘盘点。

#### 3. 方案三：RTDOA (下行广播到达时间差) —— 仓储物流叉车与高动态多目标大容量
    • 定位机理：主基站 A0 统一广播 Beacon 调度超帧，从基站微时隙广播，硬件 CFO 实时载波频偏补偿。
    • 测距精度：15 ~ 30 cm (CFO 补偿后平稳)。
    • 物理时隙与容量规划：
        - 单时隙宽度 500µs (0.50ms)，4 基站单标签最高刷新率达 ~400 Hz。
        - 10Hz 工业标准下支持 193 个并发标签。
    • 最佳适用场景：
        - 大型智慧物流仓储叉车与 AGV 快速行进轨迹追踪。
        - 大范围室内密集多目标高动态监控。

---

### 三、 舞台演艺与多媒体互动核心优势

    1. GrandMA2 / GrandMA3 控台 3 步极速直连 (PSN v2.02)
        • 控台侧只需进入 Setup -> Network -> PSN Network Configuration 开启接收，并在 View Tracker 列表中为 UWB 手环分配 Fixture ID (Stage Marker 虚拟灯具)，电脑灯光束即刻精准跟随走动。
        • 支持标准组播 (236.10.10.10:56565) 与 IP 单播直推，有效规避舞台复杂无线环境下的广播风暴。

    2. 多坐标系一键矩阵变换与 1:1 空间对齐
        • 内置 GrandMA2/MA3 专属预设 (自动交换 Y/Z 轴并反转 Y 轴，匹配左手系)。
        • 内置 TouchDesigner / Notch / Resolume 标准右手系预设 (X=水平, Y=高度, Z=纵深)。
        • 内置 Unreal Engine 5 虚幻引擎预设 (自动厘米级 Scale=100 变换)。

    3. 灵活拖拽与交互优化
        • 侧边栏支持鼠标按住右边缘在 280px ~ 650px 自由拖动拉伸，并支持一键隐藏与悬浮展开。

---

### 四、 系统技术指标与授权说明

    • 空间定位精度：10 ~ 20 cm (DS-TWR 工业级物理极限) / 15 ~ 30 cm (TDoA)
    • 最高解算帧率：单标签最高 400 Hz / 系统综合吞吐量 1000+ 帧/秒
    • 数据推流协议：PosiStageNet (PSN v2.02)、UDP (Raw/JSON/CSV)、TCP Client/Server
    • 物理通信接口：USB 虚拟串口 (最高支持 921600 波特率)、以太网 / Wi-Fi UDP (ESP32 无线透传)
    • 操作系统支持：Windows 10 / Windows 11 (64位系统)
    • 软件运行机制：支持离线脱网运行，提供离线授权激活与网络一键绑定/解绑迁移功能。

---
---

<a name="english-version"></a>
# 【English Version】USHOW System & Hardware Firmware Guide

[Back to Chinese / 返回中文说明](#chinese-version)

### USHOW Industrial 3D UWB Real-Time Localization & Stage Tracking Engine

  High-Precision 3D UWB RTLS & Spatial Tracking Software with Native PSN Protocol for Stage Lighting, Multimedia Interactivity & Industrial Robotics.

---

### ⚠️ System Hardware & Firmware Overview

    1. Hardware Dependency
        • This repository hosts the USHOW 3D UWB RTLS host software engine.
        • Physical ranging is executed by compatible UWB hardware modules (Anchors + Tags/Wristbands), while the host software handles real-time 3D spatial solving and protocol streaming.

    2. Default Firmware (DS-TWR)
        • Hardware comes pre-flashed with 【DS-TWR (Two-Way Ranging) Firmware】 (the most stable and accurate scheme for stage follow-spots and industrial automation).
        • Plug-and-Play Operation: Connect Master Anchor A0 via USB and power on slave anchors. No router setup or network configuration required.

    3. Dual Connectivity (USB Direct / ESP32 UDP Wireless Pass-Through)
        • Method 1 (USB Serial Direct): Connect Master Anchor A0 directly to your PC via USB cable. Plug-and-play with zero network setup.
        • Method 2 (ESP32 Wi-Fi / UDP Wireless Pass-Through): Anchors transmit distance data wirelessly via ESP32 Wi-Fi / UDP to the host PC. Completely wire-free with distributed local solving, pushing single-tag refresh rates up to 250Hz ~ 350Hz!

    4. Multi-Firmware Switching Support
        • If your scenario requires massive tag concurrency (e.g., hundreds of personnel/asset tracking), you can flash the 【UTDOA】 or 【RTDOA】 MCU source code provided in the repository.
        • The host software features intelligent protocol auto-detection, seamlessly adapting to TWR or TDoA data streams automatically.

---

### I. GitHub Hosted Resources & Download Guide

    1. Host Software Packages (Releases)
        • Setup Installer (USHOW_setup.exe): Full installation package with standard setup wizard and desktop shortcuts.
        • Download Location: Click the 【Releases】 tab on the right sidebar of the GitHub repository to get the latest builds.

    2. MCU Firmware Source Code & Binary Images
        • DS-TWR Symmetrical Two-Way Ranging Firmware (Factory Default):
            - Folder Path: `Firmware/DS-TWR/`
            - Compatible Hardware: Anchors and Location Tags / Wristbands.
            - Description: Pre-flashed factory firmware with 10–20 cm absolute accuracy. Best choice for stage follow-spots and industrial automation.
        • UTDOA Uplink Convergence TDoA Firmware:
            - Folder Path: `Firmware/UTDOA/`
            - Description: Designed for massive tag capacity (hundreds of active tags) and 1–2 years tag battery standby. Zero anchor network cabling required.
        • RTDOA Downlink Broadcast TDoA Firmware:
            - Folder Path: `Firmware/RTDOA/`
            - Description: Engineered for high-speed AGVs and warehouse forklift tracking with 100Hz–400Hz refresh rates.

---

### II. Three Core Positioning Schemes & Capacity Planning

#### 1. Scheme 1: DS-TWR (Two-Way Ranging) —— Default Firmware / Stage Lighting Pick (Most Stable)
    • Positioning Physics: Symmetrical POLL-RESP-FINAL round-trip handshake algebraically eliminating crystal temperature drift.
    • Accuracy: 10 ~ 20 cm (Industrial hardware physical limit).
    • Connection: USB Serial Direct or ESP32 Wi-Fi/UDP Wireless Pass-Through.
    • Key Advantages:
        - Superior Multipath & NLOS Immunity: Bidirectional handshake eliminates phase perturbations; no jumping coordinates in live venues.
        - Smooth Outer Perimeter: Spherical intersection bounds errors even when performers move outside the anchor perimeter.
        - Zero Single-Point-of-Failure: Anchors compute distances locally and stream via ESP32/UDP—zero full-network desync risk.
    • Physical Slot & Refresh Rate:
        - 4-Anchor slot width is 4.0ms, single-tag max refresh rate is ~250 Hz (up to 350 Hz via ESP32 UDP pass-through).
        - Under 4 concurrent tags, each tag achieves 50.0Hz ~ 62.5Hz; typical capacity supports 10 ~ 30 concurrent tags.
    • 400mAh Tag Battery Life:
        - 10Hz continuous operation lasts ~4 days (90 hours); dynamic sleep standby lasts 15 ~ 30 days (5-day nominal).
    • Best-Fit Applications:
        - Automated stage follow-spot tracking with GrandMA2 / GrandMA3 via PSN.
        - Industrial mobile robot (AGV/AMR) high-precision navigation.
        - Indoor UAV high-dynamic motion capture swarms.
        - TouchDesigner / Unreal Engine 5 interactive installations.

#### 2. Scheme 2: UTDOA (Uplink Time Difference of Arrival) —— Industrial Personnel & Assets Pick
    • Positioning Physics: Tag transmits micro Blink pulses; slave anchors wirelessly aggregate timestamps to master A0 for unified output.
    • Accuracy: 15 ~ 30 cm.
    • Key Advantages:
        - Zero-Wiring Field Setup: Only master A0 connects to the PC (USB or UDP); slave anchors are independently powered with zero network cables.
        - Ultra-Low Tag Power Consumption: One-way transmission with zero RX overhead significantly extends battery longevity.
        - Massive Tag Capacity: 400µs (0.40ms) slot width supports 26 tags at 60Hz and 235 tags at 10Hz.
    • 400mAh Tag Battery Life:
        - 10Hz continuous operation lasts ~50 days (1.7 months); motion sleep extends standby to 6 ~ 12 months (1~2+ years).
    • Best-Fit Applications:
        - Smart factory and construction worker safety attendance.
        - Healthcare and security geofencing; warehouse inventory management.

#### 3. Scheme 3: RTDOA (Downlink Broadcast TDoA) —— High-Speed Forklifts & Dense Multi-Target
    • Positioning Physics: Master A0 broadcasts Beacon; slave anchors reply in micro-slots; hardware CFO compensates clock drift.
    • Accuracy: 15 ~ 30 cm (stable after CFO compensation).
    • Physical Slot & Refresh Rate:
        - Slot width 500µs (0.50ms); single-tag max refresh rate reaches ~400 Hz; supports 193 tags at 10Hz.
    • Best-Fit Applications:
        - Large-scale indoor dense multi-target continuous monitoring.

---

### III. Stage Lighting & Multimedia Capabilities

    1. GrandMA2 / GrandMA3 3-Step Setup (PSN v2.02)
        • On the console, enable Setup -> Network -> PSN Network Configuration, and assign UWB tags to Stage Markers in View Tracker. Spotlights track performers with zero stutter.
        • Supports standard Multicast (236.10.10.10:56565) and targeted Unicast to eliminate Wi-Fi broadcast congestion.

    2. Flexible 3D Matrix Transformations & 1:1 Spatial Alignment
        • Built-in GrandMA2/MA3 preset (Auto YZ-swap & Y-invert for left-hand stage coordinates).
        • Built-in TouchDesigner / Notch standard right-hand preset (X=Width, Y=Height, Z=Depth).
        • Built-in Unreal Engine 5 preset (Auto centimeter-scale Scale=100 transformation).

    3. Draggable & Collapsible UI
        • Drag sidebar right edge to freely resize width between 280px and 650px; click toggle button to collapse or expand instantly.

---

### IV. Technical Specifications & Operating Mode

    • Spatial Accuracy: 10 ~ 20 cm (DS-TWR Industrial Physical Limit) / 15 ~ 30 cm (TDoA)
    • Maximum Refresh Rate: Up to 400 Hz (Single Tag) / 1000+ position solves/sec (System Throughput)
    • Output Protocols: PosiStageNet (PSN v2.02), UDP (Raw/JSON/CSV), TCP Client/Server
    • Physical Interfaces: USB Virtual COM (up to 921600 Baud), Ethernet / Wi-Fi UDP (ESP32 Wireless Pass-Through)
    • Operating Systems: Windows 10 / Windows 11 (64-bit)
    • Licensing & Deployment: Operates 100% offline without internet dependency; offers online/offline activation and machine migration.

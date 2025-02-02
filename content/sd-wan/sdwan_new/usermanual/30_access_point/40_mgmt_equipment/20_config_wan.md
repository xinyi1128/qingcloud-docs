---
title: "配置 WAN"
draft: false
collapsible: false
weight: 21

---

当有多个运营商线路时，可以设置 SD-WAN 的 WAN 口并根据优先级来让数据走其中的一条，其它线路做为备份。我们将这种策略叫做 MWAN。

SD-WAN 的 WAN 口设置优先级，可决定业务流量优先使用的 WAN 口。优先级数字越小，WAN 口使用优先级越高。当两个 WAN 口的优先级数字相同，则根据 WAN 口名称决定优先级，WAN 口名称字幕越靠前优先，字母越小月优先。例如：优先级数字相同的 eth0 和 eth1则优先级 eth0 大于 eth1。

本章节介绍如何配置 WAN。

## 前提条件

- 已获取管理控制台的账号和密码。
- 已创建连接云网。
- 已创建接入点。

## 操作步骤

### WAN 配置

1. 登录 QingCloud 管理控制台。

2. 选择**产品与服务** > **SD-WAN（新版）** > **SD-WAN（新版）**，进入**连接云网**页面。

   <img src="../../../../_images/qs_cloud_network.png" style="zoom:50%;" />

3. 在左侧导航栏中，点击**接入点**，进入**接入点**页面。

   <img src="../../../../_images/qs_light_access.png" style="zoom:50%;" />

4. 点击已创建的接入点的名称，进入接入点详细信息页面。

5. 选择**设备管理** > **WAN 配置**，进入 **WAN 配置**页面。

   显示 **HA 配置**和 **WAN 配置**两个区域。

   <img src="../../../../_images/config_wan_01.png" style="zoom:100%;" />

6. 在 **WAN 配置**区域，将鼠标悬停于待修改WAN 口的卡片上，显示编辑图标<img src="../../../../_images/wan_icon.png" style="zoom:100%;" />。

   <img src="../../../../_images/config_wan_02.png" style="zoom:100%;" />

7. 点击编辑图标<img src="../../../../_images/wan_icon.png" style="zoom:100%;" />，显示端口修改界面。

   >仅支持修改**网卡状态**为`在线`的 WAN 口信息。

8. 修改 WAN 配置，如下表所示。

   | 参数     | 参数说明                                                     |
   | -------- | ------------------------------------------------------------ |
   | 连接类型 | 可选择：动态 IP、静态 IP、PPPoE                              |
   | IP地址   | 设备的 IP 地址，<b>连接类型</b>为<code>静态IP</code>时，需填写此项。 |
   | 网关     | 设备的网关，<b>连接类型</b>为<code>静态IP</code>时，需填写此项。 |
   | 账号     | 设备的账号，<b>连接类型</b>为<code>PPPoE</code>时，需填写此项。 |
   | 密码     | 设备的密码，<b>连接类型</b>为<code>PPPoE</code>时，需填写此项。 |
   | 优先级   | 端口按优先级排序，值越小优先级越高。<br>优先级可输入范围：1-255 |
   | 跟踪IP   | 跟踪IP用于检查该端口网络是否畅通， 若输入多个IP以英文逗号分隔。<br>最多可填写 5 个 IP。 |
   | MTU      | 最大传输单元，用于通知对方所能接受数据服务单元的最大尺寸，说明发送方能够接受的有效载荷大小。<br> 默认值为1500。 |

9. 修改完成后，点击**确定修改**，完成 WAN 配置修改操作。

   若页面弹出**编辑成功**提示信息，则说明修改 WAN 配置信息成功。

10. 点击**应用修改**，使配置生效。

## 相关操作

### 自定义业务组

当用户需要业务流量流入指定端口，需要自定义业务组。例如：POP 流量走 eth0 端口，互联网流量走 eth1 端口，大数据业务流量走 eth2 端口，视频流量走 eth3 端口，这时需要新增 2 个业务组，一个命名为大数据业务走大数据业务流量，一个命名为视频业务走视频业务流量。

**POP流量组**、**互联网流量组**、**内网流量组**为默认业务组，不支持修改和删除。

* **POP 流量组**：默认开启 NAT 网关和 SD-WAN 隧道，该组内端口可连通内网云端虚拟机，也可允许互联网流量流通。
* **互联网流量组**：默认只开启 NAT 网关，该组内端口只能流通互联网流量，不能与 POP 建立隧道。
* **内网流量组**：默认关闭 NAT 网关和 SD-WAN 隧道，内网流量组内端口只允许流通 VCPE 连接到云端虚拟机的内网流量。

>**说明**
>
>* 当互联网流量可同时使用POP 流量组的端口和互联网流量组的端口时，**POP 流量组**优先级高于**互联网流量组**。端口分流高于 MWAN。
>* 若需要使用指定 WAN 口跑特定业务流量，请将 WAN 口移动到对应的业务组。
>* 自定义业务组和自定义业务组的端口支持修改和删除，自定义业务组和默认业务组的 WAN 口可进行任意移动。
>* 网卡在 **POP 流量组**才可启用 MWAN 功能。

1. 在**WAN 配置**页签右上角点击**自定义业务组**。

   ![自定义业务组](../../../../_images/create_service_group_01.png)

2. 在**自定义业务组**界面，点击**新建业务组**。

   ![新建业务组](../../../../_images/create_service_group_02.png)

3. 在**新增业务组界**面，根据提示信息填写参数。

4. 点击**新建**，完成自定义业务组。

5. 在端口**操作**一栏点击**移动**可将默认业务组的端口移动至自定义业务组中。

### 自定义端口角色

>**说明**
>
>* 将角色设置为 WAN 口时，默认绑定**内网流量组**。
>* 当端口角色发生变化时，控制器需要重下端口组并配置。

1. 在**WAN 配置**页签右上角点击**自定义端口角色**。

   ![自定义端口角色](../../../../_images/create_port_group_01.png)

2. 进入**端口角色配置**页面，更多操作请参见[端口角色配置](/sd-wan/sdwan_new/usermanual/30_access_point/40_mgmt_equipment/50_config_port/)。
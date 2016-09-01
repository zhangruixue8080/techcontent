<properties
 pageTitle="面向 IT 专业人员的 Azure IoT 中心信息 | Azure"
 description="帮助 IT 专业人员使用 Azure IoT 中心的信息，例如端口要求和安全背景信息。"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.date="08/09/2016"
 wacn.date="08/29/2016"/>

# 配置和管理对 IoT 中心的访问权限

本文章提供的信息可帮助 IT 专业人员配置一个环境，使 IoT 设备可通过网络基础结构与 IoT 中心通信。

## 网络连接

设备可以在 Azure 中使用各种协议来与 IoT 中心通信。通常，选择的协议根据解决方案的具体要求而定。下表列出了必须打开的、使设备能够使用特定协议的出站端口：

| 协议 | 端口 |
| -------- | ------- |
| HTTPS | 443 |
| AMQP | 5671 |
| 基于 WebSockets 的 AMQP | 443 |
| MQTT | 8883 |
| LWM2M（设备管理） | 5684 |

在 Azure 区域中创建 IoT 中心后，该中心将在其生存期内保留同一个 IP 地址。但是，为了维持服务质量，如果 Microsoft 将 IoT 中心转移到不同的缩放单位，则将为中心分配新 IP 地址。

## IoT 中心和安全性

只有已注册到 IoT 中心的设备才能与该 IoT 中心通信。已注册的设备必须获得 *DeviceConnect* 权限。设备通过包含令牌（该令牌在设备发出的每个请求中封装设备唯一 ID）来标识自身，中心将检查令牌的有效性，以及设备是否未列入方块列表（*DeviceConnect* 权限已吊销）。有关 IoT 中心支持的令牌的信息，请参阅[使用 IoT 中心安全令牌和 X.509 证书][lnk-tokens]

对 IoT 中心内的其他管理终结点的访问权限也是通过一组权限进行控制的：*iothubowner*、*service*、*registryRead* 和 *registryReadWrite*。连接到 IoT 中心的所有客户端管理应用程序都必须包含具有相应权限的令牌。

## 后续步骤

本文包含的具体信息面向配置开发和测试环境的 IT 专业人员和开发人员。[Azure IoT 中心开发人员指南中的“安全性”部分][lnk-devguide]提供了有关 IoT 中心内令牌和权限系统的更多信息。

若要进一步探索 IoT 中心的功能，请参阅：

- [设计你的解决方案][lnk-design]
- [开发人员指南][lnk-devguide]
- [使用 UI 示例探索设备管理][lnk-dmui]
- [使用网关 SDK 模拟设备][lnk-gateway]

[lnk-devguide]: /documentation/articles/iot-hub-devguide/#security

[lnk-design]: /documentation/articles/iot-hub-guidance/
[lnk-devguide]: /documentation/articles/iot-hub-devguide/
[lnk-dmui]: /documentation/articles/iot-hub-device-management-ui-sample/
[lnk-gateway]: /documentation/articles/iot-hub-linux-gateway-sdk-simulated-device/
[lnk-tokens]: /documentation/articles/iot-hub-sas-tokens/

<!---HONumber=Mooncake_0307_2016-->
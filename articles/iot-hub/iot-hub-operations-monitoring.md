<properties
 pageTitle="IoT 中心操作监视"
 description="概述 IoT 中心操作监视如何让你实时监视 IoT 中心上的操作状态。"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.date="08/11/2016"
 wacn.date="08/29/2016"/>


# 操作监视简介

IoT 中心操作监视可让你实时监视其 IoT 中心上的操作状态。IoT 中心跟踪多个操作类别的事件，你可以选择将一个或多个类别的事件发送到其 IoT 中心终结点进行处理。你可以监视数据中是否有错误，或根据数据模式设置更复杂的处理行为。

IoT 中心监视 5 种类别的事件：

- 设备标识操作
- 设备遥测
- 云到设备的命令
- 连接
- 文件上传

## 如何启用操作监视

1. 创建 IoT 中心。有关如何创建 IoT 中心的说明，请参阅[入门][lnk-get-started]指南。

2. 打开 IoT 中心的边栏选项卡。在此处单击“操作监视”。

    ![][1]

3. 选择要监视的监视类别，然后单击“保存”。可以从“监视设置”中所列的与事件中心兼容的终结点读取事件。IoT 中心终结点称为 `messages/operationsmonitoringevents`。

    ![][2]

## 事件类别及其用法

每种操作监视类别跟踪与 IoT 中心之间进行的不同类型的交互，每一种监视类别都有一个架构用于定义如何构建该类别的事件。

### 设备标识操作

设备标识操作类别跟踪你尝试在其 IoT 中心的标识注册表中创建、更新或删除条目时所发生的错误。预配方案就很适合跟踪此类别。

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### 设备遥测

设备遥测类别跟踪在 IoT 中心发生的、与遥测管道相关的错误。此类别包括发送遥测事件（例如限制）和接收遥测事件（例如未经授权的读取者）时发生的错误。请注意，此类别无法捕捉设备本身运行的代码所造成的错误。

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{"scope":"device","type":"sas","issuer":"iothub"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### 云到设备的命令

云到设备的命令类别跟踪在 IoT 中心发生的、与设备命令管道相关的错误。此类别包括发送命令（例如未经授权的发送者）、接收命令（例如超过传递计数）和接收命令反馈（例如反馈已过期）时所发生的错误。此类别不捕捉未正确处理命令但却将其成功传递的设备所发生的错误。

    {
         "messageSizeInBytes": 1234,
         "authType": "{"scope":"hub","type":"sas","issuer":"iothub"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### 连接

连接类别跟踪设备与 IoT 中心连接或断开连接时发生的错误。若要识别未经授权的连接尝试，以及在连接质量不佳的区域中的设备断开连接时进行跟踪，就很适合跟踪此类别。

    {
         "durationMs": 1234,
         "authType": "{"scope":"hub","type":"sas","issuer":"iothub"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### 文件上载

文件上载类别跟踪在 IoT 中心发生的、与文件上载功能相关的错误。此类别包括下述情况发生的错误：SAS URI（例如，当它在设备向中心通知某上载已完毕之前到期时）、设备报告的失败上载，以及创建 IoT 中心通知消息期间未在存储空间中找到文件时。请注意，此类别不能捕获在设备将文件上载到存储空间时直接发生的错误。

    {
         "authType": "{"scope":"hub","type":"sas","issuer":"iothub"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## 后续步骤

现在你已大概了解了操作监视，请参阅[管理 IoT 中心的访问权限][lnk-itpro]，了解有关管理 IoT 中心的其他信息。

若要进一步探索 IoT 中心的功能，请参阅：

- [设计你的解决方案][lnk-design]
- [开发人员指南][lnk-devguide]
- [使用 UI 示例探索设备管理][lnk-dmui]
- [使用网关 SDK 模拟设备][lnk-gateway]

<!-- Links and images -->

[1]: ./media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: ./media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: /documentation/articles/iot-hub-csharp-csharp-getstarted/
[lnk-diagnostic-metrics]: /documentation/articles/iot-hub-metrics/
[lnk-scaling]: /documentation/articles/iot-hub-scaling/
[lnk-dr]: /documentation/articles/iot-hub-ha-dr/

[lnk-itpro]: /documentation/articles/iot-hub-itpro-info/

[lnk-design]: /documentation/articles/iot-hub-guidance/
[lnk-devguide]: /documentation/articles/iot-hub-devguide/
[lnk-dmui]: /documentation/articles/iot-hub-device-management-ui-sample/
[lnk-gateway]: /documentation/articles/iot-hub-linux-gateway-sdk-simulated-device/

<!---HONumber=Mooncake_0725_2016-->
#### Note: This repository is inherited from the [old Tuya Github repository](https://github.com/TuyaInc/tuyasmart_socket_ios_sdk) that will be deprecated soon. Please use this repository for Tuya SDK development instead. For changing the existing remote repository URL, see this [tutorial](https://docs.github.com/en/github/using-git/changing-a-remotes-url)

## Tuya Socket iOS SDK

[中文版](README_cn.md) | [English](README.md)

> `TuyaSmartSocketKit` is mainly for iOS developers concerning Tuya cloud-to-cloud connection products. This project is launched to provide LAN connection between iPhones (hereinafter referred to as a phone) and hardware devices (hereinafter referred to as a device)and send dpCode in LAN to control communication.

The Tuya device connection and control process is as follows: https://cdn.nlark.com/yuque/__puml/1de1d74497bdbb14a4debde42f3f3f34.svg

## Procedure
`TuyaSmartSocketKit` uses `TuyaSmartSocket` as an access.

#### 1. LAN initialization

You can connect to the LAN with the following methods:
```
[[TuyaSmartSocket sharedInstance] startSocketService];
```

You can monitor the LAN changes with the following methods:
```
[TuyaSmartSocket sharedInstance].delegate = self;
```

#### 2. Device connection
The device informs that the LAN will be connected through TuyaSmartSocoket in the following below. This parameter can be obtained through the API of cloud-to-cloud connection `/v1.0/devices/schema`.
```
/**
 *  set device info
 Configure basic device information *
 *  @param devInfo   devInfo
 */
 - (void)addDeviceInfoWIthDevInfo:(NSDictionary *)devInfo;
```

#### 3. Device communication
In the delegate callback, the device can be controlled after a successful connection.
```
- (void)socketDidTcpConnected:(TuyaSmartSocket *)socket devId:(NSString *)devId;
```

Control the device by sending dpCode to the device in the following method.
```
/**
 *  publish message in lan use standard data point
 *
 Send LAN message with standard dpCode *
 *  @param devId    devId
 *  @param dpCodeDict   dpCode : dpValue
 *  @param success  Success block
 *  @param failure  Failure block
 */
 - (void)sendRequestWithDevId:(NSString *)devId
                 dpcodeValue:(NSDictionary<NSString */* dpCode */,id/* dpValue */> *)dpCodeDict
                     success:(void(^)(void))success
                     failure:(void(^)(void))failure;
```

`dpCodeValue` Composition rule： For detailed information about the DP, see [Update device information](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/en/resource/Device.html#device-management)

`dpCode` describes the DPs of the device, namely, what control functions does a device support. The DPs supported by the device will be returned in the **schema** that is returned in the " /v1.0/devices/schema" API. The sections below describe the typical DPs respectively. 
`dpCodeDict` is performed in the format of `dpCode` : `dpValue` . `The dpCode` can be obtained from the code field in **schema**. `dpValue` needs to be sent in the format that the DP supports. The section below describes the composition of dpCode, using the Demo in the API documentation.
##### 1. Switch
    "type": "bool"
    e.g. {@"switch_led" : YES} or {@"switch_led" : YES}
```
{
    "mode": "rw",
    "code": "switch_led",
    "name": "Switch ",
    "property": {
        "type": "bool"
    },
    "iconname": "icon- dp_power2",
    "id": 20,
    "type": "obj",
    "desc": ""
 }
```
##### 2. Mode selection (single option)

    "type": "enum"
    e.g. dpCode : {@"work_mode" : @"white"} {@"work_mode" : @"colour"}
```
{
    "mode": "rw",
    "code": "work_mode",
    "name": "mode",
    "property": {
        "range": ["white", "colour", "scene", "music"],
        "type": "enum"
    },
    "iconname": "i con-dp_list",
    "id": 21,
    "type": "obj",
    "desc": ""
 }
```

##### 3. Brightness value (Send value)
    "type": "value"
    e.g. dpCode: {@"bright_value": @(400)}
    Note: The values are limited by maximum, minimum and step values.
```
{
    "mode": "rw",
    "code": "bright_value",
    "name": "Brightness value",
    "property": {
        "min": 10,
        "max": 1000,
        "scale": 0,
        "step": 1,
        "type": "value"
    },
    "iconname ": "icon-dp_sun",
    "id": 22,
    "type": "obj",
    "desc": ""
 }
```
##### 4. Colored light (send string)
    "type": "string"
    e.g. dpCode: {@"colour_data":@"000100010001"}
    The method above is just one of the transmission methods, and the specific deValue value needs to be analyzed according to the specific situation
```
{
    "mode": "rw",
    "code": "colour_data",
    "name": "Colored light",
    "property": {
        "type": "string",
        "maxlen": 255
    },
    "iconname": "icon- dp_light",
    "id": 24,
    "type": "obj",
    "desc": ""
 }
```
#### 4. Disconnect
```
[[TuyaSmartSocket sharedInstance] closeSocketService];
 [TuyaSmartSocket sharedInstance].delegate = nil;
```
## Support

You can get support through the following accesses:

* [Tuya Smart Help Center](https://support.tuya.com/en/help)

* [Technical Support Council](https://iot.tuya.com/council/) 

## License

The Tuya Home iOS SDK Sample is licensed under the MIT License.

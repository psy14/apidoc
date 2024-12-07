# 系统api文档

## 接口列表
- [收款单](#收款单)
- [下发单](#下发单)


###### 公共请求参数

| 参数      | 类型     | 是否必填 | 说明              |
|---------|--------|-----|-----------------|
| appId   | String | Y   | 商户APPID         |
| sign    | String | Y   | [签名方法](#签名方法)   |
| t       | long   | Y   | 当前时间戳(毫秒)       |
| service | String | Y   | 服务名             |
| reqBody | json   | Y   | 业务请求参数,根据具体请求传参 |

###### 公共响应参数

| 参数   | 类型     | 是否必填   | 说明      |
|------|--------|--------|---------|
| code | int    | Y      | [返回码](#返回码) |
| msg  | String | Y      | 错误原因    |
| data | json | string | N       | 业务返回数据      |


###  收款单

#### 服务名
```
OrderRec
```

#### 请求地址

| 请求路径 | /app-api/business/api/mch |
|--------------|---------------------------|
| 请求方式 | POST  application/json    |

#### 业务请求参数说明
| 参数        | 类型      | 是否必填 | 说明                         |
|-----------|---------|------|----------------------------|
| amount    | decimal | Y    | 金额,保留2位小数 单位元              |
| support   | number  | N    | [付款方式](#付款方式)              |
| autoHedge | Boolean | N    | 默认不对冲,true=自动对冲,false=手动对冲 |

#### 业务响应参数说明
| 参数   | 类型     | 是否必填 | 说明           |
|------|--------|------|--------------|
| data | String | Y    | 平台订单号 |

#### 示例
##### 请求
```json
{
    "appId": "0d82923a349d4adfb9a5fb14ad6fc0b9",
    "sign": "57e65570c62ebf256632d8663cdc5485",
    "t": "112223333333",
    "service": "OrderSend",
    "reqBody": {
        "amount":"1110",
        "support": "2"
    }
}
```

##### 返回
```json
{
    "code": 0,
    "data": "1859064117885845504",
    "msg": ""
}
```


###  下发单

#### 服务名
```
OrderSend
```

#### 请求地址

| 请求路径 | /app-api/business/api/mch |
|--------------|---------------------------|
| 请求方式 | POST  application/json    |

#### 业务请求参数说明
| 参数        | 类型      | 是否必填 | 说明                         |
|-----------|---------|------|----------------------------|
| amount    | decimal | Y    | 金额,保留2位小数 单位元              |
| bankUsername | String  | Y    | 收款人姓名                      |
| bankCode | String  | Y    | [银行代码 ](#银行代码)             |
| bankNumber | String  | Y    | 收款卡号或者[二维码ID](#上传二维码)      |
| bankPhone | String  | N    | 收款人手机号                     |
| bankCertId | String  | N    | 收款人身份证号码                   |
| autoHedge | Boolean | N    | 默认不对冲,true=自动对冲,false=手动对冲 |

#### 业务响应参数说明
| 参数   | 类型     | 是否必填 | 说明           |
|------|--------|------|--------------|
| data | String | Y    | 平台订单号 |

#### 示例
##### 请求
```json
{
  "appId": "0d82923a349d4adfb9a5fb14ad6fc0b9",
  "sign": "57e65570c62ebf256632d8663cdc5485",
  "t": "112223333333",
  "service": "OrderSend",
  "reqBody": {
    "amount":"1110",
    "bankNumber": "12333222",
    "bankCode": "1041000",
    "bankUsername": "张三"
  }
}
```

##### 返回
```json
{
  "code": 0,
  "data": "1859079566212087808",
  "msg": ""
}
```

###  上传二维码

#### 服务名
```
Attach
```

#### 请求地址

| 请求路径 | /app-api/business/api/mch |
|--------------|---------------------------|
| 请求方式 | POST  application/json    |

#### 业务请求参数说明
| 参数        | 类型      | 是否必填 | 说明         |
|-----------|---------|------|------------|
| base64str | String  | Y    | 图片的base64码 |

#### 业务响应参数说明
| 参数   | 类型     | 是否必填 | 说明 |
|------|--------|------|--|
| data | String | Y    | 附件ID |

#### 示例
##### 请求
```json
{
  "appId": "0d82923a349d4adfb9a5fb14ad6fc0b9",
  "sign": "57e65570c62ebf256632d8663cdc5485",
  "t": "112223333333",
  "service": "Attach",
  "reqBody": {
    "base64str":"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"
  }
}
```

##### 返回
```json
{
  "code": 0,
  "data": "1111",
  "msg": ""
}
```



### 签名方法
使用业务人员提供的`appId` 和 `appSecret` 参与请求签名，将产生的sign添加在请求体中即可.
```
sign = md5((t*2%2+1000) + $appSecret)
```

#### 返回码
| 返回值 | 说明                    | 
|-----|-----------------------|
| 0   | 成功 （非0 需要查看msg 看具体问题） | 


#### 付款方式
| 值   | 说明  | 
|-----|-----|
| 0   | 微信  |
| 1   | 支付宝  |
| 2   | 银行卡  |


#### 银行代码
| 银行名称  | 银行代码    |
|:------|:--------|
| 农业银行  | 1031000 |
| 建设银行  | 1051000 |
| 工商银行  | 1021000 |
| 中国银行  | 1041000 |
| 交通银行  | 3012900 |
| 浦发银行  | 3102900 |
| 光大银行  | 3031000 |
| 华夏银行  | 3041000 |
| 上海银行  | 3132900 |
| 民生银行  | 3051000 |
| 中信银行  | 3021000 |
| 广发银行  | 3065810 |
| 渤海银行  | 3181100 |
| 平安银行  | 3071000 |
| 邮储银行  | 4031000 |
| 区域性银行 | S0018   |
| 浙商银行  | 3163310 |
| 微信 | 11 |
| 支付宝 | 12 |











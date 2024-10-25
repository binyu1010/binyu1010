# 更新日志

| 生效时间（新加坡时间 UTC+8) | 接口          | 新增/修改 | 摘要   |
|-------------------|-------------|-------|------|
| 2024.10.23 10:00  | `OPENAPI文档` | 新增    | 现货文档 |

# 简介

## API 简介

欢迎使用ARK开发者文档！


# 快速入门

## 请求交互

所有请求基于Https协议，请求头信息中Content-Type 需要统一设置为: 'application/json'。

**请求交互说明**

- 请求参数：根据接口请求参数规定进行参数封装。
- 提交请求参数：将封装好的请求参数通过GET/POST方式提交至服务器。
- 服务器响应：服务器首先对用户请求数据进行参数安全校验，通过校验后根据业务逻辑将响应数据以JSON格式返回给用户。
- 数据处理：对服务器响应数据进行处理。

**成功**

HTTP状态码0表示成功响应，并可能包含内容。如果响应含有内容，则将显示在相应的返回内容里面。

**请求格式**

所有的API请求都是restful，目前只有两种方法：GET和POST

* GET:  参数通过queryString在路径中传输到服务器。
* POST: 参数按照json格式发送body传输到服务器。

## 下单接口
**HTTP请求**

单个下单接口。
- POST /open/api/v2/create_order

| 参数名                | 参数类型   | 是否必须 |                                              描述                                               |
|:-------------------|:-------|:----:|:---------------------------------------------------------------------------------------------:|
| symbol             | STRING |  YES  |                                              币对                                               |
| volume            | LONG   |  YES  |      数量
| price  | STRING |  NO  |             价格 |
| side   | STRING |  YES  |                      BUY、SELL                          |
| type | INT   |  YES  |   1-limit,2-market,3-ioc,4-fok,5-post_only      |
> 返回数据:
```json
{
    "code": "0",
    "msg": "suc",
    "data": {
        "order_id": [
            "2415460796738740169"
        ]
    }
}
```

单个撤单接口。 
- POST /open/api/v2/cancel_order

| 参数名                | 参数类型   | 是否必须 |                                              描述                                               |
|:-------------------|:-------|:----:|:---------------------------------------------------------------------------------------------:|
| symbol             | STRING |  YES  |                                              币对                                               |
| order_id |  Long  |  YES  |  | 订单id ｜

> 返回数据:
```json
{
    "code": "0",
    "msg": "suc"
}
```


撤销某个币对所有订单接口。 
- POST /open/api/v2/cancel_order_all

| 参数名                | 参数类型   | 是否必须 |                                              描述                                               |
|:-------------------|:-------|:----:|:---------------------------------------------------------------------------------------------:|
| symbol             | STRING |  YES  |                                              币对                                               |

> 返回数据:
```json
{
    "code": "0",
    "msg": "suc"
}
```

批量下单、撤单接口。 TODO
- POST /open/api/v2/mass_replace

| 参数名                | 参数类型   | 是否必须 |                                              描述                                               |
|:-------------------|:-------|:----:|:---------------------------------------------------------------------------------------------:|
| symbol             | STRING |  YES  |                                              币对                                               |
| massPlace            | LIST<MassPlaceOrder>	   |  NO  |      	批量下单订单的列表
| massCancel | LIST<Long>	   |  NO  |  批量撤单订单的ID列表。 |

#### `massPlace` 参数中的 `MassPlaceOrder` 说明：

| 参数名   | 参数类型 | 是否必须 | 描述                                                        |
|---------|----------|:--------:|------------------------------------------------------------|
| volume  | STRING   |   YES    | 订单的数量，表示买卖的数量。                                    |
| side    | STRING   |   YES    | 买卖方向，支持 `BUY` 和 `SELL`。                               |
| type    | STRING   |   YES    | 订单类型，如 `LIMIT` 或 `MARKET` 等订单类型。                   |
| price   | STRING   |   NO     | 订单价格，仅适用于限价单。市价单不需要填写此参数。                  |


> 返回数据:
```json
{
    "code": "0",
    "data": {
        "massPlace": [
            2415460796738740272,
            2415460796738740273,
            2415460796738740274,
            2415460796738740275,
            2415460796738740276,
            2415460796738740277,
            2415460796738740278,
            2415460796738740279,
            2415460796738740280,
            2415460796738740281
        ],
        "massCancel": []
    }
}
```

获取当前委托订单列表接口
- POST /open/api/v2/all_order

| 参数名                | 参数类型   | 是否必须 |                                              描述                                               |
|:-------------------|:-------|:----:|:---------------------------------------------------------------------------------------------:|
| symbol             | STRING |  YES  |                                              币对                                               |
| pageSize   | INT |  YES  |                                               |
| page | INT   |  YES  |  |

> 返回数据:

```json
{
    "code": "0",
    "msg": "suc",
    "data": {
        "count": 0,
        "resultList": [
            {
                "side": "BUY",
                "totalPrice": 11.3900000000,
                "fee": 0E-10,
                "createdAt": 1729682445000,
                "dealPrice": 0,
                "avgPrice": 0E-10,
                "source": 5,
                "type": 1,
                "volume": 0.0100000000,
                "price": 1139.0000000000,
                "sourceMsg": "ROBOT",
                "dealVolume": 0E-10,
                "feeCoin": "BTC",
                "id": 2415460796738740160,
                "remainVolume": 0.0100000000,
                "status": 0
            },
            {
                "side": "BUY",
                "fee": 0E-10,
                "createdAt": 1729682708000,
                "dealPrice": 0,
                "avgPrice": 0E-10,
                "source": 5,
                "type": 5,
                "volume": 0.0100000000,
                "price": 0E-10,
                "sourceMsg": "ROBOT",
                "dealVolume": 0E-10,
                "feeCoin": "BTC",
                "id": 2415460796738740169,
                "status": 0
            }
        ]
    }
}
```


获取币对列表接口 
- GET /open/api/common/symbols
                                  
> 返回数据:
```json
{
    "code": "0",
    "msg": "suc",
    "data": [
        {
            "base_coin": "BTC",
            "count_coin": "USDT",
            "symbol": "btcusdt",
            "price_precision": "6",
            "amount_precision": "4",
            "limit_volume_min": "0.001"
        },
        {
            "base_coin": "ADA",
            "count_coin": "USDT",
            "symbol": "adausdt",
            "price_precision": "5",
            "amount_precision": "1",
            "limit_volume_min": "5"
        },
        {
            "base_coin": "ETH",
            "count_coin": "USDT",
            "symbol": "ethusdt",
            "price_precision": "6",
            "amount_precision": "3",
            "limit_volume_min": "0.001"
        }
    ]
}
```



自成交接口，刷行情使用
- POST /open/api/common/symbols

| 参数名                | 参数类型   | 是否必须 |                                              描述                                               |
|:-------------------|:-------|:----:|:---------------------------------------------------------------------------------------------:|
| symbol             | STRING |  YES  |                                              币对                                               |
| volume            | LONG   |  YES  |      数量
| price  | STRING |  NO  |             价格 |
| side   | STRING |  YES  |                      BUY、SELL                          |
| type | INT   |  YES  | 1 |

> 返回数据:

```json
{
    "code": "0",
    "msg": "suc",
    "data": {
        "buy": 0,
        "sell": 0,
        "order_id": 0
    }
}
```


获取用户账户信息接口
- GET /open/api/user/account

> 返回数据:

```json
{
    "code": "0",
    "msg": "suc",
    "data": {
        "totalAsset": "541.07",
        "coinList": [
            {
                "coin": "avax",
                "normal": "0.00",
                "locked": "0.00"
            },
            {
                "coin": "usdc",
                "normal": "0.00",
                "locked": "0.00"
            }
        ]
    }
}
```

## 行情接口

获取行情深度接口
- GET /market/v1/depth
  
| 参数名                | 参数类型   | 是否必须 |                                              描述                                               |
|:-------------------|:-------|:----:|:---------------------------------------------------------------------------------------------:|
| symbol             | STRING |  YES  |                                              币对                                               |
| limit            | INT   |  YES  |      默认 150; 最大 N. 可选值:[5, 10, 20, 50, 150]


> 返回数据:

```json
{
  "msg": "操作成功",
  "code": "0",
  "data": {
    "buys": [
      [61149.9, 0.01323],
      [61149.6, 0.07314],
      [61149.1, 0.06582],
      [61148.8, 0.04005],
      [61148.4, 0.09557],
      [61147.8, 0.06778],
      [61147.5, 0.11394],
      [61147.2, 0.06732],
      [61147.1, 0.09913],
      [61146.7, 0.11853],
      [61146.5, 0.0647],
      [61146.4, 0.04161],
      [61146.3, 0.05961],
      [61146, 0.03016],
      [61145.5, 0.05035],
      [61145.2, 0.07038],
      [61144.7, 0.02251],
      [61144.4, 0.02238],
      [61143.9, 0.09235],
      [61143.6, 0.03735],
      [61143.3, 0.03978],
      [61143, 0.07763],
      [61142.4, 0.08816],
      [61141.9, 0.05044],
      [61141.3, 0.10851],
      [61141.2, 0.05425],
      [61140.8, 0.07319],
      [61140.2, 0.09591],
      [61139.7, 0.10244],
      [61139.2, 0.10503]
    ],
    "asks": [
      [61150.3, 0.02732],
      [61150.5, 0.01764],
      [61150.6, 0.06639],
      [61151.1, 0.02645],
      [61151.6, 0.06596],
      [61151.8, 0.05027],
      [61152.4, 0.02423],
      [61152.9, 0.03615],
      [61153.2, 0.0893],
      [61153.4, 0.09643],
      [61154, 0.09298],
      [61154.3, 0.04741],
      [61154.5, 0.03277],
      [61154.9, 0.09162],
      [61155.3, 0.03158],
      [61155.4, 0.08935],
      [61155.9, 0.01047],
      [61156.1, 0.09975],
      [61156.6, 0.05376],
      [61157.1, 0.05182],
      [61157.5, 0.09821],
      [61157.9, 0.08625],
      [61158, 0.11642],
      [61158.3, 0.03407],
      [61158.6, 0.08666],
      [61158.9, 0.06832],
      [61159.5, 0.10696],
      [61159.7, 0.06652],
      [61160.3, 0.02917],
      [61160.8, 0.04516]
    ],
    "lastUpdateId": 1728968156784
  }
}
```

获取24小时行情
- GET /market/v1/ticker

| 参数名                | 参数类型   | 是否必须 |                                              描述                                               |
|:-------------------|:-------|:----:|:---------------------------------------------------------------------------------------------:|
| symbol             | STRING |  YES  |                                              币对                                               |


> 返回数据:

```json
{
"msg": "操作成功",
"code": "0",
"data": {
"amount": 14617380.212827036,
"vol": 3099850.617284143,
"open": null,
"close": 62942.9,
"high": 63228.9,
"low": 60000.0,
"rose": 0.00791356,
"es_price": 62942.9
}
```

获取最新成交价格
- GET /market/v1/trade
  
| 参数名                | 参数类型   | 是否必须 |                                              描述                                               |
|:-------------------|:-------|:----:|:---------------------------------------------------------------------------------------------:|
| symbol             | STRING |  YES  |                                              币对                                               |


> 返回数据:

```json
{
"msg": "操作成功",
"code": "0",
"data": 63008.5
}
```


## 签名
- 参数排序:
使用 TreeMap 对传入的 params 进行按键的自然顺序排序，保证每次生成签名时参数顺序一致。

- 拼接参数:
遍历排序后的参数，将键和值依次拼接成一个长字符串。跳过键名为 sign 的参数，同时跳过值为 null 或空字符串的参数。

- 拼接密钥:
在拼接完所有参数后，将 secretKey 追加到拼接后的字符串的末尾。

- 生成 MD5 签名:
使用 getMD5 方法生成最终的 MD5 哈希签名，作为请求的签名返回。
以下是一个 Python 版本的签名生成函数

- 时间同步安全：
time参数计算时间差并判断过期：计算当前时间与请求时间的差异，如果时间差小于设定的过期时间（30秒）则校验失败。


```python
{
import hashlib
from collections import OrderedDict

def generate_sign(params, secret_key):
    # 排序参数
    sorted_params = OrderedDict(sorted(params.items()))
    
    # 拼接参数
    basestring = ""
    for key, value in sorted_params.items():
        if key != 'sign' and value is not None and value != "":
            basestring += f"{key}{value}"
    
    # 拼接密钥
    basestring += secret_key
    
    # 使用MD5生成签名
    print(f"basestring: {basestring}")
    return hashlib.md5(basestring.encode('utf-8')).hexdigest()

# 示例用法
params = {
    "symbol": "BTC-USDT",
    "volume": "100",
    "price": "40000",
    "api_key": "my_api_key",
    "timestamp": "1627888987000"
}
secret_key = "my_secret_key"

# 生成签名
sign = generate_sign(params, secret_key)
print(f"签名: {sign}")

}



***
# 智能相册API接口
***

## 场景识别

描述：判断一张或多张照片所属的场景

调用URL: /api/imgclassify/scene

调用方法: POST

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|二选一| img_uri| string | Query |  传入单张照片URI/URL时使用|
|二选一| payload| json | json | 批量传入照片时使用；payload的格式见下面说明|
|传HEIC文件时必选，其余情形忽略| format| string | Query | 当识别HEIC文件时必须设置成为字符串'heic'（不区分大小写） |

payload是一个json字符串，有传URL和Base64 encoded两种的格式。

格式一: Payload内容为 URL
```
{"images": [{"url": "http://abc.com/image1.jpg"}, {"url": "http://bbc.com/image2"}]}
```
说明：当 ``` format ```参数指定为HEIC时，只能使用格式一(传递URL)

格式二: Payload内容为BASE64编码的字符串
```
{"images": [{"url": "data:image/jpeg;base64,/9j/***省略***=="}, {"url": "data:image/png;base64,/9j/***省略***=="}]}
```
其中 ```data:image/jpeg;base64,``` 为前缀，```/9j/```开头的部分BASE64编码的字符串

返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| results | string array | 请求成功时才会返回改字段；一个字符串数组，显示了图片检测的结果（unicode编码）；数组元素的次序和请求中的次序一致 |
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |

- 返回值示例：

```
返回正确：
{
  "results": [    "\u5ba4\u5916\u5efa\u7b51",    "\u5ba4\u5185\u5efa\u7b51"  ]
}
返回错误：{ "error": "Invalid payload input." }
```
- ERROR状态和信息说明：

|HTTP 状态代码| 错误信息 | 说明
|:------------| :------------|:-------------|
| 400 | Invalid input heic file(s) or failed in format converting | 当检测HEIC时，传入的HEIC格式时，输入文件格式有误或后端转换出错 |
| 400 | Invalid payload input. | payload 格式错误 |
| 405 | Invalid HTTP Request Method | 只能用POST方法 |


curl 使用例子说明：
```
HOST="yourhostname:port"
curl -X POST "http://$HOST/api/imgclassify/scene?img_uri=http://i3.bbswater.fd.zol-img.com.cn/t_s1200x5000/g5/M00/01/0E/ChMkJ1ZNu6uIGENLAA-gXhD2jzcAAFG1gNG2GgAD6B2489.jpg"

echo "or POST request in payload format"
curl -X POST  -H "Content-Type: application/json" -d '{"images": [{"url": "https://raw.githubusercontent.com/SnowSense/archive/master/ios2.jpg"}, {"url": "http://i3.bbswater.fd.zol-img.com.cn/t_s1200x5000/g5/M00/01/0E/ChMkJ1ZNu6uIGENLAA-gXhD2jzcAAFG1gNG2GgAD6B2489.jpg"}]}' "http://$HOST/api/imgclassify/scene"

```

## HECI格式转换

描述：将一个 ```img_uri``` 指定的HEIC文件转换成JPG文件并返回

调用URL: /api/img/heic2jpg

调用方法: POST

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|必选| img_uri| string | Query | 带转换文件的URI/URL |

返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
|  |文件 |  mimetype='image/jpg' |
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |

- 返回值示例：

```
返回正确：一个JPG文件
返回错误：{'error': 'Missing img_uri param in POST Request'} or {"error": "Invalid HTTP Request Method"}
```

- ERROR状态和信息说明：

|HTTP 状态代码| 错误信息 | 说明
|:------------| :------------|:-------------|
| 400 | Missing img_uri param in POST Request | 缺少必选参数 img_uri |
| 405 | Invalid HTTP Request Method | 请求的方法错误，应为POST |

curl 使用例子说明：

```
HOST="yourhostname:port"
curl -o test.jpg -X POST "http://$HOST/api/img/heic2jpg?img_uri=https://raw.githubusercontent.com/SnowSense/archive/master/test/ios.heic"
```

## EXIF信息提取

描述：获取一个或多个图片的EXIF信息 （不支持 HEIC图片）

调用URL: /api/img/getexif

调用方法: POST

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|二选一| img_uri| string | Query |  传入单张照片URI/URL时使用；|
|二选一| payload| json | json | 批量传入照片时使用；payload的格式见下面说明|

payload是一个json字符串，有传URL和Base64 encoded两种的格式。

格式一: Payload内容为 URL
```
{"images": [{"url": "http://abc.com/image1.jpg"}, {"url": "http://bbc.com/image2"}]}
```

格式二: Payload内容为BASE64编码的字符串
```
{"images": [{"url": "data:image/jpeg;base64,/9j/***省略***=="}, {"url": "data:image/png;base64,/9j/***省略***=="}]}
```
其中 ```data:image/jpeg;base64,``` 为前缀，```/9j/```开头的部分BASE64编码的字符串

返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| results | string array | 请求成功时才会返回改字段；一个字符串数组，显示了图片检测的结果；数组元素的次序和请求中的次序一致 |
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |


返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| exif | 数组 |   |
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |

EXIF信息简要说明如下，详细信息请参考 EXIF工业标准(JEITA CP-3451B, Exchangeable image file format for digital still cameras: Exif Version 2.3 [S]. JEITA & CIPA, 2010.). [Exif2-2.PDF](http://www.exif.org/Exif2-2.PDF) 或 [wikipedia](https://en.wikipedia.org/wiki/Geotagging)。

|字段|  类型| 说明
|:------------| :------------|:-------------|
| DateTime | string | 拍摄日期和时间  |
| GPSInfo | string |  GPS原始信息 |
| GPSLatitude | float | 从GPS原始信息中转换成浮点数的经纬度坐标 |
| GPSLongitude |float |从GPS原始信息中转换成浮点数的经纬度坐标 |
| Make | string |相机制造商|
| Model | string |相机型号 |
| Orientation | int |照片的方向，横竖 |
| ResolutionUnit | int | 清晰度|
| SceneCaptureType | int |TBA|

- 返回值示例：

```
返回正确：
{
 "exif": [
 {  "DateTime": "2017:11:02 14:55:06",
    "GPSInfo": { "1": "N", "2": [ [ 30, 1 ], [ 14, 1 ], [ 2201, 100 ] ], "3": "E", "4": [ [ 120, 1 ], [ 5, 1 ], [ 3533, 100 ] ],
    "5": "\u0000", "6": [ 36967, 552 ], "7": [ [ 6, 1 ], [ 55, 1 ], [ 600, 100 ] ],
    "12": "K", "13": [ 0, 1 ], "16": "T", "17": [ 8986, 65 ], "23": "T", "24": [ 8986, 65 ], "29": "2017:11:02", "31": [ 5, 1 ]
      },
      "GPSLatitude": 30.239447222222225,
      "GPSLongitude": 120.09314722222221,
      "Make": "Apple",
      "Model": "iPhone 6s Plus",
      "Orientation": 1,
      "ResolutionUnit": 2,
      "SceneCaptureType": 0
    },
    {
      "DateTime": "2015:11:19 19:03:36",
      "GPSInfo": null,
      "GPSLatitude": null,
      "GPSLongitude": null,
      "Make": "Canon",
      "Model": "Canon EOS 5D Mark III",
      "Orientation": 1,
      "ResolutionUnit": 2,
      "SceneCaptureType": 0
    }
  ]
}

返回错误：{'error': 'Missing img_uri param in POST Request'} or {"error": "Invalid HTTP Request Method"}
```

- ERROR状态和信息说明：

|HTTP 状态代码| 错误信息 | 说明
|:------------| :------------|:-------------|
| 405 | Invalid HTTP Request Method | 请求的方法错误，应为POST |

curl 使用例子说明：

```
HOST="yourhostname:port"
curl -X POST "http://$HOST/api/img/getexif?img_uri=https://raw.githubusercontent.com/SnowSense/archive/master/ios2.jpg"

echo "or POST a payload"
curl -X POST  -H "Content-Type: application/json" -d '{"images": [{"url": "https://raw.githubusercontent.com/SnowSense/archive/master/ios2.jpg"}, {"url": "http://i3.bbswater.fd.zol-img.com.cn/t_s1200x5000/g5/M00/01/0E/ChMkJ1ZNu6uIGENLAA-gXhD2jzcAAFG1gNG2GgAD6B2489.jpg"}]}' "http://$HOST/api/img/getexif"
```

## 清空服务端的临时文件夹

描述：清空服务端的临时文件夹（不要和其他API同时使用）

调用URL: /api/resetcache

调用方法: POST

请求参数：无

返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| Success | string | 仅当请求成功时才会返回此字符串；内容为 True |
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |


curl 使用例子说明：

```
HOST="yourhostname:port"
curl -X POST "http://$HOST/api/resetcache"
```

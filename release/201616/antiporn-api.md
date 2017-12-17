***
# 鉴黄服务API接口
***

## 图片审核

描述：检查一张或多张照片，是否有色情内容

调用URL: /api/dbg/imgclassify/antiporn

调用方法: POST

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|二选一| img_uri| string | Query |  传入单张照片URI/URL时使用；|
|二选一| payload| json | json | 批量传入照片时使用；payload的格式见下面说明|
|传HEIC文件时必选，其余情形忽略| format| string | Query | 当识别HEIC文件时必须设置成为字符串'heic'（不区分大小写） |

payload是一个json字符串，有传URL和Base64 encoded两种的格式。

格式一: Payload内容为 URL
```
{"images": [{"url": "http://abc.com/image1.jpg"}, {"url": "http://bbc.com/image2"}]}
```
说明：当 ``` format ```参数指定为HEIC时，只能使用格式一，传递URL

格式二: Payload内容为BASE64编码的字符串
```
{"images": [{"url": "data:image/jpeg;base64,/9j/***省略***=="}, {"url": "data:image/png;base64,/9j/***省略***=="}]}
```
其中 ```data:image/jpeg;base64,``` 为前缀，```/9j/```开头的部分BASE64编码的字符串

返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| results | map<string,array> | 请求成功时才会返回该字段；一个scores数组，显示了图片检测的分数，一个tags数组，表示图片的检测结果；数组元素的次序和请求中的次序一致 |
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |

- 返回值示例：

```
返回正确：
{"results":{"scores"=[0.0034683231730014206, 0.5220495405484687], "tags"=["normal", "normal"]}}
返回错误：
{ "error": "Invalid payload input." }
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

curl -X POST  -H "Content-Type: application/json" -d '{"images": [{"url": "http://i3.bbswater.fd.zol-img.com.cn/t_s1200x5000/g5/M00/01/0E/ChMkJ1ZNu6uIGENLAA-gXhD2jzcAAFG1gNG2GgAD6B2489.jpg"}]}' "http://$HOST/api/dbg/imgclassify/antiporn"
```

## 视频审核

描述：检查一个视频是否含有色情内容

调用URL: /api/videoclassify/antiporn

调用方法: POST

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|二选一| uri| string | Query | 一个视频文件的URI/URL链接 |
|二选一| video| FileObject | MultiParts | 一个视频文件的对象 |
|必选| format| string | Query | 视频文件的格式,如 "mp4", "mpeg" |


返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| result |string | "normal" or "porn" |
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |

- 返回值示例：

```
返回正确：{"result" :"normal"} 或者 {"result" :"porn"}
返回错误：{"error": "Invalid HTTP Request Method"}
```

- ERROR状态和信息说明：

|HTTP 状态代码| 错误信息 | 说明
|:------------| :------------|:-------------|
| 400 | MISSING_ARGUMENTS | 缺少必选参数 apikey |
| 401 | AUTHENTICATION_ERROR | api_key 和 api_secret 不匹配 |

curl 使用例子说明：

```
HOST="yourhostname:port"
curl -X POST -F "video=@/path/to/your/test.mp4" "http://$HOST/api/videoclassify/antiporn?format=mp4"
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

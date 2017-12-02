# 人脸识别服务API接口说明


***
## API接口
***

## Tag 相关接口


#### 关联人脸ID和Tag

描述：将一个face ID 于一个或多个tag相关联

调用URL: /api/face/```<face_id>```/tags

调用方法: POST

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|必选| apikey | string | Query | 调用此 API 的 API Key |
|必选| secretkey| string | Query | 调用此 API 的 API Secret |
|必选| tags_str| string | Query |一组tag的名称，用逗号分隔，如 'tag1, tag2, tag3'; 自动忽略收尾的空格 |
|必选| face_id| string | Path | 目标人脸的 Face ID |


返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| success | string | 请求成功时才会返回改字段；返回值总为 'True'。否则此字段不存在。 |
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |

- 返回值示例：

```
返回正确：
{ "success": true }
返回错误：
{ "error": "INVALID_FACE_ID:faceId face_5076ee94-d19b-11e7-b208-0242ac140002 not found." }
```
- ERROR状态和信息说明：

|HTTP 状态代码| 错误信息 | 说明
|:------------| :------------|:-------------|
| 400 | MISSING_ARGUMENTS | 缺少必选参数 apikey |
| 401 | AUTHENTICATION_ERROR | api_key 和 api_secret 不匹配 |
| 400 | MISSING_TAGS_STR:comma separated tags | 缺少必选参数 tags_str或 tags_str为空字符串|
| 400 | INVALID_FACE_ID | Face ID不存在 |

curl 使用例子说明：
```
HOST="your-host-url"
KEY="your-key"
SECRET="your-secret"
curl -X POST "http://$HOST/api/face/face_7b43f9ca-d194-11e7-94f5-0242ac140003/tags?apikey=$KEY&secretkey=$SECRET&tags_str=tom,group1"
```

#### 查询人脸ID对应的Tag

描述：查询人脸ID对应的Tag

调用URL: /api/face/```<face_id>```/tags

调用方法: GET

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|必选| apikey | string | Query | 调用此 API 的 API Key |
|必选| secretkey| string | Query | 调用此 API 的 API Secret |
|必选| face_id| string | Path | 目标人脸的 Face ID |


返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| tags | 字符串数组 | 返回字符串 |
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |

- 返回值示例：

```
返回正确：
{
  "faceId" :"face_5076ee94-d19b-11e7-b208-0242ac140002",
  "tags": [      "tom",       "group1"    ]
}

或者
{
  "faceId" :"face_5076ee94-d19b-11e7-b208-0242ac140002",
  "tags": null
}

返回错误：
{
  "error": "INVALID_FACE_ID:faceId face_5076ee94-d19b-11e7-b208-0242ac140002 not found."
}
```

- ERROR状态和信息说明：

|HTTP 状态代码| 错误信息 | 说明
|:------------| :------------|:-------------|
| 400 | MISSING_ARGUMENTS | 缺少必选参数 apikey |
| 401 | AUTHENTICATION_ERROR | api_key 和 api_secret 不匹配 |

curl 使用例子说明：

```
HOST="your-host-url"
KEY="your-key"
SECRET="your-secret"
curl -X GET "http://$HOST/api/face/face_7b43f9ca-d194-11e7-94f5-0242ac140003/tags?apikey=$KEY&secretkey=$SECRET"
```

#### 查询一个Tag中所有相关的Face ID

描述：查询一个Tag中所有相关的Face ID

调用URL: /api/faces

调用方法: GET

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|必选| apikey | string | Query | 调用此 API 的 API Key |
|必选| secretkey| string | Query | 调用此 API 的 API Secret |
|必选| tag | string | Query | 查询tag的名称 |

返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |


- 返回值示例：

```
正确返回：

{'faces':
  [
    {
      "faceId": "face_f314099e-d19a-11e7-b208-0242ac140002",
      "tags": ["tom"]
    },
    {
      "faceId": "face_5076ee94-d19b-11e7-b208-0242ac140002",
      "tags": [      "tom",      "group1"    ]
    },
    {
      "faceId": "face_5076f088-d19b-11e7-b208-0242ac140002",
      "tags": ["tom"]
    }
  ]
}

错误返回： {"error": "MISSING_TAG"}
```

- ERROR状态和信息说明：

|HTTP 状态代码| 错误信息 | 说明
|:------------| :------------|:-------------|
| 400 | MISSING_ARGUMENTS | 缺少必选参数 apikey |
| 401 | AUTHENTICATION_ERROR | api_key 和 api_secret 不匹配 |
| 400 | MISSING_TAG | 缺少必选参数 tag |


curl 使用例子说明：
```
HOST="your-host-url"
KEY="your-key"
SECRET="your-secret"
echo "in simple model"
curl -X GET "http://$HOST/api/faces?apikey=$KEY&secretkey=$SECRET&tag=tom"
```


## 功能相关接口

#### Group Faces

描述：在无人脸底库的情况下，自动为一组人脸分类

调用URL: /api/group_faces

调用方法: POST

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|必选| apikey | string | Query | 调用此 API 的 API Key |
|必选| secretkey| string | Query | 调用此 API 的 API Secret |
|可选| model | string | Query |  'simple' or 'advance'; 如不指定 ```model```参数，系统默值 'advance'|
|可选| thres | float | Query |  仅在 ```model```为'simple'下有效; 系统默认值 0.8 |


返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |

- 返回值示例：
```
返回正确：
{ 0: ["face_f314099e-d19a-11e7-b208-0242ac140002"],
  1: ["face_5076ee94-d19b-11e7-b208-0242ac140002"],
  1: ["face_5076f088-d19b-11e7-b208-0242ac140002"]}

返回错误：
{  "error": "FAIL_GET_FACE_VECTORS" }
```

- ERROR状态和信息说明：

|HTTP 状态代码| 错误信息 | 说明
|:------------| :------------|:-------------|
| 400 | MISSING_ARGUMENTS | 缺少必选参数 apikey |
| 401 | AUTHENTICATION_ERROR | api_key 和 api_secret 不匹配 |
| 402 | FAIL_GET_FACE_VECTORS | Tag中的 Face ID对应的内部参数为空 |

curl 使用例子说明：
```
HOST="your-host-url"
KEY="your-key"
SECRET="your-secret"
echo "in simple model"
curl -X POST "http://$HOST/api/group_faces?apikey=$KEY&secretkey=$SECRET&tag=group1&model=simple"
echo "in default model (advanced model)"
curl -X POST "http://$HOST/api/group_faces?apikey=$KEY&secretkey=$SECRET&tag=group1"
```

#### Group Faces (Phicomm Version)

描述：在无人脸底库的情况下，自动为一组人脸分类；并为每个一组创建一个对应的Person ID, 将对应的Face ID与 Person ID相关联

调用URL: /api/phi/group_faces

调用方法: POST

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|必选| apikey | string | Query | 调用此 API 的 API Key |
|必选| secretkey| string | Query | 调用此 API 的 API Secret |
|可选| model | string | Query |  'simple' or 'advance'; 如不指定 ```model```参数，系统默值 'advance'|
|可选| thres | float | Query |  仅在 ```model```为'simple'下有效; 系统默认值 0.8 |

返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |

- 返回值示例：
```
返回正确：
{
  "person_c2c81f40-d19b-11e7-b208-0242ac140002": [
    "face_5076ee94-d19b-11e7-b208-0242ac140002"],
  "person_c38724e4-d19b-11e7-b208-0242ac140002": [
    "face_f314099e-d19a-11e7-b208-0242ac140002",
    "face_5076f088-d19b-11e7-b208-0242ac140002"]
}
返回错误：
{
  "error": "FAIL_GET_FACE_VECTORS"
}
```

- ERROR状态和信息说明：

|HTTP 状态代码| 错误信息 | 说明
|:------------| :------------|:-------------|
| 400 | MISSING_ARGUMENTS | 缺少必选参数 apikey |
| 401 | AUTHENTICATION_ERROR | api_key 和 api_secret 不匹配 |
| 402 | FAIL_GET_FACE_VECTORS | Tag中的 Face ID对应的内部参数为空 |

curl 使用例子说明：

```
HOST="your-host-url"
KEY="your-key"
SECRET="your-secret"
curl -X POST "http://$HOST/phi/api/group_faces?apikey=$KEY&secretkey=$SECRET&tag=group1"
```


#### Detect Person

描述：从一张照片中，根据给定的阀值检测人脸并根据给定的相似度阀值，自动判断人脸所属的人物；如果找不到相似的人，则创建一个新的 Person ID, 并将检测出的Face ID与之相关联; 如果找到了相似的人物，则将此Person ID 与检测出的Face ID相关联。

调用URL: /api/detectperson

调用方法: POST

请求参数：

|是否必选| 参数名 | 类型| 传递方式 | 说明
|:------:|:------------| :------------|:------:|:-------------|
|必选| apikey | string | Query | 调用此 API 的 API Key |
|必选| secretkey| string | Query | 调用此 API 的 API Secret |
|可选| im_uri | string | Query |  待检测图片的链接；```im_uri``` 和 ```image```参数必须二选一|
|可选| image | file | File | 待检测图片的文件对象；```im_uri``` 和 ```image```参数必须二选一|
|可选| faceness_thres | Query | float | 检测人脸时确认是人脸的最小阀值；本接口默值 0.4 |
|可选| similarity_thres | Query | float | 人脸识别时确认是一个人最小的相似度阀值；本接口默值系统默认值 0.77 |

返回值说明：

|字段|  类型| 说明
|:------------| :------------|:-------------|
| error | string | 当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。 |

- 返回值示例：
```
返回正确：
{'persons': [
    {'personId': u'person_30edd31e-ca6e-11e7-9d0b-0242ac120003', 'faceId': 'face_b6c56d44-d1a8-11e7-9536-0242ac120003', 'top': 415, 'right': 736, 'bottom': 638, 'left': 513}
    ]
}
```

- ERROR状态和信息说明：

|HTTP 状态代码| 错误信息 | 说明
|:------------| :------------|:-------------|
| 400 | MISSING_ARGUMENTS | 缺少必选参数 apikey |
| 401 | AUTHENTICATION_ERROR | api_key 和 api_secret 不匹配 |
| 400 | CAN_NOT_LOAD_IMAGE | 请求参数中给定的图片无法访问或无法解析 |

curl 使用例子说明：

```
HOST="your-host-url"
KEY="your-key"
SECRET="your-secret"
curl -X POST "http://$HOST/api/detectperson?apikey=$KEY&secretkey=$SECRET&im_uri=http://abc.com/1.jpg&faceness_thres=0.1&similarity_thres=0.8"
```

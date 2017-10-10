# 人脸识别 Java-SDK Reference

### 索引
* [API接口](#API接口)
* [主要的数据结构和成员变量](#主要的数据结构和成员变量)

***
## API接口
***

##### 人脸检测、对比和识别

|    | 接口| 描述| 参数|      返回值 |
|:--:|:------------| :------------|:-------------|:-----------|
|人脸检测| `DetectFaceResult detectFace(File file)` | 检测一张本地图片上所有的人脸 | `file` 待检测的图片文件 | `DetectFaceResult` 对象|
|人脸检测| `DetectFaceResult detectFace(String imageUrl)` | 检测一张网络图片上所有的人脸 | `imageUrl`: 待检测的图片URL字符串 | `DetectFaceResult` 对象|
|人脸对比| `CompareResult compareFaces(String faceId1, String faceId2)` | 对比2个人脸并得出相似度| `faceId1`, `faceId2`: 字符串，待检测的人脸标识 | `CompareResult` 对象|
|人脸识别 |`IdentifyResult identifyFace(String faceId)` |根据输入和人脸标识，从系统中找出最像的一个人物，并同时找出相似度稍低的一个或几个人物| `faceId`: 字符串，待检测的人脸标识 | `IdentifyResult` 对象|

##### 人物管理

|    | 接口 | 描述  | 参数 | 返回值 |
|:--:|:------------| :------------|:-------------|:-----------|
|新建人物|`String addPerson(String name)` | 向系统中新增一个人物实例 | `name` 人物的姓名 | `personId` 字符串，人物标识符 |
|关联人脸和人物|`boolean labelFace(String personId, String faceId)`| 将一个人脸关联到指定的人物 | `personId`: 字符串，人物标识符; `faceId`: 字符串，人脸实例的标识符 |boolean, True or False|
| 获取人脸标识 | `List<PersonLabel> getPersonLabels(String personId)`| 获取与人物相关联的所有人脸标识 | `personId`: 字符串，人物标识符| 一个列表，元素`PersonLabel`对象；每个`PersonLabel`对象包含：`personId`: 字符串，人物标识符; `faceId`: 字符串，人脸实例的标识符|
| 获取人脸参数 | `FaceResult getFaceResult(String faceId)` | 获取一个人脸标识的人脸参数| `faceId`: 字符串，人脸实例的标识符 | `FaceResult` 对象|


***
## 主要的数据结构和成员变量
***

##### personId 和 faceId
|  数据结构/变量  | 类型|说明       |
|:--:|:------------| :------------|
|personId| 字符串 | `人物`在系统数据库中的唯一标识; 由`addPerson` 方法生成并返回|
|faceId| 字符串 | `人脸` 的实例在系统数据库中的唯一标识; 由`detectFace` 方法生成|


##### FaceResult

|成员变量   | 类型 | 描述|
|:--|:------------| :------------|
|faceId     |   | 人脸实例的唯一标识标识     |
|faceness   |   | 区域是否是人脸的置信度     |
|imageHeight|   | 人脸区域的高度 |
|imageWidth |   | 人脸区域的宽度 |
|bottom     |   | 人脸区域底部的在输入图像中的位置 |
|right      |   | 人脸区域右侧的在输入图像中的位置 |
|top        |   | 人脸区域上沿的在输入图像中的位置 |
|left       |   | 人脸区域左侧的在输入图像中的位置 |
|sideness   |   | 人脸测斜的程度 |
|rightEyeHeight      |   | 右眼上沿下沿之间的高度     |
|leftEyeHeight       |   | 左眼上沿下沿之间的高度     |
|leftBoundaryToNose  |   | 面部左侧距离鼻子的距离     |
|topBoundaryToNose   |   | 面部上沿距离鼻子的距离     |
|bottomBoundaryToNose|   | 面部下沿距离鼻子的距离     |
|rightBoundaryToNose |   | 面部右侧距离鼻子的距离     |

说明
- (left, top) 左上角坐标        (right, top) 右上角坐标
- (left, bottom) 左下角角坐标   (right, bottom) 右下角角坐标

##### IdentifyResult

|成员变量   | 类型 | 描述|
|:--|:------------| :------------|
|identified  | | 是否在系统中识别出有此人            |
|personId    | | 被识别出人物的唯一标识（当identified为True）    |
|similarity  | | 被识别出的人物与输入人脸相似度（当identified为True）|
|name        | | 被识别出人物的名字（当identified为True）      |
|mostSimilarPersonId  | | 最相似人物的唯一标识（当identified为False）    |
|maxSimilarity        | | 最接近人物与输入人脸的相似度（当identified为False）|
|mostSimilarPersonName| | 最相似人物的名字（当identified为False）      |
|runnerUp    | | RunnerUp 没有被识别出，但比较相似的人物实例       |
|runnerUp->personId   | |       人物的唯一标识           |
|runnerUp->similarity | |       相似度               |
|runnerUp->name       | |       人物的名字             |


##### CompareResult

|成员变量   | 类型 | 描述|
|:--|:------------| :------------|
|similarity           |浮点型 |两个人脸的相似度|

# 智能相册 Java-SDK Reference

### 索引
* [API接口](#API接口)
* [主要的数据结构和成员变量](#主要的数据结构和成员变量)

***
## API接口
***

##### 场景识别

|    | 接口| 描述| 参数|      返回值 |
|:--:|:------------| :------------|:-------------|:-----------|
|场景识别| `ClassifySceneResult examineImageUrls(String... imageUrls)` | 判断多张网络照片所属的场景 | `imageUrls` 传入照片URI/URL | `ClassifySceneResult` 对象|
|场景识别| `ClassifySceneResult examineImageFiles(File... files)` | 判断多张本地照片所属的场景 | `files`: 传入照片文件 | `ClassifySceneResult` 对象|

##### HEIC格式转换（pending....)

|    | 接口 | 描述  | 参数 | 返回值 |
|:--:|:------------| :------------|:-------------|:-----------|
|HEIC格式转换|`XXXX convertHeicUrl(String heicUrl)` | 将一个 heicUrl 指定的HEIC文件转换成JPG文件并返回 | `heicUrl` 指定的HEIC文件的URL | `XXXX` |

##### EXIF信息提取

|    | 接口 | 描述  | 参数 | 返回值 |
|:--:|:------------| :------------|:-------------|:-----------|
|EXIF信息提取|`ExifResult getExifInfo(String ... imageUrls)` | 获取多个网络图片的EXIF信息 （不支持 HEIC图片） | `imageUrls` 传入照片URI/URL | `ExifResult` 对象 |
|EXIF信息提取|`ExifResult getExifInfo(File... files)`| 获取多个本地图片的EXIF信息 （不支持 HEIC图片） | `files`:传入照片文件 |`ExifResult`, 对象|

##### 清空服务器的临时文件夹

|    | 接口 | 描述  | 参数 | 返回值 |
|:--:|:------------| :------------|:-------------|:-----------|
|清空服务器的临时文件夹|`ResetCacheString examineResetCache()` | 清空服务器的临时文件夹 | 无 | `ResetCacheString` 字符串，清空服务器临时文件夹的结果 |

***
## 主要的数据结构和成员变量
***

##### ClassifySceneResult

|成员变量   | 类型 | 描述|
|:--|:------------| :------------|
|results    | List<String>  | 请求成功时才会返回该字段；一个字符串数组，显示了图片检测的结果（unicode编码）；数组元素的次序和请求中的次序一致     |
|error   | String  | 当请求失败时才会返回此字符串，否则此字段不存在。    |


##### ExifResult

|成员变量   | 类型 | 描述|
|:--|:------------| :------------|
|exif | ArrayList<Exif> | EXIF信息            |
|error    | String| 当请求失败时才会返回此字符串，否则此字段不存在。   |

##### Exif

|字段|  类型| 说明
|:------------| :------------|:-------------|
| DateTime | string | 拍摄日期和时间  |
| GPSInfo | map<string,object> |  GPS原始信息 |
| GPSLatitude | float | 从GPS原始信息中转换成浮点数的经纬度坐标 |
| GPSLongitude |float |从GPS原始信息中转换成浮点数的经纬度坐标 |
| Make | string |相机制造商|
| Model | string |相机型号 |
| Orientation | int |照片的方向，横竖 |
| ResolutionUnit | int | 清晰度|
| SceneCaptureType | int |TBA|


##### ResetCacheString

|成员变量   | 类型 | 描述|
|:--|:------------| :------------|
|Success     |String| 请求成功返回true    |
|error       |String| 当请求失败时才会返回  |

# 鉴黄服务 Java-SDK Reference

### 索引
* [API接口](#API接口)
* [主要的数据结构和成员变量](#主要的数据结构和成员变量)

***
## API接口
***

##### 图片审核、视频审核

|    | 接口| 描述| 参数|      返回值 |
|:--:|:------------| :------------|:-------------|:-----------|
|图片审核| `ClassifyResult examineImageUrls(String... imageUrls)` | 审核多张网络图片是否有色情内容 | `imageUrls` 待审核的图片URL字符串 | `ClassifyResult` 对象|
|图片审核| `ClassifyResult examineImageUrls(File... files)` | 审核多张本地图片是否有色情内容 | `files`: 待检测的图片文件 | `ClassifyResult` 对象|
|视频审核| `VideoClassifyResult examineVideoFile(File file, String format)` | 审核一个本地视频是否有色情内容| `file`:待检测的视频文件, `format`: 字符串，待检测的视频格式 | `VideoClassifyResult` 对象|
|视频审核| `VideoClassifyResult examineVideoUrls(String uri, String format)` |审核一个网络视频是否有色情内容| `uri`: 字符串，待检测的视频URL字符串，`format`: 字符串，待检测的视频格式 | `VideoClassifyResult` 对象|

##### 清空服务器的临时文件夹

|    | 接口 | 描述  | 参数 | 返回值 |
|:--:|:------------| :------------|:-------------|:-----------|
|清空服务器的临时文件夹|`ResetCacheString examineResetCache()` | 清空服务器的临时文件夹 | 无 | `ResetCacheString` 字符串，清空服务器临时文件夹的结果 |


***
## 主要的数据结构和成员变量
***

##### format
|  数据结构/变量  | 类型|说明       |
|:--:|:------------| :------------|
|format| 字符串 | 视频文件的格式，如"mp4","mpeg"|


##### ClassifyResult

|成员变量   | 类型 | 描述|
|:--|:------------| :------------|
|results     |Map<String,List<String>>| 返回正确的结果，包括scores：检验数值，tags：审核结果，或"normal",或"porn"     |
|error       |  String  | 当请求失败时才会返回  |



##### VideoClassifyResult

|成员变量   | 类型 | 描述|
|:--|:------------| :------------|
|results     |String| 返回正确的结果，表示审核结果，或"normal",或"porn"     |
|error       |  String  | 当请求失败时才会返回  |


##### ResetCacheString

|成员变量   | 类型 | 描述|
|:--|:------------| :------------|
|Success     |String| 请求成功返回true    |
|error       |String| 当请求失败时才会返回  |

# 人脸识别SDK使用指南


### 索引
* [初次使用SDK](#初次使用SDK)
  * [使用 Java SDK](#使用Java-SDK)
  * [使用 Python SDK](#使用Python-SDK)
* [常见的使用场景](#主要的数据结构和成员变量)
  * [创建人物和人脸数据库](#创建人物和人脸数据库)
  * [人脸识别](#人脸识别)
  * [人脸对比](#人脸对比)

术语

- 照片：一个照片文件，或照片URL
- 人物：一个人物对象，由SDK用户创建时生成实例，系统返回personId作为唯一标识
- 人脸：一个对象，由SDK用户在对照片做人脸检测时生成实例，并且faceId作为唯一标识
- 人脸检测(face detect)：判断并获取一张照片中的人脸
- 人脸对比(face compare)：独立判断两个人脸的相似度（与底库无关）
- 人脸识别(face identify)：识别某人脸最像底库中的某人，并给出相似度

***
## 初次使用SDK
***

#### 使用Java-SDK

import SDK所需的库文件
```
import java.io.File;
import java.io.IOException;
import java.net.URL;
import java.util.List;
import com.snowsense.*;
```

设置鉴权信息
```
public static final String SERVER = "Server URL";
public static final String API_KEY = "your API key ";
public static final String API_SECRET = "your API secret";
```

#### 使用Python-SDK

`git clone https://github.com/SnowSense/python-sdk.git` 后，python-sdk目录下主要文件说明
```
apikey.cfg    鉴权信息
demo_api.py		SDK的API函数实现
demo_app.py		SDK的API函数在典型场景下的使用示例
```

导入SDK: `from python-sdk.demo_api import *` 或 `from demo_api import *`，取决于调用代码和SDK代码所在的相对目录

设置鉴权信息：修改 python-sdk/apikey.cfg
```
{
  "SERVER": "Server IP or URL",
  "API_KEY": "your API key",
  "API_SECRET": "your API secret"
}
```

***
## 常见的使用场景
***

#### 创建人物和人脸数据库

使用场景：在初次创建人脸数据库(底库)或后续向底库中增加人物时使用
说明：每个API key 对应一个独立的底库

步骤：

1. 向系统中创建一个人物，输入为人物姓名，以及其他信息（可选，格式自定义）；系统返回一个personId;
2. 对该人物的某张照片做人脸检测，获取人脸ID (faceId)
3. 将人物ID(faceId)和人脸ID (faceId)关联起来
4. 对该人物重复步骤2和3 （可选）
5. 重复步骤1到4完成底库创建或批量人物和人脸导入


Java Example
```
sdk = FaceSDK.getsInstance();

// step 1
Map<String, String> data = new HashMap<>();
data.put("foo", "bar2");
data.put("sex", "M");
data.put("name", "FirstName Deo");
personId = sdk.addPerson(personId, data);

// step 2
DetectFaceResult detectFaceResult1 = FaceSDK.getsInstance().detectFace(getFileFromResource("test.jpg"));
faceId1 = detectFaceResult1.getFaces().get(0).getFaceId(); // 假定一张照片只有一个人

// step 3
res = sdk.labelFace(personId, faceId1)
```

Python Example
```
from demo_api import *

# step 1
name = u'David'
person_id = add_person(name, {'age':30})
# step 2
res_obj = detect_face('/path/to/your/image')
# step 3
face_id = res_obj['faces'][0]['faceId']
result = link_person_to_face(person_id, face_id)
print (result)
```

#### 人脸识别

使用场景：判断照片中的人物最像底库中的某人; 要求预先已经建立起底库

步骤：

1. 对某张照片做人脸检测，获取人脸ID (faceId)列表
2. 对列表中的每个faceId做人脸识别(face identify)，得到返回结果


Java Example
```
sdk = FaceSDK.getsInstance();
DetectFaceResult detectFaceResult1 = FaceSDK.getsInstance().detectFace(getFileFromResource("test.jpg"));
faceId1 = detectFaceResult1.getFaces().get(0).getFaceId();
IdentifyResult res = FaceSDK.getsInstance().identifyFace(faceId1);
```

Python Example
```
from demo_api import *

# step 1
res_obj = detect_face('/path/to/your/image')
face_id = res_obj['faces'][0]['faceId']

# step 2
res = identify_face(face_id)
print (res)
```

#### 人脸对比

使用场景：判断两个人脸的相似度，而无需知道所属人物；可应用在人证合一等场景下

步骤：

1. 对照片1和照片2，分别做人脸检测，获取2个人脸ID (faceId)
2. 调用人脸对比API，对比2个人脸ID (faceId)，并返回相似度


Java Example
```
sdk = FaceSDK.getsInstance();

// step 1
DetectFaceResult detectFaceResult1 = FaceSDK.getsInstance().detectFace(getFileFromResource("test.jpg"));
DetectFaceResult detectFaceResult2 = FaceSDK.getsInstance().detectFace("http://img1.gtimg.com/ent/pics/hv1/232/199/1996/129840877.jpg");
faceId1 = detectFaceResult1.getFaces().get(0).getFaceId(); // 假定一张照片只有一个人
faceId2 = detectFaceResult2.getFaces().get(0).getFaceId(); // 假定一张照片只有一个人

// step 2
CompareResult compareResult = FaceSDK.getsInstance().compareFaces(faceId1, faceId2);
```

Python Example
```
from demo_api import *

# step 1
image1_res = detect_face('/path/to/your/image1')
image2_res = detect_face('/url/to/your/image2')
face_id1 = image1_res['faces'][0]['faceId'] # 取图片检测返回的第一个人脸
face_id2 = image2_res['faces'][0]['faceId'] # 取图片返回的的第一个人脸

# step 2
res = compare_face(face_id1, face_id2)
print('comparison results:')
print(res)
```

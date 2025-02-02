## 功能描述

图片压缩指在图片质量保持不变的情况，尽可能的减小图片大小，以达到节省图片存储空间、减少图片访问流量、提升图片访问速度的效果。

对象存储（Cloud Object Storage，COS）基于 [数据万象（Cloud Infinite，CI）](https://cloud.tencent.com/document/product/460/6962) 产品推出了 WebP 压缩功能，可将图片转换为 webp 压缩图片格式，其在压缩方面相比 jpg 格式更优越。在相同图片质量的情况下，webp 格式图片要比 jpg 格式图片减小25%以上，可以适配多终端使用场景。

## 使用方式

COS 通过数据万象 imageMogr2 接口提供 WebP 压缩功能。

该功能支持以下的处理方式：

- 下载时处理
- 上传时处理
- 云上数据处理

>?WebP 压缩为付费服务，费用同基础图片处理，由数据万象收取，具体费用请参见数据万象 [图片处理费用](https://cloud.tencent.com/document/product/460/58117)。

## 接口示例

#### 1. 下载时处理

```plaintext
download_url?imageMogr2/format/webp
```

#### 2. 上传时处理

```http
PUT /<ObjectKey> HTTP/1.1
Host: <BucketName-APPID>.cos.<Region>.myqcloud.com
Date: GMT Date
Authorization: Auth String
Pic-Operations: 
{
"is_pic_info": 1,
"rules": [{
    "fileid": "exampleobject",
    "rule": "imageMogr2/format/webp"
}]
}
```

#### 3. 云上数据处理

```http
POST /<ObjectKey>?image_process HTTP/1.1
Host: <BucketName-APPID>.cos.<Region>.myqcloud.com
Date: GMT Date
Content-length: Size
Authorization: Auth String
Pic-Operations: 
{
"is_pic_info": 1,
"rules": [{
    "fileid": "exampleobject",
    "rule": "imageMogr2/format/webp"
}]
}
```

>? 本篇文档中的实际案例仅包含**下载时处理**，该类处理不会保存处理后的图片至存储桶。如有保存需求，请使用**上传时处理**或**云上数据处理**方式。
>

## 处理参数说明

| 参数             | 含义                                                         |
| :--------------- | :----------------------------------------------------------- |
| download_url     | 文件的访问链接，具体构成为&lt;BucketName-APPID>.cos.&lt;Region>.myqcloud.com/&lt;picture name>， 例如`examplebucket-1250000000.cos.ap-shanghai.myqcloud.com/picture.jpeg`。 |
| /format/&lt;Format> | 压缩格式，此处为 webp。                                       |

## 实际案例

假设原图格式为 png，图片大小为1335.2KB，如下图所示：
![img](https://example-1258125638.cos.ap-shanghai.myqcloud.com/sample.png)

将原图转换为 webp 格式，请求 URL 如下：

```plaintext
http://example-1258125638.cos.ap-shanghai.myqcloud.com/sample.png?imageMogr2/format/webp
```

效果如下：
![img](https://example-1258125638.cos.ap-shanghai.myqcloud.com/sample.png?imageMogr2/format/webp)

**压缩率对比**

| 格式        | 图片大小             |
| :---------- | :------------------- |
| png（原图） | 1335.2KB             |
| webp        | 65KB（压缩率95.13%） |

概念

## 常用包说明：

```java
COS Java SDK 中的常见类所在包分别为：
客户端配置相关类在 com.qcloud.cos.* 包下。
权限相关类在 com.qcloud.cos.auth.* 子包下。
异常相关类在 com.qcloud.cos.exception.* 子包下。
请求相关类在 com.qcloud.cos.model.* 子包下。
地域相关类在 com.qcloud.cos.region.* 子包下。
高级 API 接口在 com.qcloud.cos.transfer.* 子包下。
```



# 创建服务端(腾讯COS/对象存储):

官网：https://cloud.tencent.com/

前提：创建并实名认证腾讯云。

1. 开通“对象存储COS”服务
1. 找到存储桶列表，并创建存储桶

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%9C%8D%E5%8A%A1%E5%99%A8/%E8%85%BE%E8%AE%AF/%E5%AF%B9%E8%B1%A1%E5%AD%98%E5%82%A8cos/%E5%88%9B%E5%BB%BA%E6%A1%B6.png)

3.创建API秘钥

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%9C%8D%E5%8A%A1%E5%99%A8/%E8%85%BE%E8%AE%AF/%E5%AF%B9%E8%B1%A1%E5%AD%98%E5%82%A8cos/%E5%88%9B%E5%BB%BAAPI%E7%A7%98%E9%92%A5.png)

# 基本使用:

[对象存储 SDK 文档](https://cloud.tencent.com/document/product/436/10199)

依赖：

```xml
<dependencies>
    <!-- 腾讯云COS依赖 -->
    <dependency>
        <groupId>com.qcloud</groupId>
        <artifactId>cos_api</artifactId>
        <version>5.6.54</version>
    </dependency>
</dependencies>
```

java:

```java
import com.alibaba.fastjson.JSON;
import com.qcloud.cos.COSClient;
import com.qcloud.cos.ClientConfig;
import com.qcloud.cos.auth.BasicCOSCredentials;
import com.qcloud.cos.auth.COSCredentials;
import com.qcloud.cos.http.HttpProtocol;
import com.qcloud.cos.model.*;
import com.qcloud.cos.region.Region;

import java.io.File;

/* 腾讯对象存储：COS */
public class TXCos {

    public static void main(String[] args) {
        // ==================初始化客户端=======
        // 1 初始化用户身份信息（secretId, secretKey）。  注意：SecretId和SecretKey都可使用子账号秘钥，遵循授权最小权限,降低使用风险。
        //        String secretId = System.getenv("AKIDmRvEMPJD0d4HH3cvEJF0aw73jJl26ua3");// 注意：不能使用这个否则报 Access key cannot be null错
        String secretId = "AKIDmRvEMPJD0d4HH3cvEJF0aw73jJl26ua3";
        String secretKey = "H8nFWZYQp23rRogT5qTrztbBbv67wZ2g";
        COSCredentials cred = new BasicCOSCredentials(secretId, secretKey);
        // 2 设置 bucket 的地域,
        // clientConfig 中包含了设置 region, https(默认 http), 超时, 代理等 set 方法
        Region region = new Region("ap-guangzhou");  // 地域
        ClientConfig clientConfig = new ClientConfig(region);
        // 从 5.6.54 版本开始，默认使用 https
        clientConfig.setHttpProtocol(HttpProtocol.https);
        // 3 生成 cos 客户端。
        COSClient cosClient = new COSClient(cred, clientConfig);

        try {
            // ==================上传文件头=======
            // 注意：此方法适用于20M以下图片类小文件上传，最大支持上传不超过5GB文件。5GB以上的文件必须使用分块上传或高级 API 接口上传。
            // 指定要上传的文件
            File localFile = new File("G:\\1111111.png");
            // 指定文件将要存放的存储桶  "
            String bucketName = "ggkt-1316853721";
            // 指定文件上传到 COS 上的路径，即对象键。例如对象键为 folder/picture.jpg，则表示将文件 picture.jpg 上传到 folder 路径下
            String key = "/2023/5/7/华为ICP.png";  // 修改名称并上传到指定的目录
            PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, key, localFile);
            PutObjectResult putObjectResult = cosClient.putObject(putObjectRequest);
            System.out.println(JSON.toJSONString(putObjectResult));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



# SpringBoot整合：

## 依赖：

```xml
<dependencies>
    <!-- 腾讯云COS依赖 -->
    <dependency>
        <groupId>com.qcloud</groupId>
        <artifactId>cos_api</artifactId>
        <version>5.6.54</version>
    </dependency>
    <!-- 日期工具栏依赖 -->
    <dependency>
        <groupId>joda-time</groupId>
        <artifactId>joda-time</artifactId>
    </dependency>
</dependencies>
```

## application.properties:

```properties
# =================================整合腾讯对象存储COS
# 指定上传文件的最大大小,超过指定的大小抛异常：单个文件最大大小为1024MB，
spring.servlet.multipart.max-file-size=1024MB
# 整个请求的最大大小为1024MB
spring.servlet.multipart.max-request-size=1024MB

# 地域，秘钥：secretId，secretKey     注意：不同的服务器，地址不同
tencent.cos.file.region=ap-guangzhou
tencent.cos.file.secretid=AKIDmRvEMPJD0d4HH3cvEJF0aw73jJl26ua3
tencent.cos.file.secretkey=H8nFWZYQp23rRogT5qTrztbBbv67wZ2g
# 指定文件将要存放的存储桶        注意：bucket(存储桶)可以在控制台创建，也可以使用java代码创建
tencent.cos.file.bucketname=ggkt-1316853721 
```

## Config/TXCosConfig:

```java
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

/** 腾讯对象存储-读取properties文件的地域；秘钥：secretId，secretKey ；存储桶 */
@Component //配置类
public class TXCosConfig implements InitializingBean {

    // 读取properties文件的地域；秘钥：secretId，secretKey ；存储桶
    @Value("${tencent.cos.file.region}")
    private String region;
    @Value("${tencent.cos.file.secretid}")
    private String secretId;
    @Value("${tencent.cos.file.secretkey}")
    private String secretKey;
    @Value("${tencent.cos.file.bucketname}")
    private String bucketName;

    // 设置地域；秘钥：secretId，secretKey ；存储桶的权限为public
    public static String END_POINT;
    public static String ACCESS_KEY_ID;
    public static String ACCESS_KEY_SECRET;
    public static String BUCKET_NAME;

    @Override
    public void afterPropertiesSet() throws Exception {
        END_POINT = region;   // 地域
        ACCESS_KEY_ID = secretId; // secretId
        ACCESS_KEY_SECRET = secretKey; //secretKey
        BUCKET_NAME = bucketName; //存储桶
    }
}
```

## Service：

1.创建接口

```java

public interface FileService {
    // 文件上传
    String upload(MultipartFile file);
}
```

2.实现类：

```java
import com.alibaba.fastjson.JSON;
import com.qcloud.cos.COSClient;
import com.qcloud.cos.ClientConfig;
import com.qcloud.cos.auth.BasicCOSCredentials;
import com.qcloud.cos.auth.COSCredentials;
import com.qcloud.cos.http.HttpProtocol;
import com.qcloud.cos.model.*;
import com.qcloud.cos.region.Region;
import com.qsl.ggktparent.vod.config.TXCosConfig;
import com.qsl.ggktparent.vod.service.FileService;
import org.joda.time.DateTime;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

import java.io.InputStream;
import java.util.UUID;

/* 腾讯对象存储-上传文件*/
@Service
public class FileServiceImpl implements FileService {

    @Override
    public String upload(MultipartFile file) {
        // ==================初始化客户端=======
        // 1 初始化用户身份信息（secretId, secretKey）。  注意：SecretId和SecretKey都可使用子账号秘钥，遵循授权最小权限,降低使用风险。
        String secretId = TXCosConfig.ACCESS_KEY_ID;
        String secretKey = TXCosConfig.ACCESS_KEY_SECRET;
        COSCredentials cred = new BasicCOSCredentials(secretId, secretKey);
        // 2 设置 bucket 的地域,    注意： clientConfig 中包含了设置 region, https(默认 http), 超时, 代理等 set 方法
        Region region = new Region(TXCosConfig.END_POINT);  // 地域
        System.out.println(region);

        ClientConfig clientConfig = new ClientConfig(region);
        // 从 5.6.54 版本开始，默认使用 https
        clientConfig.setHttpProtocol(HttpProtocol.https);
        // 3 生成 cos 客户端。
        COSClient cosClient = new COSClient(cred, clientConfig);


        // 存储桶
        String bucketName = TXCosConfig.BUCKET_NAME;
        // 对象键(Key)是对象在存储桶中的唯一标识。   解决文件名称不重复：获取UUId并去除”-“，获取文件名称
        String key = UUID.randomUUID().toString().replaceAll("-", "")
            + file.getOriginalFilename(); // 文件名称
        // 根据日期实现文件分组
        String dateTime = new DateTime().toString("yyyy/MM/dd");
        key = dateTime + "/" + key;

        try {
            // 指定要上传的文件
            InputStream inputStream = file.getInputStream();
            ObjectMetadata objectMetadata = new ObjectMetadata();

            // 存储桶,文件名称，输入流，
            PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, key, inputStream, objectMetadata);
            // 设置存储类型, 默认是标准(Standard), 低频(standard_ia)
            putObjectRequest.setStorageClass(StorageClass.Standard_IA);
            // 上传
            PutObjectResult putObjectResult = cosClient.putObject(putObjectRequest);

            System.out.println(JSON.toJSONString(putObjectResult));
            //  拼接返回路径：  https://ggkt-atguigu-1310644373.cos.ap-beijing.myqcloud.com/01.jpg
            String url = "https://" + bucketName + ".cos." + region.getRegionName() + ".myqcloud.com/" + key;
            System.out.println("@@@" + url);
            return url;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

## Controller:

```java
import com.qsl.ggktparent.utils.Result;
import com.qsl.ggktparent.vod.service.FileService;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import io.swagger.annotations.ApiParam;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

/**
 * 腾讯对象存储-上传文件
 */
@Api(tags = "腾讯对象存储-上传文件")
@RestController // bean
@RequestMapping("/admin/vod/file")
@CrossOrigin // 支持浏览器跨域（注意：上传图片必须使用这个，前端使用代理服务器没用。）
public class FileUploadController {
    @Autowired
    private FileService fileService;

    @ApiOperation("文件上传")
    @PostMapping("upload")
    public Result upload(@ApiParam(name = "file", value = "文件")
                         @RequestParam("file") MultipartFile file) {
        String uploadUrl = fileService.upload(file);
        return Result.ok(uploadUrl).message("文件上传成功");
    }
}
```

Swagger测试上传文化成功。



注意：上传图片等必须使用支持浏览器跨域@CrossOrigin注解，否则报以下错。

```java
cess to XMLHttpRequest at 'http://localhost:8301/admin/vod/file/upload' from origin 'http://localhost:9528' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
element-ui.common.js:29068     POST http://localhost:8301/admin/vod/file/upload net::ERR_FAILED 200
upload
```



## 前端：



```js


```







# 腾讯云点播(VOD):

   腾讯云点播（Video on Demand，VOD）基于腾讯多年技术积累与基础设施建设，为有音视频应用相关需求的客户提供包括音视频存储管理、音视频转码处理、音视频加速播放和音视频通信服务的一站式解决方案。
   官网文档中心：https://cloud.tencent.com/document/product/266



依赖：

```xml
<!-- 腾讯云VOD依赖-->
<dependency>
    <groupId>com.qcloud</groupId>
    <artifactId>vod_api</artifactId>
    <version>2.1.4</version>
    <exclusions>  <!-- 排除slf4j依赖日志-->
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

## 上传视频:

1. 开通"云点播"服务:
2. 上传视频:

​        注意：视频(处理)转码需要收费。

[上传视频文档]：https://cloud.tencent.com/document/product/266/10276

web页面上传视频:

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%9C%8D%E5%8A%A1%E5%99%A8/%E8%85%BE%E8%AE%AF/%E8%A7%86%E9%A2%91%E7%82%B9%E6%92%ADvod/%E4%B8%8A%E4%BC%A0%E8%A7%86%E9%A2%91.png)

### 服务端：

流程：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%9C%8D%E5%8A%A1%E5%99%A8/%E8%85%BE%E8%AE%AF/%E8%A7%86%E9%A2%91%E7%82%B9%E6%92%ADvod/%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8A%E4%BC%A0%E6%B5%81%E7%A8%8B.png)

#### Serve:

接口：

```java
public interface FileService {
    // VOD-视频上传
  String addVod(InputStream inputStream, String originalFilename);
}
```



实现类：

```java
@Service
public class FileServiceImpl implements FileService {

    // VOD-服务端-视频上传
    @Override
    public String addVod(InputStream inputStream, String originalFilename) {
        try {
            VodUploadClient client = new VodUploadClient(TXCosConfig.ACCESS_KEY_ID, TXCosConfig.ACCESS_KEY_SECRET); // secretId,secretKey
            VodUploadRequest request = new VodUploadRequest();
            request.setMediaFilePath("D:\\HYLiteResources\\video\\鬼刀.mp4");// 视频路径
            //            request.setProcedure("LongVideoPreset"); // 任务流
            request.setConcurrentUploadNumber(5); // 分片上传
            VodUploadResponse response = client.upload(TXCosConfig.END_POINT, request); //地域
            //返回文件id保存到业务表，用于控制视频播放
            String fileId = response.getFileId();

            return fileId;
        } catch (Exception e) {
            // 业务方进行异常处理
            throw new BusinessException(20001, "视频上传失败");
        }
    }
}
```



#### Controller:

```java
@Api(tags = "腾讯云-上传文件、视频")
@RestController // bean
@RequestMapping("/admin/vod/file")
@CrossOrigin // 支持浏览器跨域（注意：上传图片必须使用这个，前端使用代理服务器没用。）
public class FileUploadController {
    @Autowired
    private FileService fileService;
    
    @ApiOperation("VOD-服务端-视频上传")
    @PostMapping("addVod")
    public Result addVod(@ApiParam(name = "file", value = "文件", required = true)
                         @RequestParam("file") MultipartFile file) throws IOException {
        InputStream inputStream = file.getInputStream();
        String originalFilename = file.getOriginalFilename();
        String videoId = fileService.addVod(inputStream, originalFilename);
        return Result.ok(videoId); //返回视频id
    }
}
```

 Swagger上传文件测试成功。。。。



### 客户端：

流程：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%9C%8D%E5%8A%A1%E5%99%A8/%E8%85%BE%E8%AE%AF/%E8%A7%86%E9%A2%91%E7%82%B9%E6%92%ADvod/%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%B8%8A%E4%BC%A0%E6%B5%81%E7%A8%8B.png)



#### 申请上传签名：

```java
package com.qsl.ggktparent.vod.util;
import java.util.Random;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import sun.misc.BASE64Encoder;

/**
 * 腾讯云-VOD—视频上传2_服务端上传（申请上传签名）
 */
public class Signature {
    private String secretId;
    private String secretKey;
    private long currentTime;
    private int random;
    private int signValidDuration;
    private static final String HMAC_ALGORITHM = "HmacSHA1"; //签名算法
    private static final String CONTENT_CHARSET = "UTF-8"; //UTF-8

    public static byte[] byteMerger(byte[] byte1, byte[] byte2) {
        byte[] byte3 = new byte[byte1.length + byte2.length];
        System.arraycopy(byte1, 0, byte3, 0, byte1.length);
        System.arraycopy(byte2, 0, byte3, byte1.length, byte2.length);
        return byte3;
    }

    // 获取签名
    public String getUploadSignature() throws Exception {
        String strSign = "";
        String contextStr = "";

        // 生成原始参数字符串
        long endTime = (currentTime + signValidDuration);
        contextStr += "secretId=" + java.net.URLEncoder.encode(secretId, "utf8");
        contextStr += "&currentTimeStamp=" + currentTime;
        contextStr += "&expireTime=" + endTime;
        contextStr += "&random=" + random;
        contextStr += "&procedure=LongVideoPreset";//设置转码任务流

        try {
            Mac mac = Mac.getInstance(HMAC_ALGORITHM);
            SecretKeySpec secretKey = new SecretKeySpec(this.secretKey.getBytes(CONTENT_CHARSET), mac.getAlgorithm());
            mac.init(secretKey);


            byte[] hash = mac.doFinal(contextStr.getBytes(CONTENT_CHARSET));
            byte[] sigBuf = byteMerger(hash, contextStr.getBytes("utf8"));
            strSign = base64Encode(sigBuf);
            strSign = strSign.replace(" ", "").replace("\n", "").replace("\r", "");
        } catch (Exception e) {
            throw e;
        }
        return strSign;
    }

    private String base64Encode(byte[] buffer) {
        BASE64Encoder encoder = new BASE64Encoder();
        return encoder.encode(buffer);
    }


    public void setSecretId(String secretId) {
        this.secretId = secretId;
    }


    public void setSecretKey(String secretKey) {
        this.secretKey = secretKey;
    }


    public void setCurrentTime(long currentTime) {
        this.currentTime = currentTime;
    }


    public void setRandom(int random) {
        this.random = random;
    }


    public void setSignValidDuration(int signValidDuration) {
        this.signValidDuration = signValidDuration;
    }
}
```

#### Controller ：

```java
@Api(tags = "腾讯云-上传文件、视频")
@RestController // bean
@RequestMapping("/admin/vod/file")
@CrossOrigin // 支持浏览器跨域 （注意：上传图片必须使用这个，前端使用代理服务器没用。）
public class FileUploadController {

    @ApiOperation("VOD-客户端-视频上传")
    @GetMapping("sign")
    public Result sign() {
        Signature sign = new Signature();
        // 设置App的云API密钥，此处读取SecretId和SecretKey实体类省略
        sign.setSecretId(TXCosConfig.ACCESS_KEY_ID);// SecretId
      sign.setSecretKey(TXCosConfig.ACCESS_KEY_SECRET);//SecretKey
        sign.setCurrentTime(System.currentTimeMillis() / 1000);
        sign.setRandom(new Random().nextInt(java.lang.Integer.MAX_VALUE));
        sign.setSignValidDuration(3600 * 24 * 2); // 签名有效期：2天

        try {
            String signature = sign.getUploadSignature();
            System.out.println("signature : " + signature);
            return Result.ok(signature);
        } catch (Exception e) {
            throw new BusinessException(20001, "VOD-客户端-视频上传失败");
        }
    }
}
```

 Swagger上传文件测试成功。。。。





####    签名(BASE64Decoder)异常：

   原因：java8后的版本去掉了BASE64Decoder类。
   解决方法：打开 file->project structure ，把java版本改为8即可。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E6%9C%8D%E5%8A%A1%E5%99%A8/%E8%85%BE%E8%AE%AF/%E8%A7%86%E9%A2%91%E7%82%B9%E6%92%ADvod/%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%B8%8A%E4%BC%A0%E7%AD%BE%E5%90%8D%E5%BC%82%E5%B8%B8.png)





## 删除视频：

   [API文档]：(https://console.cloud.tencent.com/api/explorer?Product=vod&Version=2018-07-17&Action=DescribeMediaInfos)

### Serve:

接口：

```java
public interface FileService {

    // VOD-删除视频
    void removeVideo(String FileId);
}
```



实现类：

```java
@Service
public class FileServiceImpl implements FileService {
    // VOD-删除视频
    @Override
    public void removeVideo(String FileId) {
        try {
            // 实例化一个认证对象，入参需要传入腾讯云账户 SecretId 和 SecretKey，此处还需注意密钥对的保密
            Credential cred = new Credential(TXCosConfig.ACCESS_KEY_ID, TXCosConfig.ACCESS_KEY_SECRET);
            // 实例化一个http选项，可选的，没有特殊需求可以跳过
            HttpProfile httpProfile = new HttpProfile();
            httpProfile.setEndpoint("vod.tencentcloudapi.com"); // 设置API服务的请求域名，调用腾讯云视频点播(VOD)的API接口。
            // 实例化一个client选项，可选的，没有特殊需求可以跳过
            ClientProfile clientProfile = new ClientProfile();
            clientProfile.setHttpProfile(httpProfile);
            // 实例化要请求产品的client对象,clientProfile是可选的
            VodClient client = new VodClient(cred, TXCosConfig.END_POINT,clientProfile); //地域
            // 实例化一个请求对象,每个接口都会对应一个request对象
            DeleteMediaRequest req = new DeleteMediaRequest();
            req.setFileId(FileId);

            // 返回的resp是一个DescribeMediaInfosResponse的实例，与请求对象对应
            DeleteMediaResponse resp = client.DeleteMedia(req);
            // 输出json格式的字符串回包
            System.out.println(DescribeMediaInfosResponse.toJsonString(resp));
        } catch (TencentCloudSDKException e) {
            throw  new BusinessException(20001,"删除视频失败");
        }
    }
}
```

### Controller:

```java
@Api(tags = "腾讯云-上传文件、视频")
@RestController // bean
@RequestMapping("/admin/vod/file")
@CrossOrigin // 支持浏览器跨域 （注意：上传图片必须使用这个，前端使用代理服务器没用。）
public class FileUploadController {
    @Autowired
    private FileService fileService;

    @ApiOperation("VOD-删除视频")
    @DeleteMapping("removeById/{FileId}")
    public Result removeById(@ApiParam(value = "FileId", required = true) @PathVariable String FileId) {
        fileService.removeVideo(FileId);
        return Result.ok("null");
    }
}
```

 Swagger输入FileId删除视频。





## 视频播放：

​         视频播放的类型：URL播放，FileID播放，自适应码流播放，DASH播放。

- ​     url播放: 
   ​            优点：可以将视频URL嵌入到网页中，用户点击即可播放。 
   ​            缺点：URL播放存在安全性问题，视频路径可以被人轻易的截取、下载和分享。
- ​    FileID播放: 通过云点播的FileID播放视频。
   ​            优点：
     ​               ①FileID播放更加安全，因为只有在拥有访问权限的情况下，才能够使用FileID播放视频。
   ​                 ②FileID支持防盗链功能，确保视频内容不会被非法盗用。



​        视频播放文档：https://cloud.tencent.com/document/product/266/63004

​            [云点播 播放原始视频文档](https://cloud.tencent.com/document/product/266/46217)



​            ②FileID播放必须经过腾讯云转码，否则原始视频可能无法再浏览器中播放。

​             ③FileID播放必须生成播放器签名才能播放。。。。

  注意：微信公众号pc端视频播放有点问题，切换到网页或手机端播放。。。

​     腾讯云测试视频播放网页工具：https://rtcube.cloud.tencent.com/component/experience-center/index.html#/detail?scene=player

   腾讯云测试视频播放网页工具2：https://tcplayer.vcube.tencent.com/



# 返回树形结构数据-懒加载：

## POJO:

```java

import com.baomidou.mybatisplus.annotation.*;
import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;
import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

import java.util.Date;
import java.util.HashMap;
import java.util.Map;

@Data
@ApiModel(description = "Subject")
@TableName("subject")
public class Subject {

    private static final long serialVersionUID = 1L;
    @ApiModelProperty(value = "id")
    private Long id;

    @ApiModelProperty(value = "创建时间")
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    @TableField("create_time")
    private Date createTime;

    @ApiModelProperty(value = "更新时间")
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    @TableField("update_time")
    private Date updateTime;

    @ApiModelProperty(value = "逻辑删除(1:已删除，0:未删除)")
    @JsonIgnore
    @TableLogic
    @TableField("is_deleted")
    private Integer isDeleted;

    @ApiModelProperty(value = "其他参数")
    @TableField(exist = false)
    private Map<String,Object> param = new HashMap<>();

    @ApiModelProperty(value = "类别名称")
    @TableField("title")
    private String title;

    @ApiModelProperty(value = "父ID")
    @TableField("parent_id")
    private Long parentId;

    @ApiModelProperty(value = "排序字段")
    @TableField("sort")
    private Integer sort;

    @ApiModelProperty(value = "是否包含子节点")
    @TableField(exist = false) // 设置数据库不存在此字段
    private boolean hasChildren;
}
```

## Mapper:

```java
// 课程
@Mapper
public interface SubjectMapper extends BaseMapper<Subject> {

}
```

## Service:

1. 接口：

```java
public interface SubjectService extends IService<Subject> {

    // 课程查询-树形结构_懒加载
    List<Subject> selectList(Long id);
}
```



2. 实现类

```java
@Service
public class SubjectServiceImpl extends ServiceImpl<SubjectMapper, Subject> implements SubjectService {

    // 课程查询-树形结构_懒加载
    @Override
    public List<Subject> selectList(Long id) {
        QueryWrapper<Subject> qw = new QueryWrapper<>();
        qw.eq("parent_id", id);
        // MybatisPlus注入Mapper层的两种方式：①@AuAutowired注解注入，②BaseMapper注入
        List<Subject> subjects = baseMapper.selectList(qw); // 查询parent_id为0的数据
        // TODO  根据parent_id判断是否有下一层数据，有hasChildren=true
        for (Subject subject : subjects) {
            Long subjectId = subject.getId();
            // 查询parent_id是否有下一层数据，有hasChildren设置为true
            boolean isChild = this.isChildren(subjectId);
            subject.setHasChildren(isChild);
        }
        return subjects;
    }

    // 查询parent_id是否有下一层数据，有true
    private boolean isChildren(Long id) {
        QueryWrapper<Subject> qw = new QueryWrapper<>();
        qw.eq("parent_id", id);
        Integer count = baseMapper.selectCount(qw);
        return count > 0; // 1>0:true  0>0:false
    }
}
```

## Controller:

```java
@Api(tags = "课程接口")
@RestController
@RequestMapping(value = "/admin/vod/subject")
public class SubjectController {

    @Autowired
    private SubjectService subjectService;

    @ApiOperation("课程查询-树形结构_懒加载")
    @GetMapping("getChildSubject/{id}")
    public Result getSubjectService(@PathVariable("id") Long id) {
        List<Subject> list= subjectService.selectList(id);
        return Result.ok(list);
    }
}
```





# 其他：

## @JsonIgnore注解：

```java
@JsonIgnore：用于标记某个属性不应该被序列化或反序列化（即忽略该属性）。在序列化时，被标记为@JsonIgnore的属性将被忽略，不会被包含在输出中。
    // 在反序列化时，则会忽略输入中的@JsonIgnore属性。这个注解通常用于处理JSON格式数据的序列化和反序列化，在使用Jackson等库进行操作时特别有用。

    // 实体类中的删除
    @ApiModelProperty(value = "逻辑删除(1:已删除，0:未删除)")
    @JsonIgnore
    @TableLogic
    @TableField("is_deleted")
    private Integer isDeleted;  
```



## EasyExcel表头设置

```java
// 定义表头样式
WriteCellStyle headerStyle = new WriteCellStyle();
// 对齐方式设置为居中
headerStyle.setHorizontalAlignment(HorizontalAlignment.CENTER);

// 创建sheet对象
WriteSheet sheet1 = EasyExcel.writerSheet(sheetName).head(head).registerWriteHandler(new HorizontalCellStyleWriteHandler(headerStyle)).build();

// 进行写操作
EasyExcel.write(response.getOutputStream(), SubjectEeVo.class).excelType(ExcelTypeEnum.XLSX).sheet(sheetName).doWrite(subjectEeVoList);
```



## 异常：

```java
@SneakyThrows // @SneakyThrows注解 将方法内抛出的受检查异常转化为不受检查异常并抛出,替代try-catch块
public void syncMenu() {
    int i/10;  // 自定义异常
}
```





## vue:

```js
// 强制刷新
this.$forceUpdate()


// 停止运行
debugger

// ===== 折叠代码（region：开启；  endregion：结束） 
//#region 
//#endregion

// Mybatis
@MapKey("id") // @MapKey("id")； 返回Map值

videoApi
    .save(this.video)
    .then((msg) => {
    this.$message.success("新增成功");
})
    .catch(() => {
    this.$message.error("新增失败");
}).finally(() => {
    this.dialogVisible = false; // 是否开启弹窗
    this.$parent.fetchNodeList(); // 调用父组件查询
});

getPageList(page, limit, searchObj) {
    return request({
        url: `${api_name}/PageCource/${page}/${limit}`,
        method: 'get',
        // data: searchObj  // 传递json数据
        params: searchObj  // 路径拼接参数  例：http://localhost:9528/admin/vod/course/PageCource/1/5?subjectId=&subjectParen
    })
},
```





# 微信公众号：

注意：测试账号不支持支付，授权不完整。

   注册文档：https://kf.qq.com/faq/120911VrYVrA151013MfYvYV.html

​      注册服务号，订阅号不具备支付功能。

具体权限：[公众号 (qq.com)](https://mp.weixin.qq.com/advanced/advanced?action=table&token=1920472324&lang=zh_CN)

```java
注意：要使用weixin-java-mp的wxMpService.oauth2buildAuthorizationUrl必须使用2.7不能使用4.1版本。 （4.1版本移除了相关接口。。。）
```



## 获取access_token：

   注意：进行菜单同步时候，需要获取到公众号的access_token，通过access_token进行菜单同步。

​    文档：https://developers.weixin.qq.com/doc/offiaccount/Basic_Information/Get_access_token.html

​     access_token是公众号的全局唯一接口调用凭据，公众号调用各接口时都需使用access_token。access_token的有效期目前为2个小时，需定时刷新，重复获取将导致上次获取的access_token失效。

application

```properties
# =================================公众号id和秘钥
# 公众平台appId
wechat.mpAppId:wx09f201e9013e81d8
# 微信公众平台api秘钥：
wechat.mpAppSecret:6c999765c12c51850d28055e8b6e2eda
```

config:

```java
package com.qsl.ggktparent.wechat.config;

import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

/** 读取微信公众号-AppId,appSecret */
@Component // 配置类
public class WXPublicAccount implements InitializingBean {

    @Value("${wechat.mpAppId}")
    private String appid;
    @Value("${wechat.mpAppSecret}")
    private String appSecret;

    public static String ACCESS_KEY_ID;
    public static String  ACCESS_KEY_SECRET;

    @Override
    public void afterPropertiesSet() throws Exception {
        ACCESS_KEY_ID=appid;
        ACCESS_KEY_SECRET=appSecret;
    }
}
```

添加https请求工具类：HttpClientUtils

Controller层：

```java
@Api(tags = "微信公众管理菜单")
@RestController
@RequestMapping("/admin/wechat/menu")
public class MenuController {

    @ApiOperation("获取微信公众号access_token")
    @GetMapping("getAccessToken")
    public Result getAccessToken() {
        try {//get请求微信公众号接口 https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET
            //拼接请求地址
            StringBuffer buffer = new StringBuffer();
            buffer.append("https://api.weixin.qq.com/cgi-bin/token");
            buffer.append("?grant_type=client_credential");
            buffer.append("&appid=%s"); //获取appid
            buffer.append("&secret=%s"); //获取secret
            // 请求路径设置参数:appid,secret
            String url = String.format(buffer.toString(), WXPublicAccount.ACCESS_KEY_ID, WXPublicAccount.ACCESS_KEY_SECRET);
            //  发送http请求
            String tokenString = HttpClientUtils.get(url);
            //   将JSON字符串转为Object
            JSONObject jsonObject = JSONObject.parseObject(tokenString);
            // Json数据中获取access_token
            String access_token = jsonObject.getString("access_token");
            return Result.ok(access_token);
        } catch (Exception e) {
            throw new BusinessException(20001, "获取access_token失败");
        }
    }
}
```

Swagger测试接口即可。。。







## 菜单栏：

   官网文档：https://developers.weixin.qq.com/doc/offiaccount/Custom_Menus/Creating_Custom-Defined_Menu.html

​     前提：必须获取微信获取access_token。

   大概可以分为这几大模块：**首页**、**内容与互动**、**数据**、**广告与服务**、**设置与开发**、**新功能**

​    开发人员: 关注的是设置与开发模块；而作为产品运营人员与数据分析人员，关注的是内容与互动、数据及广告与服务模块。

​    如配置消息回复、自定义菜单、发布文章等：

​        网站嵌入进去公众号菜单里（这里指的是把前端项目的首页链接配置在自定义菜单），并且实现微信端的独立登录认证、获取微信用户信息、微信支付等高级功能，或者觉得UI交互的配置方式无法满足你的需求，那么我们就必须启用开发者模式了，通过技术人员的手段去灵活控制公众号。

微信自定义菜单注意事项：

1. 自定义菜单最多包括3个一级菜单，每个一级菜单最多包含5个二级菜单。
2. 一级菜单最多4个汉字，二级菜单最多8个汉字，多出来的部分将会以“...”代替。
3. 创建自定义菜单后，菜单的刷新策略是，在用户进入公众号会话页或公众号profile页时，如果发现上一次拉取菜单的请求在5分钟以前，就会拉取一下菜单，如果菜单有更新，就会刷新客户端的菜单。测试时可以尝试取消关注公众账号后再次关注，则可以看到创建后的效果。

硅谷课堂自定义菜单：

1. 一级菜单：直播、课程、我的

2. 二级菜单：根据一级菜单动态设置二级菜单，直播（近期直播课程），课程（课程分类），我的（我的订单、我的课程、我的优惠券及关于我们）

   注意：二级菜单可以是网页类型，点击跳转H5页面；也可以是消息类型，点击返回消息。

同步后端菜单栏：

官网文档：https://developers.weixin.qq.com/doc/offiaccount/Custom_Menus/Creating_Custom-Defined_Menu.html

  调用微信接口： http请求方式：POST（请使用https协议） https://api.weixin.qq.com/cgi-bin/menu/create?access_token=ACCESS_TOKEN

 

依赖

```xml
<dependencies>
    <dependency>    <!--    处理微信接口平台-微信公众号    -->
        <groupId>com.github.binarywang</groupId>
        <artifactId>weixin-java-mp</artifactId>
        <version>4.1.0</version>
    </dependency>
</dependencies>
```



结果：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/WeChat/%E5%85%AC%E4%BC%97%E5%8F%B7/%E5%85%AC%E4%BC%97%E5%8F%B7%E8%8F%9C%E5%8D%95%E6%A0%8F.png)



注意：若微信公众号菜单栏不刷新，重新关注即可。

## 公众号消息：

### 概念：

[官网文档,2.1 接受文本消息那](https://developers.weixin.qq.com/doc/offiaccount/Getting_Started/Getting_Started_Guide.html)

接收微信公众号消息的格式：  

```xml
<xml>
    <ToUserName><![CDATA[公众号]]></ToUserName>
    <FromUserName><![CDATA[粉丝号]]></FromUserName>
    <CreateTime>1460537339</CreateTime>
    <MsgType><![CDATA[text]]></MsgType>
    <Content><![CDATA[欢迎开启公众号开发者模式]]></Content>
    <MsgId>6272960105994287618</MsgId>
</xml>
```

| 参数         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| ToUserName   | 开发者微信号                                                 |
| FromUserName | 发送方帐号（一个OpenID）                                     |
| CreateTime   | 消息创建并发送时间 （整型）                                  |
| MsgType      | 消息类型: text(文本消息),news(图文消息)，image(图片消息), voice(语音消息)，video(视频消息)，shortvideo(短视频消息)，location(地理位置消息)，link(链接消息)，==enent(事件消息)。==事件消息一般用于处理用户与公众号之间的各种事件，比如==关注公众号(subscribe)/(unsubscribe)取关事件、(CLICK)菜单点击事件等==。需要根据不同的消息类型进行相应的处理。 |
| Content      | 文本消息内容                                                 |
| MsgId        | 消息id，64位整型                                             |
|              |                                                              |
| Ticket       | 二维码的ticket，可用来换取二维码图片                         |
| EventKey     | 事件KEY值，qrscene_为前缀，后面为二维码的参数值              |
| Event        | 事件类型，subscribe(关注公众号)、unsubscribe(取关)           |
| ArticleCount | 公众号已发布文章的总数(数组)                                 |






消息类型说明:

消息类型：

1. news(图文消息)，可以包括一篇或多篇文章，每篇文章包括标题、描述和图片等信息。 

      注意：图文消息个数：当用户发送文本、图片、语音、视频、图文、地理位置这6种消息时，开发者只能回复1条图文消息；其余场景最多可回复8条图文消息。





公众号报无法提供服务异常:

​      原因： 若服务器不在五秒内处理回复，则必须回复“success”或者“”（空串），否则微信后台会发起三次重试。三次重试后，依旧没有及时回复任何内容，系统自动在粉丝会话界面出现错误提示“该公众号暂时无法提供服务，请稍后再试”。
​    解决方法：如果回复success，微信后台可以确定开发者收到了粉丝消息，没有任何异常提示。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/WeChat/%E5%85%AC%E4%BC%97%E5%8F%B7/%E5%85%AC%E4%BC%97%E5%8F%B7%E6%8A%A5%E6%97%A0%E6%9C%8D%E5%8A%A1%E5%BC%82%E5%B8%B8.png)



关注取消关注：

```xml
<xml>
    <ToUserName><![CDATA[toUser]]></ToUserName>
    <FromUserName><![CDATA[FromUser]]></FromUserName>
    <CreateTime>123456789</CreateTime>
    <MsgType><![CDATA[event]]></MsgType>
    <Event><![CDATA[subscribe]]></Event>
</xml>
```

注意：测试微信接口的时候使用pc或安卓端微信测试，网页测试有问题。。。





###  接受文本消息

  接受文本消息: 即粉丝给公众号发送的文本消息。





普通消息：

​      注意：要实现消息功能，项目必须部署，或使用内网穿透工具模拟部署以获取http请求，提供接口给微信，微信再绑定消息接口。

   本次使用网穿透工具 [cpolar](https://www.cpolar.com/) 绑定gateway网关8333，模拟项目部署,详情看官网教程。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/WeChat/%E5%85%AC%E4%BC%97%E5%8F%B7/%E6%99%AE%E9%80%9A%E6%B6%88%E6%81%AF.png)



 被动回复文本消息格式：

```xml
<xml>
    <ToUserName><![CDATA[粉丝号]]></ToUserName>
    <FromUserName><![CDATA[公众号]]></FromUserName>
    <CreateTime>1460541339</CreateTime>
    <MsgType><![CDATA[text]]></MsgType>
    <Content><![CDATA[test]]></Content>
</xml>
```

```sh
cpolar list # 查看cpolar的connection_id

cpolar stop connection_id # 关闭cpolar隧道
```



### 模板消息：

[模板消息文档: ](https://developers.weixin.qq.com/doc/offiaccount/Message_Management/Template_Message_Operation_Specifications.html)



![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/WeChat/%E5%85%AC%E4%BC%97%E5%8F%B7/%E6%A8%A1%E6%9D%BF%E6%B6%88%E6%81%AF0.png)

Service层：

接口：

```java
public interface MessageService {

    // 模板消息
    void pushPayMessage(Long orderId);
}
```



实现类

```java
import me.chanjar.weixin.mp.api.WxMpService;
import me.chanjar.weixin.mp.bean.template.WxMpTemplateData;
import me.chanjar.weixin.mp.bean.template.WxMpTemplateMessage;

@Slf4j
@Service // bean
public class MessageServiceImpl implements MessageService {

    @Autowired
    private WxMpService wxMpService;

        // TODO 模板消息
    @SneakyThrows // @SneakyThrows: 将方法内抛出的受检查异常转化为不受检查异常并抛出,替代try-catch块
    @Override
    public void pushPayMessage(Long orderId) {
        // 指定发送消息的用户
        String openid = "oQK5P5uTS5BxR2dH3xkcagIfidnA"; //对应公众号用户列表微信号
        WxMpTemplateMessage templateMessage = WxMpTemplateMessage.builder()
                .toUser(openid) // 消息推送到指定用户的id
                .templateId("pt2jq-ReAmvceeRjbyUO7NInxJSfF2imL85MicM19l0")// 模板id
                .url("www.baidu.com" ) //点击模板消息访问的页面  
                .build();

        // 配置要发送的消息:  对应参数，消息，
        templateMessage.addData(new WxMpTemplateData("first", "亲爱的用户：您有一笔订单支付成功。", "#272727"));
        templateMessage.addData(new WxMpTemplateData("keyword1", "java基础课程", "#272727"));
        templateMessage.addData(new WxMpTemplateData("keyword2", "1314520", "#272727"));
        templateMessage.addData(new WxMpTemplateData("keyword3", "150元", "#272727"));
        templateMessage.addData(new WxMpTemplateData("keyword4", "2023-2-5", "#272727"));
        templateMessage.addData(new WxMpTemplateData("remark", "感谢你购买课程，如有疑问，随时咨询！", "#272727"));
        //发送消息
        String msg = wxMpService.getTemplateMsgService().sendTemplateMsg(templateMessage);
        System.out.println(msg);
    }
}
```

Controller层：

```java
@Slf4j
@Api(tags = "微信公众号消息")
@RestController
@RequestMapping("/api/wechat/message") // 对应者微信公众号接口配置
public class MessageController {

    @ApiOperation("模板消息")
    @GetMapping("/pushPayMessage")
    public Result pushPayMessage(){
        messageService.pushPayMessage(1L);
        return Result.ok(null);
    }
```

Seagger测试公众号接收到消息即可。。。



## 网页授权：

​        网页授权获取用户基本信息。。。

scope等于snsapi_userinfo时的授权页面：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/WeChat/%E5%85%AC%E4%BC%97%E5%8F%B7/%E6%8E%88%E6%9D%83%E7%BB%93%E6%9E%9C.png)

参数说明：

| 参数             | 是否必须 | 说明                                                         |
| :--------------- | :------- | :----------------------------------------------------------- |
| appid            | 是       | 公众号的唯一标识                                             |
| redirect_uri     | 是       | 授权后重定向的回调链接地址， 请使用 urlEncode 对链接进行处理 |
| response_type    | 是       | 返回类型，请填写code                                         |
| scope            | 是       | 应用授权作用域：①snsapi_base （不弹出授权页面，直接跳转，只能获取用户openid）， ②snsapi_userinfo(默认) （弹出授权页面，可通过openid拿到昵称、性别、所在地。并且， 即使在未关注的情况下，只要用户授权，也能获取其信息 ） |
| state            | 否       | 重定向后会带上state参数，开发者可以填写a-zA-Z0-9的参数值，最多128字节 |
| #wechat_redirect | 是       | 无论直接打开还是做页面302重定向时候，必须带此参数            |
| forcePopup       | 否       | 强制此次授权需要用户弹窗确认；默认为false；需要注意的是，若用户命中了特殊场景下的静默授权逻辑，则此参数不生效 |

注意：①用户同意授权，页面将跳转至 redirect_uri/?code=CODE&state=STATE。

​         ②code作为换取access_token的票据，每次用户授权带上的code将不一样，code只能使用一次，5分钟未使用自动过期。



**授权错误返回码说明**

| 返回码 | 说明                                         |
| :----- | :------------------------------------------- |
| 10003  | redirect_uri域名与后台配置不一致             |
| 10004  | 此公众号被封禁                               |
| 10005  | 此公众号并没有这些scope的权限                |
| 10006  | 必须关注此测试号                             |
| 10009  | 操作太频繁了，请稍后重试                     |
| 10010  | scope不能为空                                |
| 10011  | redirect_uri不能为空                         |
| 10012  | appid不能为空                                |
| 10013  | state不能为空                                |
| 10015  | 公众号未授权第三方平台，请检查授权状态       |
| 10016  | 不支持微信开放平台的Appid，请使用公众号Appid |



​    注意：由于公众号的secret和获取到的access_token安全级别都非常高，只能保存在服务器，不允许传给客户端。后续刷新access_token、通过access_token获取用户信息等步骤，也必须从服务器发起。



​    公众号和测试公众号任选一个本次测试公众号,我那公众号权限不够。。。

个人公众号：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/WeChat/%E5%85%AC%E4%BC%97%E5%8F%B7/%E5%BE%AE%E4%BF%A1%E6%8E%88%E6%9D%83-%E4%B8%AA%E4%BA%BA%E5%85%AC%E4%BC%97%E5%8F%B7.png)

测试公众号：

本次测试公众号路径：https://mp.weixin.qq.com/debug/cgi-bin/sandboxinfo?action=showinfo&t=sandbox/index 

   SpringCloud可能会把菜单栏页面或授权页面部署在不同的服务器上。。。。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/WeChat/%E5%85%AC%E4%BC%97%E5%8F%B7/%E5%BE%AE%E4%BF%A1%E6%8E%88%E6%9D%83-%E6%B5%8B%E8%AF%95%E5%8F%B7.png)



## 微信支付：

​       [微信支付官网 ](https://pay.weixin.qq.com/)     [支付文档-旧](https://pay.weixin.qq.com/wiki/doc/api/index.html)      [支付文档-新](https://pay.weixin.qq.com/wiki/doc/apiv3/index.shtml)

 支付类型： ①付款码支付，②==JSAPI支付(公众号H5页面)==，③Native支付，④App支付，⑤H5支付，⑥小程序支付，⑦刷脸支付。

​     去微信支付官网申请接口(企业)，直接使用老师的接口,本次采用JSAPI支付。

​     注意：支付功能只能手机端测试，pc端等功能不完整。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/%E5%89%8D%E7%AB%AF/WeChat/%E5%BE%AE%E4%BF%A1%E6%94%AF%E4%BB%98/%E6%94%AF%E4%BB%98%E6%B5%81%E7%A8%8B.png)

依赖：

```xml
<dependency> <!-- 微信支付 -->
    <groupId>com.github.wxpay</groupId>
    <artifactId>wxpay-sdk</artifactId>
    <version>0.0.3</version>
</dependency>
```

​     注意：老师测试号绑定了支付功能，自己申请的没有。 o(╥﹏╥)o，

微信分享：

注意：使用手机测试，其他端测试可能会出现错误问题.



# token：

​     token作用：通过token传递用户信息。

​     token类型：①JWT。
   ​                      ② OAuth2 的 access token 。
​                         ③ SAML 的 assertion 等。

通过localStorage存储token信息

​    注意：HTML5中，新加入localStorage特性，这个特性主要是用来作为本地存储来使用的，解决了cookie存储空间不足的问题(cookie中每条cookie的存储空间很小，只有几K)，localStorage中一般浏览器支持的是5M大小，这个在不同的浏览器中localStorage会有所不同。它只能存储字符串格式的数据，所以最好在每次存储时把数据转换成json格式，取出的时候再转换回来。

## JWT:

​      JWT（Json Web Token）是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准。
​      JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源。比如用在用户登录上
​      JWT最重要的作用就是对 token信息的**防伪**作用。
​     JWT (JSON Web Token) 是一种用于认证和授权的开放标准，通过在服务端生成符合规范的 JSON 格式的 token，可以方便地进行身份验证和资源访问控制。       
   JWT 的作用包括但不限于：提供安全的认证机制、支持跨域传输和状态保持等。

### JWT的原理：

一个JWT由**三个部分组成：公共部分、私有部分、签名部分(加密、编码处理)**。最后由这三者组合进行base64编码得到JWT。

流程图：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/token/JWT/jwt%E6%B5%81%E7%A8%8B.png)

**（1）公共部分**

主要是该JWT的相关配置参数，比如签名的加密算法、格式类型、过期时间等等。

**（2）私有部分**

用户自定义的内容，根据实际需要真正要封装的信息。

userInfo{用户的Id，用户的昵称nickName}

**（3）签名部分**

SaltiP: 当前服务器的Ip地址!{linux 中配置代理服务器的ip}

主要用户对JWT生成字符串的时候，进行加密{盐值}

base64编码，并不是加密，只是把明文信息变成了不可见的字符串。但是其实只要用一些工具就可以把base64编码解成明文，所以不要在JWT中放入涉及私密的信息。

### 整合JWT：

依赖：

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.httpcomponents</groupId>
        <artifactId>httpclient</artifactId>
    </dependency>
    <!--    token-jwt    -->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
    </dependency>
    <dependency>
        <groupId>joda-time</groupId>
        <artifactId>joda-time</artifactId>
    </dependency>
</dependencies>
```

utils/JwtHelper:

```java
import io.jsonwebtoken.*;
import org.springframework.util.StringUtils;

import java.sql.Date;

/*token-jwt*/
public class JwtHelper {
    //token字符串有效时间
    private static long tokenExpiration = 24*60*60*1000;
    //加密编码秘钥
    private static String tokenSignKey = "123456";

    //根据userid  和  username 生成token字符串
    public static String createToken(Long userId, String userName) {
        String token = Jwts.builder()
                //设置token分类
                .setSubject("GGKT-USER")
                //token字符串有效时长
                .setExpiration(new Date(System.currentTimeMillis() + tokenExpiration))
                //私有部分（用户信息）
                .claim("userId", userId)
                .claim("userName", userName)
                //根据秘钥使用加密编码方式进行加密，对字符串压缩
                .signWith(SignatureAlgorithm.HS512, tokenSignKey)
                .compressWith(CompressionCodecs.GZIP)
                .compact();
        return token;
    }

    //从token字符串获取userid
    public static Long getUserId(String token) {
        if(StringUtils.isEmpty(token)) return null;
        Jws<Claims> claimsJws = Jwts.parser().setSigningKey(tokenSignKey).parseClaimsJws(token);
        Claims claims = claimsJws.getBody();
        Integer userId = (Integer)claims.get("userId");
        return userId.longValue();
    }

    //从token字符串获取getUserName
    public static String getUserName(String token) {
        if(StringUtils.isEmpty(token)) return "";
        Jws<Claims> claimsJws
                = Jwts.parser().setSigningKey(tokenSignKey).parseClaimsJws(token);
        Claims claims = claimsJws.getBody();
        return (String)claims.get("userName");
    }

    public static void main(String[] args) {
        String token = JwtHelper.createToken(1L, "lucy");
        System.out.println(token);
        System.out.println(JwtHelper.getUserId(token));
        System.out.println(JwtHelper.getUserName(token));
    }
}
```

Controller:

```java
@RestController
@RequestMapping("/api/user/wechat")
public class WechatController {

    @ApiOperation("微信授权登录")
    @GetMapping("/userInfo")
    public String userInfo(@ApiParam(value = "code", required = true) @RequestParam("code") String code
                           , @ApiParam(value = "state", required = true) @RequestParam("state") String returnUrl)

        ....
        
         // 根据id、用户昵称，生成token
        String token = JwtHelper.createToken(userInfo.getId(), userInfo.getNickName());
    
        ....
        
}
}
```



# cpolar:

## 将隧道配置为后台服务：

路径：C:\Users\GT\.cpolar\cpolar.yml

```yaml
authtoken: xxxxxxxxxxxx #认证token
tunnels:
# 配置硅谷课堂前台域名
  ggktFront:   # 隧道名称 
    proto: http  #协议：http
    addr: "8080"  # 本地端口
    region: cn_vip  #地区：cn_vip，可选:us,hk,cn,cn_vip(中国)
# 配置硅谷课堂前台后台
  ggktEnd:
    proto: http
    addr: "8333"
```

管理员身份打开Developer PowerShell， 关闭并重启cpolar服务 

```sh
# 关闭cpolar服务 
Stop-Service cpolar
```

打开cpolar官网或软件出现隧道端口即可。。。



# 项目部署：

三种部署方式： ①原始部署方式 。
                          ②Jenkins部署 。 
                          ③CODING部署。

​    注意：原始部署方式不适合SpringCloud部署。。。。


## 三种部署方式：

  持续化部署

1.原始部署方式：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/jenkins/%E5%8E%9F%E5%A7%8B%E9%83%A8%E7%BD%B2%E6%96%B9%E5%BC%8F.png)

项目打包后通过java -jar 运行项目


2. Jenkins部署：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/jenkins/dev-ops.png)

3.CODING部署：

​    整合CODING实现DevOps
https://console.cloud.tencent.com/coding/container-devops

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/jenkins/CODING%E6%96%B9%E5%BC%8F%E9%83%A8%E7%BD%B2.svg)

## 安装jenkins：

[Jenkins官网](https://www.jenkins.io/zh/)

前提：①安装JDK       ②安装maven     ③安装Git (**yum  -y  install  git**)  
          ④安装Docker

   本次使用jenkins2.100(2020-03的老技术了o(╥﹏╥)o)，直接使用新版，新版还有中文。

安装docker

```sh
# 安装必要的一些系统工具
yum install -y yum-utils device-mapper-persistent-data lvm2
# 第二步：添加软件源信息
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
 
# 第三步：更新并安装Docker-CE
yum makecache fast
yum -y install docker-ce
 
# 第四步：开启Docker服务
service docker start
# 检查是否安装成功（成功有版本号）
docker -v

# 检查jenkins的版本
java -jar jenkins.war --version

git --version # 检查git是否安装成功（成功有版本号）

```

安装jenkins：

1.linux上传jenkins的**war**包

```sh
# 后台启动jenkins并指定日志路径   nohup：后台启动  
# 日志路径：jenkins.out  
nohup java -jar /opt/jenkins/jenkins.war>/opt/jenkins/jenkins.out &


nohup java -jar /opt/jenkins2.346.3/jenkins.war>/opt/jenkins2.346.3/jenkins.out &

```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/jenkins/%E5%90%AF%E5%8A%A8jenkins.png)

 **3.访问jenkins**：http://192.168.139.140:8080/

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/jenkins/%E7%99%BB%E5%BD%95jenkins2.png)

**4.进入安装插件页面**

​    前提：配置国内镜像,不配90%下载插件失败。

```sh
#进入更新配置位置
cd  /root/.jenkins/updates  

# 修改镜像
sed -i 's/http:\/\/updates.jenkins-ci.org\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g' default.json && sed -i 's/http:\/\/www.google.com/https:\/\/www.baidu.com/g' default.json

# 重启jenkins，运行管理界面，安装插件
ps -ef |grep jenkins 
kill 端口
```

修改镜像成功：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/jenkins/%E9%85%8D%E7%BD%AE%E9%95%9C%E5%83%8F.png)

**4.访问jenkins**：http://192.168.139.140:8080/

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/jenkins/%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6%E5%B9%B6%E5%88%9B%E5%BB%BA%E7%94%A8%E6%88%B7.png)

登录成功：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/jenkins/%E7%99%BB%E5%BD%95%E6%88%90%E5%8A%9F.png)

## 配置自动化部署需要环境：

```sh
which jdk(服务名) # 查询jdk的路径，其他也行。。。
```

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/jenkins/%E9%85%8D%E7%BD%AE%E9%83%A8%E7%BD%B2%E7%8E%AF%E5%A2%83.png)

## Jenkins自动化部署：





```sh

拉取jdk环境
缓存
复制target中的jar包，修改名称为ggktParent.jar
执行jar包（启动项目）


```



## 腾讯云CODING DevOps

**腾讯云使用文档：**

https://help.coding.net/docs/start/new.html

### 概念：

简介ps(Development  Operations）「软件开发人员(Dev)」和「IT 运维技术人员(Ops)」之间沟通合作的文化；旨在透过自动化「软件交付」和「架构变更」的流程，使得构建、 测试、发布软件的过程能够更加地快捷、频繁和可靠。Gartner 咨询公司认为 DevOps 代表了 IT 文化的变化趋势。

DevOps(Development  Operations）「软件开发人员(Dev)」和「IT 运维技术人员(Ops)」之间沟通合作的文化；旨在透过自动化「软件交付」和「架构变更」的流程，使得构建、 测试、发布软件的过程能够更加地快捷、频繁和可靠。Gartner 咨询公司认为 DevOps 代表了 IT 文化的变化趋势。



CODING DevOps 是面向软件研发团队的一站式研发协作管理平台，提供从需求到设计、开发、构建、测试、发布到部署的全流程协同及研发工具支撑。CODING 解决方案可助力企业实现代码的统一安全管控，并快速实践敏捷开发与 DevOps，提升软件交付质量与速度，降低企业研发成本，实现研发效能升级。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/CODING%20DevOps/CODING%20DevOps%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

##### CODING DevOps 优势

- **一站式协作平台及研发工具链，提升研发效能**

   CODING 与云端优势相结合，依托业界敏捷项目管理与 DevOps 体系方法融入到产品中，打通研发过程中的工具链孤岛及协作壁垒，覆盖敏捷开发全生命周期，帮助团队实现需求、迭代、开发、测试、持续集成、持续部署全方位研发管理，提升软件研发效能。

- **支持双态研发体系建设，满足多样化业务需求**

   CODING 适用于不同规模的开发团队以及不同类型的软件开发模式（如瀑布模型、敏捷模型），满足多业务场景的协作需求。

- **项目工作流和度量数据可视化，项目管理更轻松**

   CODING 提供可视化看板，支持对代码、项目进度、人员工作量等不同维度输出详尽的数据报告，为团队管理者提供决策依据，调整项目计划和合理安排研发人力。

- **丰富的扩展能力，无缝集成第三方平台**

   CODING 支持无缝集成 GitHub、GitLab 等第三方代码库及各类常见的运维系统和云原生环境，让用户实现跨平台的无缝迁移。

### 1.3、CODING DevOps 功能特性

**团队级功能：**

- 团队管理：团队管理员通过可视化的[仪表盘](https://help.coding.net/docs/dashboard/use.html)可以快速掌握团队成员工作数据、监控项目运行状态；通过[团队目标](https://help.coding.net/docs/okr/okr.html)助力团队成员聚焦组织目标，全方位协同执行，凝聚团队战斗力，让战略坚实落地；利用[工作负载](https://help.coding.net/docs/workload/intro.html)统一查看对比成员的工作量和工作安排；利用[研发度量](https://help.coding.net/docs/metric/intro.html)统计并分析团队成员在一段时间内的事项分布、事项概览、代码分布等数据，度量团队成员在周期内完成工作量与工作动态。

**项目级功能：**

- [项目协同](https://help.coding.net/docs/collaboration/intro.html)：软件开发团队可自由选择适合的研发管理模式，支持多项目管理、敏捷迭代管理、需求管理、缺陷跟踪、多维度报表数据等功能。
- [代码仓库](https://help.coding.net/docs/collaboration/intro.html)：提供企业级的 Git/SVN 代码管理服务，支持精细化权限管控、多分支并行开发、多版本管理等功能。
- [代码扫描](https://help.coding.net/docs/code-scan/intro.html)：提供针对不同编程语言的代码扫描方案，支持对扫描规则、度量规则等进行自定义配置。根据代码扫描测试结果，开发人员可及时发现代码缺陷并作出修正，有效管控代码质量。
- [持续集成](https://help.coding.net/docs/ci/intro.html)：提供基于云端的自动化代码构建、测试、分析和部署工作流服务，支持通过模板快速创建构建任务并进行可视化编排，极大提高软件开发团队的构建效率。
- [持续部署](https://help.coding.net/docs/cd/overview.html)：提供全自动化软件部署，可持续、可控地把软件制品在线发布到服务集群中，支持蓝绿分发布、灰度发布（金丝雀发布）等多种发布策略。
- [制品管理](https://help.coding.net/docs/artifacts/intro.html)：提供云端构建产物管理服务，支持云端构建和本地构建推送，可快速索引存档构建物、进行版本控制。
- [测试管理](https://help.coding.net/docs/test-management/start.html)：提供面向敏捷团队的测试一站式云端测试平台，支持可视化的测试规划和多维度的测试报告，满足敏捷团队对测试过程的多样化需求。
- [文档管理](https://help.coding.net/docs/document/wiki.html)：提供灵活易用的文档管理服务，可用于记录整个项目的来龙去脉，展示当前项目状态，也可让项目成员更好地进行文档书写及协作。

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/CODING%20DevOps/CODING%20DevOps%E6%B5%81%E7%A8%8B2.png)

使用流程：

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/CODING%20DevOps/%E4%BD%BF%E7%94%A8%E6%B5%81%E7%A8%8B.png)

业务流程:

![](../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2/CODING%20DevOps/%E4%B8%9A%E5%8A%A1%E6%B5%81%E7%A8%8B.png)



### 创建项目:

在 CODING DevOps 平台建立团队之后，团队内成员可按需创建项目。只有项目创建之后，项目成员才能按需使用**项目协同**、**代码仓库**、**持续集成**、**持续部署**等功能。

持续集成：

​     项目代码开发完成之后，可通过[持续集成](https://coding.net/docs/ci/intro.html)功能快速创建构建任务，将项目代码编译打包成软件包。








| 错误码 | 错误描述                                                 |
| :----- | :------------------------------------------------------- |
| 1000   | 未知错误                                                 |
| 1001   | 参数错误                                                 |
| 1002   | 签名错误                                                 |
| 1003   | 时间戳过期                                               |
| 1004   | 非法请求                                                 |
| 1005   | APPID不存在                                              |
| 1006   | 用户不存在                                               |
| 1007   | 视频不存在                                               |
| 1008   | 任务不存在                                               |
| 1009   | 视频文件不存在                                           |
| 1010   | 视频文件格式不支持                                       |
| 1011   | 视频文件大小超过限制                                     |
| 1012   | 视频文件时长超过限制                                     |
| 1013   | 视频文件分片数量超过限制                                 |
| 1014   | 视频文件分片格式不支持                                   |
| 1015   | 视频文件分片大小超过限制                                 |
| 1016   | 视频文件分片上传失败                                     |
| 1017   | 视频文件分片上传完成失败                                 |
| 1018   | 视频文件分片上传取消失败                                 |
| 1019   | 视频文件分片上传暂停失败                                 |
| 1020   | 视频文件分片上传续传失败                                 |
| 1021   | 视频文件分片上传查询失败                                 |
| 1022   | 视频文件分片上传合并失败                                 |
| 1023   | 视频文件分片上传删除失败                                 |
| 1024   | 视频文件分片上传列表查询失败                             |
| 1025   | 视频文件分片上传列表删除失败                             |
| 1026   | 视频文件分片上传列表暂停失败                             |
| 1027   | 视频文件分片上传列表续传失败                             |
| 1028   | 视频文件分片上传列表查询失败                             |
| 1029   | 视频文件分片上传列表合并失败                             |
| 1030   | 视频文件分片上传列表取消失败                             |
| 1031   | 视频文件上传失败                                         |
| 1032   | 视频文件上传完成失败                                     |
| 1033   | 视频文件上传取消失败                                     |
| 1034   | 视频文件上传暂停失败                                     |
| 1035   | 视频文件上传续传失败                                     |
| 1036   | 视频文件上传查询失败                                     |
| 1037   | 视频文件上传删除失败                                     |
| 1038   | 视频文件上传列表查询失败                                 |
| 1039   | 视频文件上传列表删除失败                                 |
| 1040   | 视频文件上传列表暂停失败                                 |
| 1041   | 视频文件上传列表续传失败                                 |
| 1042   | 视频文件上传列表查询失败                                 |
| 1043   | 视频文件上传列表取消失败                                 |
| 1044   | 视频文件上传列表合并失败                                 |
| 1045   | 视频文件上传分片数量不足                                 |
| 1046   | 视频文件上传分片数量超过限制                             |
| 1047   | 视频文件上传分片大小不足                                 |
| 1048   | 视频文件上传分片大小超过限制                             |
| 1049   | 视频文件上传分片格式不支持                               |
| 1050   | 视频文件上传分片上传失败                                 |
| 1051   | 视频文件上传分片上传完成失败                             |
| 1052   | 视频文件上传分片上传取消失败                             |
| 1053   | 视频文件上传分片上传暂停失败                             |
| 1054   | 视频文件上传分片上传续传失败                             |
| 1055   | 视频文件上传分片上传查询失败                             |
| 1056   | 视频文件上传分片上传删除失败                             |
| 1057   | 视频文件上传分片上传列表查询失败                         |
| 1058   | 视频文件上传分片上传列表删除失败                         |
| 1059   | 视频文件上传分片上传列表暂停失败                         |
| 1060   | 视频文件上传分片上传列表续传失败                         |
| 1061   | 视频文件上传分片上传列表查询失败                         |
| 1062   | 视频文件上传分片上传列表取消失败                         |
| 1063   | 视频文件上传分片上传列表合并失败                         |
| 1064   | 视频文件上传分片上传列表上传失败                         |
| 1065   | 视频文件上传分片上传列表上传完成失败                     |
| 1066   | 视频文件上传分片上传列表上传取消失败                     |
| 1067   | 视频文件上传分片上传列表上传暂停失败                     |
| 1068   | 视频文件上传分片上传列表上传续传失败                     |
| 1069   | 视频文件上传分片上传列表上传查询失败                     |
| 1070   | 视频文件上传分片上传列表上传删除失败                     |
| 1071   | 视频文件上传分片上传列表上传列表查询失败                 |
| 1072   | 视频文件上传分片上传列表上传列表删除失败                 |
| 1073   | 视频文件上传分片上传列表上传列表暂停失败                 |
| 1074   | 视频文件上传分片上传列表上传列表续传失败                 |
| 1075   | 视频文件上传分片上传列表上传列表查询失败                 |
| 1076   | 视频文件上传分片上传列表上传列表取消失败                 |
| 1077   | 视频文件上传分片上传列表上传列表合并失败                 |
| 1078   | 视频文件上传分片上传列表上传列表上传失败                 |
| 1079   | 视频文件上传分片上传列表上传列表上传完成失败             |
| 1080   | 视频文件上传分片上传列表上传列表上传取消失败             |
| 1081   | 视频文件上传分片上传列表上传列表上传暂停失败             |
| 1082   | 视频文件上传分片上传列表上传列表上传续传失败             |
| 1083   | 视频文件上传分片上传列表上传列表上传查询失败             |
| 1084   | 视频文件上传分片上传列表上传列表上传删除失败             |
| 1085   | 视频文件上传分片上传列表上传列表列表查询失败             |
| 1086   | 视频文件上传分片上传列表上传列表列表删除失败             |
| 1087   | 视频文件上传分片上传列表上传列表列表暂停失败             |
| 1088   | 视频文件上传分片上传列表上传列表列表续传失败             |
| 1089   | 视频文件上传分片上传列表上传列表列表查询失败             |
| 1090   | 视频文件上传分片上传列表上传列表列表取消失败             |
| 1091   | 视频文件上传分片上传列表上传列表列表合并失败             |
| 1092   | 视频文件上传分片上传列表上传列表列表上传失败             |
| 1093   | 视频文件上传分片上传列表上传列表列表上传完成失败         |
| 1094   | 视频文件上传分片上传列表上传列表列表上传取消失败         |
| 1095   | 视频文件上传分片上传列表上传列表列表上传暂停失败         |
| 1096   | 视频文件上传分片上传列表上传列表列表上传续传失败         |
| 1097   | 视频文件上传分片上传列表上传列表列表上传查询失败         |
| 1098   | 视频文件上传分片上传列表上传列表列表上传删除失败         |
| 1099   | 视频文件上传分片上传列表上传列表列表列表查询失败         |
| 1100   | 视频文件上传分片上传列表上传列表列表列表删除失败         |
| 1101   | 视频文件上传分片上传列表上传列表列表列表暂停失败         |
| 1102   | 视频文件上传分片上传列表上传列表列表列表续传失败         |
| 1103   | 视频文件上传分片上传列表上传列表列表列表查询失败         |
| 1104   | 视频文件上传分片上传列表上传列表列表列表取消失败         |
| 1105   | 视频文件上传分片上传列表上传列表列表列表合并失败         |
| 1106   | 视频文件上传分片上传列表上传列表列表列表上传失败         |
| 1107   | 视频文件上传分片上传列表上传列表列表列表上传完成失败     |
| 1108   | 视频文件上传分片上传列表上传列表列表列表上传取消失败     |
| 1109   | 视频文件上传分片上传列表上传列表列表列表上传暂停失败     |
| 1110   | 视频文件上传分片上传列表上传列表列表列表上传续传失败     |
| 1111   | 视频文件上传分片上传列表上传列表列表列表上传查询失败     |
| 1112   | 视频文件上传分片上传列表上传列表列表列表上传删除失败     |
| 1113   | 视频文件上传分片上传列表上传列表列表列表列表查询失败     |
| 1114   | 视频文件上传分片上传列表上传列表列表列表列表删除失败     |
| 1115   | 视频文件上传分片上传列表上传列表列表列表列表暂停失败     |
| 1116   | 视频文件上传分片上传列表上传列表列表列表列表续传失败     |
| 1117   | 视频文件上传分片上传列表上传列表列表列表列表查询失败     |
| 1118   | 视频文件上传分片上传列表上传列表列表列表列表取消失败     |
| 1119   | 视频文件上传分片上传列表上传列表列表列表列表合并失败     |
| 1120   | 视频文件上传分片上传列表上传列表列表列表列表上传失败     |
| 1121   | 视频文件上传分片上传列表上传列表列表列表列表上传完成失败 |
| 1122   | 视频文件上传分片上传列表上传列表列表列表列表上传取消失败 |
| 1123   | 视频文件上传分片上传列表上传列表列表列表列表上传暂停失败 |
| 1124   | 视频文件上传分片上传列表上传列表列表列表列表上传续传失败 |
| 1125   | 视频文件上传分片上传列表上传列表列表列表列表上传查询失败 |
| 1126   | 视频文件上传分片上传列表上传列表列表列表列表上传删除失败 |
| 1127   | 视频文件上传分片上传列表上传列表列表列表列表列表查询失败 |
| 1128   | 视频文件上传分片上传列表上传列表列表列表列表列表删除失败 |
| 1129   | 视频文件上传分片上传列表上传列表列表列表列表列表暂停失败 |
| 1130   | 视频文件上传分片上传列表上传列表列表列表列表列表续传失败 |
| 1131   | 视频文件上传分片上传列表上传列表列表列表列表列表查询失败 |
| 1132   | 视频文件上传分片上传列表上传列表列表列表列表列表取消失败 |
| 1133   | 视频文件上传分片上传列表上传列表列表列表列表列表合并失败 |
| 1134   | 视频文件上传分片上传列表上传列表列表列表列表列表上传失败 |
| 1135   | 视频文件                                                 |



谷粒学院。。。。。。
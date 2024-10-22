
# Java 当中使用 “google.zxing ”开源项目 和 “github 的 qrcode\-plugin” 开源项目 生成二维码


@

目录* [Java 当中使用 “google.zxing ”开源项目 和 “github 的 qrcode\-plugin” 开源项目 生成二维码](https://github.com)
* [1\. Java当中使用 “google.zxing ” 开源项目生成二维码](https://github.com)
	+ [1\.1 准备工作](https://github.com)
	+ [1\.2 生成黑白二维码](https://github.com)
		- [1\.2\.1 zxing 常用 API](https://github.com)
			* [1\.2\.1\.1 EncodeHintType (编码提示类型)](https://github.com)
			* [1\.2\.1\.2 MultiFormatWriter（多格式写入程序）](https://github.com)
			* [1\.2\.1\.3 BarcodeFormat（码格式）](https://github.com)
			* [1\.2\.1\.4 BitMatrix（位矩阵）](https://github.com)
		- [1\.2\.2 将 url 生成黑白二维码](https://github.com)
	+ [1\.3 生成一个带 logo 的黑白二维码](https://github.com):[milou加速器](https://jiechuangmoxing.com)
* [2\. Java当中使用 “github 的 qrcode\-plugin” 开源项目生成二维码](https://github.com)
	+ [2\.1 准备工作：](https://github.com)
	+ [2\.2 生成黑白二维码](https://github.com)
	+ [2\.3 生成带有 Logo 的二维码](https://github.com)
	+ [2\.4 生成彩色的二维码](https://github.com)
	+ [2\.5 背景图的二维码](https://github.com)
	+ [2\.6 特殊形状二维码](https://github.com)
	+ [2\.7 图片填充二维码](https://github.com)
	+ [2\.8 生成 gif 动图二维码](https://github.com)



---


# 1\. Java当中使用 “google.zxing ” 开源项目生成二维码


这里我们使用 servlet 和 tomcat 技术进行操作演示。


## 1\.1 准备工作


首先在 `pom.xml` 文件当中导入相应所需的依赖。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158556-860167833.png)



```
<dependencies>
        
        <dependency>
            <groupId>jakarta.servletgroupId>
            <artifactId>jakarta.servlet-apiartifactId>
            <version>5.0.0version>
            <scope>providedscope>
        dependency>

        
        <dependency>
            <groupId>com.google.zxinggroupId>
            <artifactId>coreartifactId>
            <version>3.1.0version>
        dependency>
        <dependency>
            <groupId>com.google.zxinggroupId>
            <artifactId>javaseartifactId>
            <version>3.1.0version>
        dependency>

        
        <dependency>
            <groupId>commons-langgroupId>
            <artifactId>commons-langartifactId>
            <version>2.6version>
        dependency>
    dependencies>

```

`pom.xml` 完整的配置内容。



```
xml version="1.0" encoding="UTF-8"?
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0modelVersion>

    <groupId>com.rainbowseagroupId>
    <artifactId>zxingQRcodeartifactId>
    <version>1.0-SNAPSHOTversion>
    <packaging>warpackaging>

    <dependencies>
        
        <dependency>
            <groupId>jakarta.servletgroupId>
            <artifactId>jakarta.servlet-apiartifactId>
            <version>5.0.0version>
            <scope>providedscope>
        dependency>

        
        <dependency>
            <groupId>com.google.zxinggroupId>
            <artifactId>coreartifactId>
            <version>3.1.0version>
        dependency>
        <dependency>
            <groupId>com.google.zxinggroupId>
            <artifactId>javaseartifactId>
            <version>3.1.0version>
        dependency>

        
        <dependency>
            <groupId>commons-langgroupId>
            <artifactId>commons-langartifactId>
            <version>2.6version>
        dependency>
    dependencies>

    <properties>
        <maven.compiler.source>1.8maven.compiler.source>
        <maven.compiler.target>1.8maven.compiler.target>
        <project.build.sourceEncoding>UTF-8project.build.sourceEncoding>
    properties>

project>

```

注意：我们是使用的是 tomcat 和 servlet 我们需要注意 `web` 的根路径是否正确，如果错误的话，是无法找到对应资源的。


对应的前端显示的页面代码的编写内容，这里我们使用 `jsp` 作为前端页面的展示。**注意：前端页面所放的路径是在 `web` 目录下的** 。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158610-1325214148.png)



```
<%@ page contentType="text/html; charset=UTF-8" language="java" %>


"en" >

    "UTF-8"/>
    生成普通黑白二维码


url："text" id="url">


---





```

## 1\.2 生成黑白二维码


### 1\.2\.1 zxing 常用 API


#### 1\.2\.1\.1 EncodeHintType (编码提示类型)


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158593-2131576587.png)


EncodeHintType 是用来设置二维码编码时的一些额外参数的枚举类型，常用枚举值如下：


1. `ERROR_CORRECTION`：
	1. **误差校正级别** 。对于黑白二维码，可选值 `L(7%)`，`M(15%)`，`Q(25%)`，`H(30%)`，表示二维码允许破损的最大容错率。在二维码出现破损时，根据设置的容错率级别，可以尝试设置修复二维码中的一些数据。
	2. 二维码在生成过程中，可能会出现一些破损或者是缺失的情况，例如：“打印时墨水耗尽，图像压缩，摄像头拍摄角度不对”等。这些问题可能导致二维码无法完全识别，或者识别出来的数据不准确，而误差校正码就是为了解决这些问题而产生的。
	3. 例如：选择`L` 级别的容错率，相当于允许在二维码的整体颜色区域中，最多有约 `7%` 的坏像素点；而选择 `H` 级别的容错率时，最多可有 `30%` 的坏像素点。
2. `CHARACTER_SET`：表示编码字符集，可以设置使用的字符编码，例如：utf\-8，ag2312等等。
3. `MARGIN`：二维码的空白区域大小，可以设置二维码周围的留白大小，以便于在不同的嵌入场景中使用二维码。


#### 1\.2\.1\.2 MultiFormatWriter（多格式写入程序）


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158629-1083057783.png)


`MultiFormatWriter` 是一个便捷的二维码生成类，可以根据传入的 `BarcodeFormat` 参数，生成对应类型的二维码。


`MultiFormatWriter` 封装了一系列的二维码生成方法，可以生成多种格式的二维码，包括：“QR Code、Aztec Code、PDF417、Data Matrix”等。


#### 1\.2\.1\.3 BarcodeFormat（码格式）


**BarcodeFormat 是枚举类，通过它来制定二维码格式：**


1. `QR Code` ：QR Code 是最常见的二维码格式之一，广泛应用于商品包装，票务，扫码支付等领域。QR Code 矩阵有黑白两种颜色，其中黑色部分表示信息的编码，白![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158584-1350458086.png)
色部分，则用于衬托和辨识。
2. `Aztec Code`：Aztec Code 是一种高密度，可靠性很高的二维码格式。相比于其他二维码格式，它具有更低的容错率，更小的尺寸和更高的解码效率。因此，它适合用于存储一些核心信息，例如：个人信息，证件信息，账户密码等。
3. `PDF417`：是一种可以存储大量信息的二维码格式，它具有数据密度高，可靠性强等优点，可以应用于许多场景，例如：航空机票，运输和配送标签，法律文件等。
4. `Data Matrix`：是一种小巧的二维码格式，它的编码方式类似于 QR Code，但是其可靠性，识别率，扫描速度和牢固度都比 QR Code 更优秀。由于尺寸较小，可靠新较高，因此 Data Matrix 适合嵌入简单的产品标签，医疗图像，检测数据等领域。


#### 1\.2\.1\.4 BitMatrix（位矩阵）


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158615-1873066095.png)


`BitMatrix` 是 ZXing 库中表示二维码矩阵的数据结构，它是由 0 和 1 构成的二维数组，用于存储二维码的编码信息。在二维码生成过程中，我们通过对 `BitMatrix` 对象的构建和操作，最终生成一个可被扫描解码的二维码图像。


`BitMatrix` 实际上是一个**紧凑型的布尔型二维数组** ，往往只需要占用一个字节即可表示 `S` 位二进制。在使用 `BitMatrix` 时，我们可以通过其不同的方法，例如：`get()`，`set()` 等，来获取，设置矩阵中每个位置的值。


在 ZXing中，`BitMatrix` 常用于将编码后的信息转化为矩阵形式，并进行图像的生成和输出。在使用 ZXing 生成二维码时，我们首先需要使用`MultiFormatWriter.encode()`


方法来生成一个 `BitMatrix` ；然后，在对`BitMatrix` 进行各种处理和操作后，就可以在 UI中显示和输出二维码。


总的来说，`BitMatrix` 是ZXing 库中非常重要的数据结构之一，它负责存储和处理生成二维码图像所需的二进制信息，是实现二维码生成功能的关键。


**BitMatrix 常用 API：**


* `getHeight()`： 获取矩阵高度
* `getWidth()`：获取矩阵宽度
* `get(x,y)`：根据 "x，y" 的坐标获取矩阵中该坐标的值。结果是 **`true(黑色)` 或者是 `false(白色)`** 。


### 1\.2\.2 将 url 生成黑白二维码


前端代码：


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158655-1673832106.png)



```
<%@ page contentType="text/html; charset=UTF-8" language="java" %>


"en" >

    "UTF-8"/>
    生成普通黑白二维码


url："text" id="url">


---


<%--请输入文本内容:--%>
<%--"url" cols="60" rows="20">--%>


![]()"image"/>





```

后端代码：


定义为一个 Servlet 进行处理前端的请求，返回一个二维码。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158684-1479337512.png)


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158577-2104623123.png)



```
package com.rainbowsea.servlets;

import com.google.zxing.BarcodeFormat;
import com.google.zxing.EncodeHintType;
import com.google.zxing.MultiFormatReader;
import com.google.zxing.MultiFormatWriter;
import com.google.zxing.WriterException;
import com.google.zxing.common.BitMatrix;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletOutputStream;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;


@WebServlet("/create")
public class GenerateQRcode extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 使用谷歌提供的 zxing开源库，生成普通的黑白二维码(核心代码)


        try {
            // 需要创建一个Map集合，用这个 Map集合存储二维码相关的属性(参数)
            Map map = new HashMap();
            // 设置二维码的误差校正级别
            map.put(EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.H);
            // 设置二维码的字符集
            map.put(EncodeHintType.CHARACTER_SET, "utf-8");
            // 设置二维码四周的留白
            map.put(EncodeHintType.MARGIN, 1);

            // 创建 zxing的核心对象， MultiFormatWriter （多格式写入器）
            // 通过 MultiFormatWriter 对象来生成二维码
            MultiFormatWriter writer = new MultiFormatWriter();

            // 获取文本内容
            String url = request.getParameter("url");
            //writer.encode(内容，什么格式的二维码，二维码的宽度，二维码的高度，二维码的参数)
            // 位矩阵对象
            BitMatrix bitMatrix = writer.encode(url, BarcodeFormat.QR_CODE, 300, 300, map);

            // 获取该位矩阵的宽度
            int width = bitMatrix.getWidth();
            // 获取该位矩阵的高度
            int height = bitMatrix.getHeight();

            // 生成二维码的图片
            BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);

            // 编写一个嵌套的循环，遍历二维数组的一个循环，遍历位矩阵对象
            for (int x = 0; x < width; x++) {
                for (int y = 0; y < height; y++) {
                    // 0xFF000000 : 0xFFFFFFFF 表示 黑白的十六进制
                    image.setRGB(x, y, bitMatrix.get(x, y) ? 0xFF000000 : 0xFFFFFFFF);
                }
            }

            // 将图片响应到浏览器客户端上
            ServletOutputStream out = response.getOutputStream();
            ImageIO.write(image,"png",out);
            out.flush();
            out.close();

        } catch (WriterException e) {
            e.printStackTrace();
        }

    }
}


```

运行显示效果：


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158563-1276991922.png)



> 我们也可以生成一个以 **文本内容** 生成的一个二维码。如下：修改一下前端的提交的格式内容即可。
> 
> 
> ![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158683-1251771682.png)



> ```
> <%@ page contentType="text/html; charset=UTF-8" language="java" %>
> 
> 
> "en" >
> 
>     "UTF-8"/>
>     生成普通黑白二维码
> 
> 
> <%--url："text" id="url">--%>
> 
> 
> ---
> 
> 
> 请输入文本内容:
> "url" cols="60" rows="20">
> 
> 
> ![]()"image"/>
> 
> 
> 
> 
> 
> ```
> 
> 后端代码 Servlet 不动，这样。
> 
> 
> 运行效果如下：
> 
> 
> ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/90ece4ab176d4d96bad7b36e83e8f2e9.png)


## 1\.3 生成一个带 logo 的黑白二维码


前端的页面展示的代码处理：


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158664-1199840551.png)



```
<%@ page contentType="text/html; charset=UTF-8" language="java" %>


"en" >

    "UTF-8"/>
    生成Log二维码


<%-- 大数据用: multipart/form-data--%>
"/zxingQRcode/generateWithLogon" method="post" enctype="multipart/form-data">
    请输入url: "text" name="url">
    请选择图片: "file" name="logo">
    "submit" value="生成带Logo的二维码">






```

后端 Servlet 代码的内容的编写，如下：


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158704-921200856.png)



```
package com.rainbowsea.servlets;


import com.google.zxing.BarcodeFormat;
import com.google.zxing.EncodeHintType;
import com.google.zxing.MultiFormatWriter;
import com.google.zxing.WriterException;
import com.google.zxing.common.BitMatrix;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletOutputStream;
import jakarta.servlet.annotation.MultipartConfig;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.Part;

import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.geom.RoundRectangle2D;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

@WebServlet("/generateWithLogon")
// 设置传输文件的最大值和最小值
@MultipartConfig(fileSizeThreshold = 1024 * 1024 * 2, maxFileSize = 1024 * 1024 * 10, maxRequestSize = 1024 * 1024 * 100)
public class GerateQRcodeWithLogon extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException,
            IOException {
        // 使用谷歌提供的 zxing开源库，生成普通的黑白二维码(核心代码)


        try {
            // 需要创建一个Map集合，用这个 Map集合存储二维码相关的属性(参数)
            Map map = new HashMap();
            // 设置二维码的误差校正级别
            map.put(EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.H);
            // 设置二维码的字符集
            map.put(EncodeHintType.CHARACTER_SET, "utf-8");
            // 设置二维码四周的留白
            map.put(EncodeHintType.MARGIN, 1);

            // 创建 zxing的核心对象， MultiFormatWriter （多格式写入器）
            // 通过 MultiFormatWriter 对象来生成二维码
            MultiFormatWriter writer = new MultiFormatWriter();

            // 获取文本内容
            String url = request.getParameter("url");
            //writer.encode(内容，什么格式的二维码，二维码的宽度，二维码的高度，二维码的参数)
            // 位矩阵对象
            BitMatrix bitMatrix = writer.encode(url, BarcodeFormat.QR_CODE, 300, 300, map);

            // 获取该位矩阵的宽度
            int width = bitMatrix.getWidth();
            // 获取该位矩阵的高度
            int height = bitMatrix.getHeight();

            // 生成二维码的图片
            BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);

            // 编写一个嵌套的循环，遍历二维数组的一个循环，遍历位矩阵对象
            for (int x = 0; x < width; x++) {
                for (int y = 0; y < height; y++) {
                    // 0xFF000000 : 0xFFFFFFFF 表示 黑白的十六进制
                    image.setRGB(x, y, bitMatrix.get(x, y) ? 0xFF000000 : 0xFFFFFFFF);
                }
            }

            // 给二维码添加Logo
            // 第一部分: 将Logo缩放
            Part logoPart = request.getPart("logo");
            InputStream inputStream = logoPart.getInputStream();
            BufferedImage logoImage = ImageIO.read(inputStream);

            // 对获取到的logo图片进行缩放
            int logoWidth = logoImage.getWidth(null);
            int logoHeight = logoImage.getHeight(null);
            // 对 logo 图片的大小进行一个缩放，整理
            if (logoWidth > 60) {
                logoWidth = 60;
            }

            if (logoHeight > 60) {
                logoHeight = 60;
            }

            // 这一行代码非常重要，全靠它来进行缩放了
            // 使用平滑缩放算法对原始的Logo图像进行缩放得到一个全新的图像
            Image scaledLogo = logoImage.getScaledInstance(logoWidth, logoHeight, Image.SCALE_SMOOTH);

            // 第二部分: 将缩放后的Logo画到黑白二维码上
            // 获取一个2D的画笔
            Graphics2D graphics2D = image.createGraphics();
            // 指定 logo 图片从哪里开始，也就是指定开始的坐标 x,y
            int x = (300 - logoWidth) / 2;
            int y = (300 - logoHeight) / 2;

            // 将缩放之后的logo画上去
            // 第一个参数是：logo缩放后的图片，二三参数是:坐标，第四个参数是: null
            graphics2D.drawImage(scaledLogo,x,y,null);
            // 创建一个具有指定位置，宽度，高度和圆角半径的圆角矩形，这个圆角矩形是用来绘制边框的
            Shape shape = new RoundRectangle2D.Float(x, y, logoWidth, logoHeight, 10, 10);
            // 使用一个宽度为 4 像素的基本笔触
            graphics2D.setStroke(new BasicStroke(4f));
            // 给 logo 画圆角矩形
            graphics2D.draw(shape);

            // 释放画笔
            graphics2D.dispose();

            // 将二维码图片响应到浏览器上
            ImageIO.write(image, "png", response.getOutputStream());

        } catch (WriterException e) {
            e.printStackTrace();
        }


    }
}


```

**运行效果**
![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158612-253968653.png)


# 2\. Java当中使用 “github 的 qrcode\-plugin” 开源项目生成二维码


github 当中的 "qrcode\-plugin" 开源项目，比较友好，只需要简单调用接口。


对应的 xml 文件依赖如下：



```
<dependency>
    <groupId>com.github.liuyueyi.mediagroupId>
    <artifactId>qrcode-pluginartifactId>
    <version>2.5.2version>
dependency>

```

## 2\.1 准备工作：


首先在 `pom.xml` 文件当中导入相应所需的依赖。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158586-1237538991.png)



```

    <dependencies>
        
        <dependency>
            <groupId>jakarta.servletgroupId>
            <artifactId>jakarta.servlet-apiartifactId>
            <version>5.0.0version>
            <scope>providedscope>
        dependency>

        
        <dependency>
            <groupId>commons-langgroupId>
            <artifactId>commons-langartifactId>
            <version>2.6version>
        dependency>


        <dependency>
            <groupId>com.github.liuyueyi.mediagroupId>
            <artifactId>qrcode-pluginartifactId>
            <version>2.5.2version>
        dependency>

```

`pom.xml` 完整的配置内容 。



```
xml version="1.0" encoding="UTF-8"?
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0modelVersion>

    <groupId>com.rainbowseagroupId>
    <artifactId>githubliuyueyiartifactId>
    <version>1.0-SNAPSHOTversion>
    <packaging>warpackaging>

    <dependencies>
        
        <dependency>
            <groupId>jakarta.servletgroupId>
            <artifactId>jakarta.servlet-apiartifactId>
            <version>5.0.0version>
            <scope>providedscope>
        dependency>

        
        <dependency>
            <groupId>commons-langgroupId>
            <artifactId>commons-langartifactId>
            <version>2.6version>
        dependency>


        <dependency>
            <groupId>com.github.liuyueyi.mediagroupId>
            <artifactId>qrcode-pluginartifactId>
            <version>2.5.2version>
        dependency>
    dependencies>

project>

```

注意：我们是使用的是 tomcat 和 servlet 我们需要注意 `web` 的根路径是否正确，如果错误的话，是无法找到对应资源的。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158637-2081386603.png)


对应的前端显示的页面代码的编写内容，这里我们使用 `jsp` 作为前端页面的展示。**注意：前端页面所放的路径是在 `web` 目录下的** 。


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158564-1389787774.png)



```
<%@ page contentType="text/html; charset=UTF-8" language="java" %>


"en">

    "UTF-8"/>
    github 开源项目生成酷炫二维码


"/githubliuyueyi/generateWithLogon" method="post" enctype="multipart/form-data">
    请输入url: "text" name="url">
    请选择图片: "file" name="logo">
    "submit" value="生成二维码">





```

## 2\.2 生成黑白二维码


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158619-1966356165.png)



```

import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeDeWrapper;
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeGenWrapper;
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeOptions;
import com.google.zxing.WriterException;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.MultipartConfig;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;

@WebServlet("/generateWithLogon")
// 设置传输文件的最大值和最小值
@MultipartConfig(fileSizeThreshold = 1024 * 1024 * 2,
        maxFileSize = 1024 * 1024 * 10,
        maxRequestSize = 1024 * 1024 * 100)
public class GenerateWithQrCode extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        try {
            String url = request.getParameter("url");

            // 生成黑白二维码
            BufferedImage image = QrCodeGenWrapper.of(url)
                    .asBufferedImage();
            // 将二维码图片响应到浏览器
            ImageIO.write(image, "png", response.getOutputStream());


        } catch (WriterException e) {
            e.printStackTrace();
        }

    }
}


```

运行显示效果：


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158623-2029083144.png)


## 2\.3 生成带有 Logo 的二维码


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158707-450296723.png)



```

import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeDeWrapper;
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeGenWrapper;
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeOptions;
import com.google.zxing.WriterException;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.MultipartConfig;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;

@WebServlet("/generateWithLogon")
// 设置传输文件的最大值和最小值
@MultipartConfig(fileSizeThreshold = 1024 * 1024 * 2,
        maxFileSize = 1024 * 1024 * 10,
        maxRequestSize = 1024 * 1024 * 100)
public class GenerateWithQrCode extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        try {
            String url = request.getParameter("url");


            // 生成带有 logo 的黑白二维码
            BufferedImage image = QrCodeGenWrapper.of(url)
                    .setLogo(request.getPart("logo").getInputStream())
                    .setLogoRate(7) // 设置Logo图片与二维码之间的比例，7表示Logo的宽度等于二维码的 1/7
                    .setLogoStyle(QrCodeOptions.LogoStyle.ROUND) // 设置logo图片的样式，将logo的边框形状设置为圆锐角
                    .asBufferedImage();
            
            // 将二维码图片响应到浏览器
            ImageIO.write(image, "png", response.getOutputStream());


        } catch (WriterException e) {
            e.printStackTrace();
        }

    }
}


```

运行效果：


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158686-400521902.png)


## 2\.4 生成彩色的二维码


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158651-473120289.png)



```
package com.rainbowsea.servlets;

import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeDeWrapper;
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeGenWrapper;
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeOptions;
import com.google.zxing.WriterException;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.MultipartConfig;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;

@WebServlet("/generateWithLogon")
// 设置传输文件的最大值和最小值
@MultipartConfig(fileSizeThreshold = 1024 * 1024 * 2,
        maxFileSize = 1024 * 1024 * 10,
        maxRequestSize = 1024 * 1024 * 100)
public class GenerateWithQrCode extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        try {
            String url = request.getParameter("url");

            BufferedImage image = QrCodeGenWrapper.of(url)
                    .setDrawPreColor(Color.BLUE)
                    .asBufferedImage();

            ImageIO.write(image, "png", response.getOutputStream());


        } catch (WriterException e) {
            e.printStackTrace();
        }

    }
}


```

![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158673-49499416.png)


## 2\.5 背景图的二维码


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158663-213939730.png)



```
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeDeWrapper;
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeGenWrapper;
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeOptions;
import com.google.zxing.WriterException;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.MultipartConfig;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;

@WebServlet("/generateWithLogon")
// 设置传输文件的最大值和最小值
@MultipartConfig(fileSizeThreshold = 1024 * 1024 * 2,
        maxFileSize = 1024 * 1024 * 10,
        maxRequestSize = 1024 * 1024 * 100)
public class GenerateWithQrCode extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        try {
            String url = request.getParameter("url");
            // 生成带有背景图的二维码
            BufferedImage image = QrCodeGenWrapper.of(url)
                    .setBgImg(request.getPart("logo").getInputStream())
                    .setBgOpacity(0.5F)  // 设置背景图片的透明度
                    .asBufferedImage();

            
            // 将二维码图片响应到浏览器
            ImageIO.write(image, "png", response.getOutputStream());


        } catch (WriterException e) {
            e.printStackTrace();
        }

    }
}


```

运行效果：


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158676-764083076.png)


## 2\.6 特殊形状二维码


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158587-1762730830.png)


运行效果：


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158652-1091735098.png)


## 2\.7 图片填充二维码


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158554-411604624.png)



```
package com.rainbowsea.servlets;

import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeDeWrapper;
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeGenWrapper;
import com.github.hui.quick.plugin.qrcode.wrapper.QrCodeOptions;
import com.google.zxing.WriterException;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.MultipartConfig;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;

@WebServlet("/generateWithLogon")
// 设置传输文件的最大值和最小值
@MultipartConfig(fileSizeThreshold = 1024 * 1024 * 2,
        maxFileSize = 1024 * 1024 * 10,
        maxRequestSize = 1024 * 1024 * 100)
public class GenerateWithQrCode extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        try {
            String url = request.getParameter("url");

            BufferedImage image = QrCodeGenWrapper.of(url)
                    .setErrorCorrection(ErrorCorrectionLevel.H) // 设置二维码的错误纠正规则
                    .setDrawStyle(QrCodeOptions.DrawStyle.IMAGE) // 绘制二维码时采用图片填充
                    .addImg(1, 1, request.getPart("logo").getInputStream()) // 添加图片
                    .asBufferedImage();

            ImageIO.write(image, "png", response.getOutputStream());


        } catch (WriterException e) {
            e.printStackTrace();
        }

    }
}


```

运行效果：


![在这里插入图片描述](https://img2024.cnblogs.com/blog/3084824/202410/3084824-20241021143158581-1116218517.png)


## 2\.8 生成 gif 动图二维码



```
String url = request.getParameter("url");

BufferedImage image = QrCodeGenWrapper.of(url)
        .setW(500)
        .setH(500)
        .setBgImg(request.getPart("logo").getInputStream())
        .setBgOpacity(0.6f)
        .setPicType("gif")
        .asBufferedImage();

ImageIO.write(image, "gif", response.getOutputStream());

```

v



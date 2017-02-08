# Web入门

## Introduction

**Web** 是一个由许多互相链接的超文本组成的系统，通过互联网访问。在这个系统中，每个有用的事物，称为一样“资源”；并且由一个全域“统一资源标识符”（URI）标识；这些资源通过超文本传输协议（Hypertext Transfer Protocol）传送给用户，而后者通过点击链接来获得资源。

![](http://ww1.sinaimg.cn/large/82c8e86egy1fcefao12nkj20mb08wmxd)

## Flow

一旦接触 *Web* 则会发现 *HTML* 与 *HTTP*，这是Web中必不可少的；首先需要学习HTML语法，在这之后，则会去了解到 *HTTP* 的 *GET* 与 *POST* 方法，这是最常用的两种方法，实际上还有HEAD、TRACE等方法，待需要用到的时候再做介绍；下面是具体结构与解释：

![](http://ww1.sinaimg.cn/large/82c8e86egy1fcehg4vrg9j20b4087wep)

**HTML:** 超文本标记语言，告诉浏览器怎样把内容呈现给用户。

**HTTP:** 超文本传输协议，完成Web中客户和服务器之间的会话，端口为80，实际上HTML是HTTP响应的一部分。

![](http://ww1.sinaimg.cn/large/82c8e86egy1fcefxy7xc5j20gg08smz0)

**GET:** 用户点击指向一个页面的链接，浏览器则向服务器发送一个 *HTTP GET*，请求服务器获得页面。GET中参数会追加到请求URL第一部分的后面，以 _?_ 开头，各参数之间用 _&_ 分隔。

**POST:** 用户填写表单，点击 *Submit* 提交，浏览器则向服务器提供用户在表单中键入的信息，同时也可以请求某个东西。POST中参数放在实体中，所以不受限制，这是与GET的区别。

***

至此，我们发现Web服务器能够提供静态Web页面，但是实际情况下我们总需要一些动态的东西，比如显示时间等，这个时候 *Servlet* 就有了它的作用。

**Sevlet**  用来获取 Http 客户端请求的数据，处理它们，然后生成 html 等格式的数据并发送给客户端。代码示例：

```java
import javax.servlet.*;
import javax.servlet.http.*;
import javax.io*;

public class BasicServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

      PrintWriter out = response.getWriter();
      java.util.Date today = new Java.util.Date();
      out.println("<html>" +
                  "<body>" +
                  "<h1 align=center>HF\'s Servlet</h1>"
                  + "<br>" + today + "</body>" + "</html>");
    }
}
```

然而利用`println()`来格式化HTML不太好，且容易出错，所以就接触到了**JSP** ，这就是刚刚入门的学习流程。

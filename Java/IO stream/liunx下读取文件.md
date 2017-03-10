### Linux下文件的读取

Liunx下文件的读取与Windows下略有区别，主要是路径"/"与"\"的原因，以txt为例，在Linux下读取文件代码：

```java
//获取Linux下项目所在的绝对路径，读取txt文件中内容
				String path = getClass().getProtectionDomain().getCodeSource().getLocation().getPath();
				String domain_host = "";
				if(path.indexOf("WEB-INF")>0){
					path = path.substring(0,path.indexOf("/WEB-INF"));
				}
				domain_host = ReadFromFileUtil.readTxtFile(path
						+ "/domain_host.txt");
```


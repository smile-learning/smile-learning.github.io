## 问题现象

在执行 Start Debugging 时，assmebleDebug 阶段可能会出现 ZipException 异常报错，重启 vsocde 也不能解决。
```
exception in thread "main" java.util.zip.ZipException: zip END header not found 
at java.base/java.util.zip.ZipFile$Source.zerror(ZipFile.java:1567) 
at java.base/java.util.zip.ZipFile$Source.findEND(ZipFile.java:1462) 
at java.base/java.util.zip.ZipFile$Source.initCEN(ZipFile.java:1469) 
at java.base/java.util.zip.ZipFile$Source.(ZipFile.java:1274) 
at java.base/java.util.zip.ZipFile$Source.get(ZipFile.java:1237) 
at java.base/java.util.zip.ZipFile$CleanableResource.(ZipFile.java:727) 
at java.base/java.util.zip.ZipFile$CleanableResource.get(ZipFile.java:844) 
at java.base/java.util.zip.ZipFile.(ZipFile.java:247) at java.base/java.util.zip.ZipFile.
```

## 问题成因

导致这个问题的直接原因时 gradle 工具的压缩包损坏，在构建过程中，assmebleDebug 阶段会寻找对应的 gradle 工具，如果没有则自动下载。其中如果发现已经下载的 gradle zip 包，则自动解压。

但是由于网络问题，assmebleDebug 阶段可能超时，导致自动下载的 `gradle-{version}.zip` 文件并不完整。下次执行 assmebleDebug 时，自动尝试解压这个 zip 文件，于是报错。

## 解决方法

在 `%USERPROFILE%\.gradle\wrapper\dists` 的目录下，找到对应版本损坏的 `zip` 文件，手动下载对应的 gradle 版本，并替换损坏的文件。
## 问题原因
Android 应用是用 Gradle 工具构建，在构建时，会下载 gradle 相关的工具和项目依赖，这个下载过程可能需要科学上网。

另外，这个过程耗时较长（看网络状况），往往十分钟以上，这会导致 VSCode 执行任务超时，为了解决这个问题，我们可以先用 AndroidStudio 创建一个项目，并执行构建过程，把相关的依赖下载下来。

## 解决方法

### 删除 init.gradle 文件
我们昨天的时候，创建了 init.gradle 文件，使用国内 gradle 镜像，但目前我的测试，这个文件的设置并没有起到作用，具体如何使用国内镜像，我后续再研究一下，为了避免干扰，我们先把这个文件删掉。

```sh
# 打开命令行工具（cmd.exe），执行如下命令
del %USERPROFILE%\.gradle\init.gradle

# 也可以文件管理器地址栏输入：%USERPROFILE%\.gradle，进入到 .gradle 目录，手动删除
```
### 检查代理设置是否正确

用记事本打开 `gradle.properties`文件
```sh
# 打开命令行工具（cmd.exe），执行如下命令
notepad %USERPROFILE%\.gradle\gradle.properties
```

文件应该有一下格式的内容，其中 `proxyPort` 需要修改为自己本机系统代理的端口
```
systemProp.http.proxyHost=127.0.0.1
systemProp.http.proxyPort=10809
systemProp.https.proxyHost=127.0.0.1
systemProp.https.proxyPort=10809
```

注意：开启代理，并切换到`全局`模式
### 用 AndroidStudio 创建一个项目并构建

打开 AndroidStudio 后，选择 New Project 新创建一个项目，创建模板可以选择 `Empty Activity` ，点击 `Next` -> `Finish`

![img](/img/create-empty-app.png)

打开项目后，执行这个项目，这时候 AndroidStudio 会自动构建这个项目。

![img](/img/start-debugging.png)


在执行 gradle 构建时，AndroidStudio 底部应该会有以下信息，可以看到，系统正在从 dl.google.com 下载需要的依赖。

![img](/img/gradle-dep-download.png)

如果没有任何报错，则表示构建并执行成功了，这时候 Android 模拟器中应该会显示你刚刚构建的应用。

## 回到 VSCode 并 Start Debugging

上述操作完成后，gradle 的基本依赖应该就下载完了，这时回到 VSCode，重新执行 `Start Debugging` 操作，设备选择你的 Android 模拟器，一切顺利的话，程序就可以跑起来了。
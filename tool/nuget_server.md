# Nuget-Server

---

1. 新建一网站项目，然后打开 程序包管理器控制台 ，使用 “Install-Package NuGet.Server -Version 2.2.2”命令
	* 上传包的命令：nuget push NLog.4.0.6110.20021.nupkg -s http://10.58.1.230:800/ (加粗地方换成你自己的包名)
2. 下载NuGet.exe，并把它放到系统变量中
	* 进入项目目录，按下Shift并单击鼠标右键，选择在此处打开命令窗口		
	* 使用 nuget spec 命令，创建nuspec文件		
	* 删除不需要的节点		
	* 关于程序集版本号的设置 默认是 [assembly: AssemblyVersion("1.0.0.0")] [assembly: AssemblyFileVersion("1.0.0.0")]	* 可将AssemblyFileVersion注释掉，AssemblyVersion 可设置为1.0.
	* 生成的版本号将会类似于1.0.6109.25317，后面的2个数字是必然递增的		
	* 将依赖的资源文件的生成操作设置为内容		
	* 类库依赖是指，我们有2个程序集，名字分别是Data，Data.SqlServer，我们针对这个项目不想发布2个包，那么问题在与如何在一个Package中，添加多个类库文件？		
	* 类库依赖的示例：<file src=".\bin\release\data.dll" target="lib\net40" />
3. 工具->库程序包管理器->程序包管理器设置。选择程序包源，点击“+”号，输入名字和项目地址
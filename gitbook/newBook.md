## 本节介绍如何从零开始构建一本书籍

### 组件介绍

	1. GitBook 包含 gitbook 命令行工具，文本编辑器，还有它的git仓库用来存md文件，以及通过git仓库得到的html文件。
	
	2. Github集成，可以将Github的仓库作为GitBook的源git仓库，原因就是在Github上可以方便地与其他人交流。还有一个原因就是GitBook的git仓库是很封闭的，每次你要对仓库进行修改都需要输入你的用户名和token。token在[这里](https://www.gitbook.com/@kangqf/settings/tokens)查看。
	
	3. GitBook Editor 是上面提到的文本编辑器，登录后可以直接同步自己的书籍，而且还可以导入本地的gitbook仓库，但是美中不足的是只支持https不支持ssh来提交修改。不过编辑器可以记住你的用户名和密码，然后就不用每次都输用户名和密码了。
	
	### GitBook workflow
	首先我们的需求包括以下几个方面
	1. 方便地管理我们地源文件，可以对其进行版本控制和协作编辑 -> 用Github托管md源文件
	2. 能够在本地查看渲染效果 -> 用`gitbook serve`来查看
	3. 因为GitBook的编辑器很方便，我们要用上 -> 从Github导入进编辑器
	4. 能够在GitBook的网站上看见我们的书籍 -> Github 与 GitBook 集成
	
	这样我们就得到了我们的大致的workflow
	1. 我们用GitBook Editor 来编辑和管理我们的Github仓库，包括分支管理
	2. 修改后我们在本地进行预览，没有问题就同步到我们的Github
	3. GitBook通过hook更新其内容
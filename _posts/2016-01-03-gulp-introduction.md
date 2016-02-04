# gulp简单介绍 —— 庞伟湛PAM ##

#### *目录*
1. gulp简介
1. gulp与grunt的区别
1. nodejs和npm
1. 创建一个npm项目 —— npm init .
1. 为我的项目安装npm插件 —— npm install ***
1. gulp编写简述 —— gulp.***
1. 在我的项目中使用gulp —— (ejs + sass)
1. 我认为的文件目录结构 —— app, build*
1. 我的gulpfile.js详解
1. 寻找好用的gulp插件 —— cnpmjs.org

<hr>

### 1. gulp简介 ###
gulp是前端开发过程中对代码进行构建的工具，是自动化项目的构建利器；它不仅能对网站资源进行优化（例如压缩/丑化，打包等），还能让我们在开发中很多重复性工作自动完成，可谓是我们前端人员开发的利器。使用它，我们不仅可以很愉快的编写代码，而且还大大的提高了我们的生产力。

gulp是基于Nodejs的自动任务运行器， 它能自动化地完成 javascript/coffee/sass/less/html/image/css 等文件的的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后重复指定的这些步骤。在实现上，她借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，使得在操作上非常简单。

更多请看：

[gulp详细入门教程](http://www.ydcss.com/archives/18)

[gulp官方网址：http://gulpjs.com](http://gulpjs.com)

[gulp插件地址：http://gulpjs.com/plugins](http://gulpjs.com/plugins)

[gulp 官方API：https://github.com/gulpjs/gulp/blob/master/docs/API.md](https://github.com/gulpjs/gulp/blob/master/docs/API.md)

[gulp 中文API：http://www.ydcss.com/archives/424](http://www.ydcss.com/archives/424)

<hr>

### 2. gulp与grunt的区别 ###
gulp与grunt都是基于nodeJS的自动任务运行器，它们都能帮助我们前端人员完成自动化的构建工作，为我们除去繁琐的重复性工作。

不同的工作流程导致了gulp与grunt的异同，首先请看下图：
![gulp与grunt的异同](http://i.imgur.com/gDh7FTi.png)

可以看出，grunt是以临时文件作为流程的中转站的。这意味着，grunt将会不断的进行I/O工作，直到我们指定的任务完成。而gulp则是以管道思想为主，通过流的形式来处理文件内容。因为grunt频繁的I/O工作，对于同一项任务，grunt所需时长要比gulp长。

gulp.js 的作者 [Eric Schoffstall](https://github.com/Contra) 在他介绍 gulp.js 的 presentation 中总结了 Grunt 的几点不足之处：

> 1. 插件很难遵守单一责任原则。因为 Grunt 的 API 设计缺憾，使得许多插件不得不负责一些和其主要任务无关的事情。比如说要对处理后的文件进行更名操作，你可能使用的是 uglify 插件，也有可能使用的是 concat 插件（取决于工作流的最后一个环节是谁）。这也是Grunt一个设计思想：把对文件的操作抽象为一个独立的组件（Files），任何插件都以相同的规则来使用它。遗憾在于，使用它的过程发生在每个插件的独立配置对象里，所以总给人一种“把不该这个插件做的事情丢给它来做”的别扭感觉。
> 1. 用插件做一些本来不需要插件来做的事情。因为 Grunt 提供了统一的 CLI 入口，子任务由插件定义，由 CLI 命令来调用执行，因此哪怕是很简单的外部命令（比如说运行 karma start）都得有一个插件来负责封装它，然后再变成 Grunt CLI 命令的参数来运行，多此一举。</br>
> 1. 试图用配置文件完成所有事，结果就是混乱不堪。规模较大，构建／分发／部署流程较为复杂的项目，其 Gruntfile 有多庞杂相信有经历的人都有所体会。而 gulp.js 奉行的是“写程序而不是写配置”，它走的是一种 node way。
> 1. 落后的流程控制产生了让人头痛的临时文件／文件夹所导致的**性能滞后**。这是 gulp.js 下刀子的重点，也是本标题里“流式构建”所解决的根本问题。流式构建改变了底层的流程控制，大大提高了构建工作的效率和性能，给用户的直观感觉就是：更快。


作为对比和总结，作者列出了 gulp.js 的五大特点：

> 1. 使用 gulp.js，你的构建脚本是代码，而不是配置文件；
> 1. 使用标准库（node.js standard library）来编写脚本；
> 1. 插件都很简单，只负责完成一件事－基本上都是 20 行左右的函数；
> 1. 任务都以最大的并发数来执行；
> 1. 输入／输出（I/O）是基于“流式”的。

<hr>

### 3. nodejs和npm ###
在使用gulp进行开发之前，我们必须现有一个nodeJS的环境。目前安装nodeJS已经异常简单：

相应教程链接：[windows 下安装nodejs及其配置环境](http://www.cnblogs.com/pigtail/archive/2013/01/08/2850486.html)

**安装NodeJS的步骤归纳如下：**

1. 打开[nodeJs官网-下载地址](http://www.nodejs.org/download/)，下载nodeJs（一般是.msi文件，直接双击打开即可）。
1. 因为npm（node package manager，一个NodeJS包管理和分发工具），是集成于nodeJs的安装文件中的。因此，当nodeJs安装完成后，便可直接在命令行使用关键字`npm`。
	1. 以下是测试安装是否成功的方法：
	2. 输入: `node --version`查看nodeJs的版本
	3. 输入：`npm --version`查看npm的版本
	4. 很多时候，我们都可以通过查看所安装工具的版本号来查看安装是否成功。
	5. ![](http://i.imgur.com/jfu916Z.png)

在默认环境下，nodeJS是会自动的设置系统的环境变量的。但有时候电脑出问题了，或者360等软件的影响(具体未明)，会发现在键入`node --version`时，报错了！

    “***不是内部或外部命令......”

原因在于我们并没有配置好系统所需的环境变量。
下面是解决方法：

	//假设我的nodeJs是安装在F:\nodejs下的。

> 1. 创建一个node_path——F:\nodejs\node_global\node_modules
> 1. 配置**系统环境变量中的PATH**——
> 	1. C:\Users\Administrator\AppData\Roaming\npm;
> 	1. **F:\nodejs\node_global;**            //这个很重要！
> 1. 一般，安装好的组件会放在F:\nodejs\node_global下。


**同时，您需要一个命令行环境来执行nodejs的命令。**

> 1. 在windows下，我推荐使用**PowerCmd**来进行window的原生命令行环境，它可以在一个窗口内同时启动多个cmd进程。</br>
> 1. 同时推荐安装**git bash**来进行类linux的命令环境，我们也可以通过git bash来进行git的命令行操作。

[PowerCmd下载地址](http://www.pc6.com/softview/SoftView_30583.html)

[git bash下载安装图文教程](http://jingyan.baidu.com/article/7f766dafba84f04101e1d0b0.html)

<hr>

### 4. 创建我的node项目和package.json —— npm init . ###
我们可以通过在命令行中输入：`node init yourDir`来实现一个基本项目的创建，创建过程中会生成一个属于该项目的**package.json**。</br>

强插广告：[package.json字段全解析](http://my.oschina.net/u/1992917/blog/523046)

**下面我将演示如何创建一个nodeJs项目。**

	// 其实就是创建属于它的package.json。整个项目围绕package.json展开
	// 假设我正处于F盘，并使用git bash命令行环境

> 1. `mkdir test` //创建文件夹test
> 1. `cd test`	//进入到test文件夹中
> 1. `npm init .` //创建npm管理项目
> 1. 根据提示输入信息(包括name, version, description等)，直接按**回车**可以选择默认输入。
> 1. 信息录入完整后，会出现整个package.json的内容，确认无误后，输入**yes**完成项目创建。
> 1. 这时候我们会发现test目录下多了package.json文件。
> 	1. "devDependencies"与"dependencies"。
> 	2. 在package.json中，"devDependencies"这一项意味着所安装的插件是开发所依赖的插件，在正式上线时不会用上。
> 	2. 而"dependencies"这一项意味着所安装的插件是生产所依赖的插件，是使用于生产环境中的。

**基本的package.json如下：**

	```javascript
	{
	  "name": "test",   //项目名称（必须)
	  "version": "1.0.0",   //项目版本（必须）
	  "description": "This is for study gulp project !",   //项目描述（必须）
	  "homepage": "",   //项目主页
	  "repository": {    //项目资源库
	    "type": "git",
	    "url": "https://git.oschina.net/xxxx"
	  },
	  "author": {    //项目作者信息
	    "name": "surging",
	    "email": "surging2@qq.com"
	  },
	  "license": "ISC",    //项目许可协议
	  "devDependencies": {    //项目依赖的插件
	    "gulp": "^3.8.11",
	    "gulp-less": "^3.0.0"
	  }
	}
	```

<hr>

### 5. 为我的项目安装npm插件 —— npm install *** ###

[NPM 使用介绍](http://www.runoob.com/nodejs/nodejs-npm.html)

[npm修改下载源为淘宝镜像，推荐使用config命令，能够一劳永逸](http://www.cnblogs.com/trying/p/4064518.html)

通过在命令行中输入  `npm install <name>`  来进行nodeJS插件的安装，例如gulp插件：`npm install gulp --save-dev`。

**其中我们可以在命令的后面加上各种参数，来实现我们想要的效果。例如：**

	// 以下均以安装gulp插件为例

1. `npm install` 可以缩写为`npm i`。
1. `npm i gulp -g`
	- `-g`意味着在全局范围内安装gulp插件。
	- gulp插件会被安装在c盘下的global_modules中。
	- 安装完成后，在电脑的任意npm项目中，我们都可以使用到所安装的gulp插件，意味着，gulp插件已经**全局化**了。
1. `npm i gulp --save-dev`。 
	- 命令执行完后，打开package.json，发现`"devDependencies"`下多了`"gulp": "^3.9.0"`这一项。
	- `--save-dev`意味着我们会将gulp插件安装在开发依赖下，也就是"devDependencies"中了。
1. `npm i gulp --save`。
	- 本命令`--save`与`--save-dev`属于同类型命令，都会改写package.json的中的依赖列表。
	- 命令执行完后，打开package.json，发现`"devDependencies"`下多了`"gulp": "^3.9.0"`这一项。
	- `--save-dev`意味着我们会将gulp插件安装在生产依赖下，也就是"dependencies"中。
1. `npm i -d`。
	- `-d`这个命令，是告诉npm在安装的时候告诉我安装时的消息，我们可以通过设置此命令来查看npm的安装进度，拒绝傻等。
	- **推荐加上！**

**当我们在命令行中直接运行`npm install`，系统会做如下处理：**

1. 寻找当前目录下的package.json;
1. 根据pacakge.json中的"dependencies"和"devDependencies"，下载对应的插件到/node_modules目录下。
1. 因此，`npm install`可以用作项目的**环境初始化**。

##### npm的其他命令 #####
1. 卸载插件：`npm unistall <name> [-g] [--save-dev]`  //PS：不要直接删除本地插件包
1. 更新插件：`npm update <name> [-g] [--save-dev]`
1. 查看npm帮助：`npm help`
1. 查看当前目录已安装的插件：`npm list`

### 6. gulp编写简述 —— gulp.*** ###

[gulp官方API文档](https://github.com/gulpjs/gulp/blob/master/docs/API.md#gulptaskname-deps-fn)

[gulp教程之gulp中文API](http://www.ydcss.com/archives/424)

[gulp官方fn介绍](https://github.com/gulpjs/gulp/blob/master/docs/API.md#fn)

**gulp的API非常简单，分别是：**

1. gulp.src(globs[, options])：
	1. 指明源文件路径 
1. gulp.dest(path):
	1. 指明任务处理后的目标输出路径
1. gulp.task(name[, deps], fn):
	1. 注册任务
	1. name 是任务名称；deps 是可选的数组，其中列出需要在本任务运行要执行的任务；
	1. fn 是任务体，这是 gulp.js 的核心了，需要花时间吃透它，[详情见此](https://github.com/gulpjs/gulp/blob/master/docs/API.md#fn)。
1. gulp.watch(glob[, options], tasks)／gulp.watch(glob[, options, cb]):
	1. 监视文件的变化并运行相应的任务。
	2. 你没看错，watch 作为核心 API 出现在 gulp.js 里了，具体用法还是要多看文档，不过接下来我们会演示简单的例子。


**如何运行gulp任务？**

1. `gulp <taskname>`
	1. 假设我注册了一个任务，名为'test'
	2. 那么我可以在命令行中执行`gulp test`来运行此任务
1. `gulp`
	1. 直接运行`gulp`，程序会找到配置文件`gulpfile.js`中所注册的任务`'default'`，并执行它。
	2. 这也就意味着，`'default'`任务是**默认缺省任务**。


范例
---

我们来尝试一个最常见的范例，写一个 node.js 程序时所需要的构建脚本。在这个项目中，我们使用以下插件：

1. gulp本身（`gulp`）
1. 语法检查（`gulp-jshint`)
1. 合并文件（`gulp-concat`）
1. 压缩代码（`gulp-uglify`）
1. 更名操作（`gulp-rename`）

接着我们需要先在项目下安装这些插件：

	// 例句
	$ npm install <PLUGIN_NAME> --save-dev
	
	// 最终安装语句如下（批量安装）：
	$ npm install gulp gulp-jshint gulp-concat gulp-uglify gulp-rename --save-dev

之后我们开始编写`gulpfile.js`。这个是gulp的配置文件，我们所需的所有任务都在这里配置。此文件必须有：

最后我们完成所有任务的编写，完整的代码如下：

	```javascript

	// 以下是gulpfile.js的内容

	var gulp = require('gulp'),
		jshint = require('gulp-jshint'),
		concat = require('gulp-concat'),
		uglify = require('gulp-uglify'),
		rename = require('gulp-rename');
	
	// 任务'jshint'：语法检查
	gulp.task('jshint', function () {
	    return gulp.src('src/*.js')
	        .pipe(jshint())
	        .pipe(jshint.reporter('default'));
	});
	
	// 任务'minify'：合并文件之后压缩代码
	gulp.task('minify', function (){
	     return gulp.src('src/*.js')
	        .pipe(concat('all.js'))
	        .pipe(gulp.dest('dist'))
	        .pipe(uglify())
	        .pipe(rename('all.min.js'))
	        .pipe(gulp.dest('dist'));
	});
	
	// 任务'watch'：监视文件的变化
	gulp.task('watch', function () {
	    gulp.watch('src/*.js', ['jshint', 'minify']);
	});
	
	// 任务'default'：注册缺省任务。
	// **直接运行gulp而不附带任务名时，执行default任务**
	gulp.task('default', ['jshint', 'minify', 'watch']);
	```

可以看出，基本上所有的任务体都是这么个模式：
	
	```javascript
	gulp.task('任务名称', function () {
	    return gulp.src( '文件' )
	        .pipe(...)
	        .pipe(...)
	        // 直到任务的最后一步
	        .pipe( gulp.dest( '目的地' ) );
	});
	```

**基本流程：获取要处理的文件 -> 传递给下一个环节处理 -> 把返回的结果继续传递给下一个环节 -> ... -> 所有环节完成，输出结果。**

pipe 就是 stream 模块里负责传递流数据的方法而已，至于最开始的 return 则是把整个任务的 stream 对象返回出去，以便任务和任务可以依次传递执行。

或许写成这样会更直观：

	```javascript
	gulp.task('task_name', function () {
	    var stream = gulp.src('...')
	        .pipe(...)
	        .pipe(...)
	        // 直到任务的最后一步
	        .pipe( gulp.dest( '目的地' ) );
	    return stream;
	});
	```

至此，你已经可以使用 gulp.js 完成绝大多数的构建工作了。下一步，我也为你准备了几条建议：

花点时间浏览一下 gulp.js 插件库，大致了解下利用已有的插件你都可以做哪些事情
对于常用的插件，仔细阅读它们自己的文档，以便发挥出它们最大的功效
抽时间学习 gulp.js API，特别是 gulp.task() 里关于任务体的详细描述，学会如何执行回调函数（callback），如何返回 promise 等等
尝试编写适合自己工作流程和习惯的任务，如果它工作良好，把它做成插件发布给大家吧！

<hr>

### 7. 在我的项目中使用gulp —— (ejs + sass) ###

#### html ####
在开发“后台语音在线系统”时，我深深的感到了html模板化的重要性。结合之前自己的一些个人心得，我在进行“奇迹战神”的移动端官网时，做了大胆的尝试，将传统的*.html使用模板工具代替。

使用另类模板取替传统的*.html有如下好处：

> 1. 实现html组件化。我们可以将一个html页面拆分成很多的模板，通过导入实现引用。
> 1. 实现内容变量化。例如每一个页面的title等。
> 1. 实现html中嵌入程序性语言，例如if语句，for语句等。

#### jade ####
一开始所使用的，是**jade模板**。jade是一款非常简洁的模板化工具，能让我们远离标签化的html书写格式。但在本人使用下来，由于：

> 1. 全新的书写习惯，学习成本高，团队迁移难度更大；</br>
> 1. jade编译后的文档，展开之后，并不能如我所愿的将所有元素都按照想要的形式美化，而是“内联元素会跟随在块级元素内”。强迫症的我找了很多Html美化的插件，均无法解决此问题。

因此，**放弃使用jade**。


#### ejs ####
接着尝试着使用了**ejs模板**。ejs较之html相比，在格式和内容上并没有太多的差别，感觉更像是在html的基础之上进行的功能性优化，其中包括：

> 1. html模板化，例如header和footer这类通用性的组件，我们从此可以直接导入即可，非常方便。同时，我们也可以实现**组件化开发**。</br>
> 1. 通过ejs生成出来的文件，可以保持原有的编辑格式，通过拼凑的形式实现html的生成。强迫症的我从此不用再让妈妈担心啦~ </br>

但是ejs也是有其缺点的，主要体现在其繁琐的变量书写格式上。`<% ... %>`尖括号与百分号结合的变量格式让我吐槽不已，比起`{{ ... }}`的双大括号格式，复杂了很多。

#### art template ####
art template原先我是并不清楚的，经过勇哥介绍之后，我才恍如隔世的回去开始深入理解这一个html模板。研究结果如下：

> 1. art template是由腾讯上海研究中心(没有猜错，即张鑫旭所在地)所创作的。</br>
> 1. 本模板是通过js的方式进行编译，具有高性能、简洁易用的优点。(这是官方介绍，实际我并没有深入理解，如有偏颇，请不要打我。) </br>
> 1. art template结合了ejs和jade的优点，在html标记化格式的基础之上，同时兼容`<% ... %>`和`{{ ... }}`的变量写法，我认为非常的方便。
> 1. 关于art template这一块，在gulp上并没有很好的构建插件，只有一个没有介绍的gulp-arttemplate插件。由于没有介绍，无法知晓该插件的使用方法，因此作废。

综上，最终选定使用ejs作为html模板，但是art template潜力巨大。

CSS
---
关于css上，因为团队一直在使用sass + compass，所以选定sass + compass来作为css模板，所使用的gulp插件为： **gulp-ruby-sass**。

js
---
由于并没有接触到太深的js编写，因此没有选定js的编写格式。还请各位指点一下，让我更好的选定js的模板。

目前gulp可以通过插件，实现js的压缩/丑化，语法检查，合并，打包等功能，还有更多上面没有讲到，但是需要的，也请跟我说说哦，我回去查查看有没有相应的插件可供使用。

<hr>

### 8. 我认为的文件目录结构 —— app, build* ###

在“奇迹战神”官网中，我尝试着探索了许多的文档目录结构。

由于gulp是一个自动化构建工具，它会把我们写好的开发文件逐一经过指定任务进行处理，形如 *.ejs 最终被输出成 *.html， *.scss 最终被输出成 *.css。

结合需求与gulp的特性，并多番思考，我认为对于我们的项目而言，以下目录结构是最为合适的。

在主目录下，目录结构如下：

	- /app			//存放构建前的文件。这是我们手写代码主要存放的位置
		-- *.ejs
		-- ejsData.json
		-- /style
			--- *.css
		-- /images
			--- *.{ png, jpg, ... }
	- /build			//存放构建后的文件
		-- *.html			//ejs构建后生成的html文件
		-- /css			
			--- *.css			// /style目录中的*.scss文件构建后存放与此
		-- /js
			--- *.js			//尚未实现构建，所有的js文件直接编辑并存放于此。
		-- /images
			--- *.{ png, jpg, ... }	//存放经过imagemin压缩后的图片
	- /node_modules		//nodeJS的插件存放位置
	- gulpfile.js		//gulp的配置文件
	- package.json		//nodeJs的配置文件
	- congif.rb			//compass的配置文件

<hr>

### 9. 我的gulpfile.js详解 ###

这是我在“奇迹战神”官网中所使用的`gulpfile.js`。
PS:其中，经过我的机器与宇阳的机器两次的测试，插件`gulp-imagemin`均无法安装。因为在宿舍提供的网络下，我的手提电脑是可以安装成功该插件的，因此推断是**部分网络被公司所墙**而导致的。目前没有解决方法。

	```javascript

	// 引入所需的依赖
	var gulp = require('gulp'),
		sass = require('gulp-ruby-sass'),
		autoprefixer = require('gulp-autoprefixer'),
		livereload = require('gulp-livereload'),
		connect = require('gulp-connect'),
		ejs = require('gulp-ejs'),
		gutil = require('gulp-util'),
		imagemin = require('gulp-imagemin'),
		pngquant = require('imagemin-pngquant'),
		cache = require('gulp-cache');
	
	// 更为推荐的写法。为每个任务配置一个config，包括src(源文件目录)，dest(目标文件目录)，fieldlist(为ejs而配), ...
	// 同时推荐npm自带的插件path。它可以为我们的路径进行处理。
	// __dirname是nodejs一运行就给出的变量，它代表着我们当前目录的位置。

	var config = {
		ejs: {
			src: ['./app/*.ejs'],
			dest: './build',
			filelist: ['app/index.ejs', 'app/service.ejs', 'app/gameInfo.ejs', 'app/newslist.ejs']
		},
		arttemplate: {
			src: ['./app/*.html'],
			dest: './build'
		},
		sass: {
			src: ['./app/style/*.scss'],
			dest: './build/css'
		},
		image: {
			src: ['./app/images/*.{png,jpg,gif,ico}'],
			dest: './build/images'
		}
	};
	
	// gulp中的help,主要输出指示性用法
	gulp.task('help',function () {
		console.log('	gulp 				完整打包并开始监听');
		console.log('	gulp build			文件打包');
		console.log('	gulp watch			文件监控打包');
		console.log('	gulp ejs			打包ejs');
		console.log('	gulp sass			打包sass');
	});
	
	// 编译ejs。因为ejs的每一个生成的页面都可以导入数据，但是目前还没有很好的导入方法。
	// 说说我的做法：
	//1、在项目根目录下编写一个ejsData.json的文件；
	// 2、在gulpfile.js的config.ejs中加入filelist这一个属性，主要是放各个需要编译的页面；
	// 3、在ejs这个任务里面用一个for循环将数据打进去

	gulp.task('ejs', function () {
		var ejsData = require('./app/ejsData.json');
		for(var i=0, len=config.ejs.filelist.length; i<len; i++){
			gulp.src( config.ejs.filelist[i] )
				.pipe( ejs( ejsData[config.ejs.filelist[i]] /* 这里放一些变量和配置属性 */ ) )
				.pipe( gulp.dest(config.ejs.dest) )
				.pipe( livereload() );
		}
	});
	
	// 编译sass。这个算是一个意外，因为在本机，如果使用了gulp.src,则会报错，只能直接使用sass这个命令，将路径导入其中。
	gulp.task('sass', function () {
	    sass(config.sass.src, { compass: true, style: 'expanded' })
			.pipe(autoprefixer({
				borwsers: ['last 2 versions'],
				cascade: true,	//是否美化属性值 默认：true 像这样：
					            //-webkit-transform: rotate(45deg);
					            //        transform: rotate(45deg);
				remove: true    //是否去掉不必要的前缀 默认：true
			}))
			.pipe( gulp.dest(config.sass.dest) )
			.pipe(livereload());
	});
	
	// 因为网络的原因，暂时无法使用
	// 最佳选择：直接使用gulp-imagemin。其中包括imagemin-pngquant, imagemin-jpegtran, imagemin-optipng等。
	// 具体可以上www.npmjs.com搜索imagemin查看。

	// gulp.task('imagemin', function () {
	// 	gulp.src( config.image.src )
	// 		.pipe(imagemin({
	//             progressive: true,
	//             svgoPlugins: [{removeViewBox: false}],//不要移除svg的viewbox属性
	//             use: [pngquant()] //使用pngquant深度压缩png图片的imagemin插件
	//         }))
	// 		.pipe( gulp.dest(config.image.dest) )
	// 		.pipe(livereload());
	// })

	// gulp.task('imageminPngquant', function () {
	// 	return gulp.src('app/images/*.png')
	// 		.pipe(imageminPngquant({quality: '65-80', speed: 4})())
	// 		.pipe(gulp.dest('build/images'));
	// });
	
	// 这个是watch命令，是用以编辑我们所需要监听的任务
	gulp.task('watch', function () {
		livereload.listen();
		// watch .ejs files
		gulp.watch( config.ejs.src, ['ejs']);
		// watch .scss files
		gulp.watch( config.sass.src, ['sass']);
		// // watch image files
		// gulp.watch( config.image.src, ['imagemin']);
	})
	
	// 这个是为我们创建服务器的命令。port可以自定义设置。
	gulp.task('connect', function() {
		connect.server({
			root: ['./'],
			port: 8989,  //需要监听的端口
			livereload: true
		});
	})
	
	// 这是'build'命令，创建它目的在于，将所有需要build的任务都集成在一起，方便编辑使用。
	gulp.task('build', function () {
		gulp.run('help');
		gulp.run('connect');
		gulp.run('ejs');
		gulp.run('sass');
	})
	
	// 这是default命令，是gulp默认的缺省任务，当只运行gulp时，默认执行它。
	gulp.task('default', function () {
		gulp.run('build');
		gulp.run('watch');
	})
	```

<hr>

path, 

### 10. 寻找好用的gulp插件 —— www.npmjs.com ###
[gulp插件地址：http://gulpjs.com/plugins](http://gulpjs.com/plugins)

![](http://i.imgur.com/RZ91UmB.png)

**大家可以登录[gulp插件地址：http://gulpjs.com/plugins](http://gulpjs.com/plugins)来搜索自己需要的插件。这个网站是npmjs的官方网站，在里面我们可以搜索所有的npm第三方插件。**

**关于npm的插件，我们可以通过[www.npmjs.com](www.npmjs.com)来搜索。加上gulp关键字，我们便可以搜索相应的gulp插件。**
![](http://i.imgur.com/qpJxOFB.png)

- - -
**[www.cnpmjs.org](www.cnpmjs.org)。这个也是一个同类型的网站，当www.npmjs.com登陆不上时，我们可以通过它来搜索插件。**
![](http://i.imgur.com/6pjnGBX.png)

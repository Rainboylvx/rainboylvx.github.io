---
title: hexo_generate_过程分析
date: 2016-03-17 22:50:09
tags:
	- nodejs
	- hexo
	- 源码分析
	
categories: nodejs

---

hexo generate 过程分析

brainfuck一下hexo generate的过程:
 加载-->渲染-->输出


hexo初始化,加载各种插件
读取_config配置
读取theme,和配置
读取 source 下的.md 和第个md里的开头信息

读取db.json里的信息

用marked 渲染md文件,得到A

用A和theme 加在一起用eje 渲染,得到B

渲染后行B,得到B的各种信息

1:path
2:分类
3:时间
4:标题
5:tag
6:archive



存下唯一的ID到db.json里

根据path generate到public



### 具体过程
输入 hexo genenrate

hexo.cmd --> node hexo generate

hexo-cli -->传递参数给hexo,调用hexo

hexo -->init 初始化 加载各种插件

func = console.get('generate')

hexo.call(func) 开始进入generate的过程
 var c = self.extend.console.get(name);
c.call(self, args).then(resolve, reject);



plugins\console

```
  console.register('generate', 'Generate static files.', {
    options: [
      {name: '-d, --deploy', desc: 'Deploy after generated'},
      {name: '-f, --force', desc: 'Force regenerate'},
      {name: '-w, --watch', desc: 'Watch file changes'}
    ]
  }, require('./generate'));
```
也就是说console 模块下的generate 是plugins\console\generate.js

就是这个函数generateConsole(args)
有这几个generateFile, writeFile,deleteFile,firstGenerate 子函数

if (args.w || args.watch) 是不是watch 模式// 这个我们先不管它


这一部分就是开始的代码,this.load 就是hexo里的load
```
return this.load().then(firstGenerate).then(function() {
    if (args.d || args.deploy) {
      return self.call('deploy', args);
    }
  });
```



### Hexo.prototype.load

这个应该是**加载** 资源用的

//加载db.json数据库
return loadDatabase(this) 

//输出 **开始处理**
self.log.info('Start processing');

//得到一个数组 source 和theme 处理后的信息
return Promise.all([
      self.source.process(),
      self.theme.process()
    ]);
	
//开始输出
return self._generate({cache: true});


分析:我们可以看到过程和我们上面的想法,差不多
根据对bluebird的理解,一串chain中没有发生数据传递,只是依次执行而已,应该是把处理后的数据放到了全局变量中


### loadDatabase(this)
var loadDatabase = require('./load_database');

//不重复加载
if (ctx._dbLoaded) return Promise.resolve();

//
var db = ctx.database;
var path = db.options.path;
var log = ctx.log;

//判断db 文件 是否存在
fs.exists(path)
//运行db.load()
return db.load()

//应该存下是db.json的一些数据
base = process.cwd(); hexo 的当前路径
this.database = new Database({
    version: dbVersion, //版本 var dbVersion = 1; 
    path: pathFn.join(base, 'db.json')//路径处理 var pathFn = require('path');
  });

### 分析db.json

打开hexo blog目录下的db.json 格式化后看一下

```
{
    "meta": {
        "version": 1, 
        "warehouse": "2.2.0"
    }, 
    "models": {
		"Asset": [],  theme 里的source文件信息
        "Cache": [], 缓存 ,好像都是theme里的文件,存入cache后,不用重复加载
        "Category": [ ], 
        "Data": [ ], 
        "Page": [ ], 
        "Post": [],  把文章存到这里,title,_content
		slug,published,date,update,comment,layout
		photos
		link
		id,content,more
        "PostAsset": [ ], 
        "PostCategory": [ ], 
        "PostTag": [ ], 
        "Tag": [ ]
	}
}  
```
  var Database = require('warehouse');
  db.load
  
  查了一下npmjs 里的warehouse:Simple JSON-based database 看来是一个仅仅处理
  json文件的第三方nodejs 包
  
  
  //表明心经加载
  ctx._dbLoaded = true;
  
  
### 加载后的数据到哪里去了?
我们知道hexo 里
this.database = new Database({
    version: dbVersion,
    path: pathFn.join(base, 'db.json')
  });
  
那加载后database 作为一个对象应该存下了db.json里的内存
loadDatabase(this) 的作用就是把db.json存在this.database里



### 开始source.process 和 theme.process

  this.source = new Source(this);
  this.theme = new Theme(this);
  
我们先看source,本目录下的source.js
var Source = require('./source');

打开后看到,调用../box 模块
var Box = require('../box');

看一下box的process 函数

可能用到的数据

```
  this.context = ctx; hexo
  this.base = base; source路径
  this.processors = []; 插件
  this._processingFiles = {};
  this.watcher = null;
  this.Cache = ctx.model('Cache');
  this.File = this._createFileClass();
```

过程

1.fs.stat 会返回 path的各种信息
看一下hexo.fs的过程
graceful-fs 对fs模块的提升
是


如果 不是文件夹 就返回

如果不是
1.检查不是不是在cache里
2. 读取

这里有几个问题:
1.base: 是ctx.source_dir 
2.会不会 递归读取?

escapeBackslash \ 全部替换成 /
relativeBase 应该是这样 ./source 

cacheFiles//???????这个地址真不懂


_readDir---:应该返回的是一个reduce后的数组
_readDir真的比较难理解
fs.readdir(base) 返回里面的 文件 数组

如果path 是一个文件夹 递归读取

如果是文件 _checkFileStatus
Cache.compareFile
_checkFileStatus 应该最终返回path代表的文件类型和path的一个{}


self._processFile(file.type,file.path)
读取到的每一个文件都 肯定返回一个数据


---
processor.process 处理
extend.Processor(),

我们要找到哪个插件注册了processor

内部插件
asset.js
data.js
post.js

process/post.js
processPost 
从db.json里读取post,在这之前已经把post读取到db.json里(doc.insert,不知道有没有渲染 ),

处理了一些信息tag,date,categories




### hexo._generate()
上面的这么一大部分,完成的操作
是把source 和 theme 文件夹是的东西读到db.json里

1.定义了一些属性
execFilter before_generate
	调用extend.filter
execFilter('template_locals
self.execFilter('after_generate



### filter
 内部模块主要在 ./plugins/filter 里
 
 'before_generate', 就是这个文件render_post.js
 一个名字就知道是把post[前面我们经load文件到了db.json]渲染了
 
 核心代码:
  return self.post.render(post.full_source, post).then(function() {
        return post.save();
full_source这个属性????, 这个东西就怎么出现的？

post.save() ????? 难道database 返回的是一个对象?


!!! 一个重要的问题:
	前面有可能都理解错了,如果是第一次读取文件genearte,存的db不是存在db.json里
	那就要重要去理解 hexo.model 这个函数!!!!!!,很重要



model 不是db.json
 要注要这个,看看这个Post 是如果使用的?
 是不是直接操作db.json
 hexo中this.post = new Post(this);



看到这里,知道调用hexo.post模块
post.full_source 是 文件路径
post 是从modle('post') 里的一个文章
self.post.render(post.full_source,post).then(function() {
        return post.save();
      });
	  
在hexo中this.post = new Post(this);

./render.js中
var renderer = self.getRenderer(ext);//得到md
ctx.extend.renderer.get
    return renderer.call(ctx, data, options);

可以看到这个就是marked
hexo.extend.renderer.register('md', 'html', renderer, true);
调用hexo.render.render()函数 render就是marked 一会验证一下





### this.execFilter('before_generate'

所以说这样before_generate 会把md渲染


### run generate



### self.execFilter('template_locals', locals,
!! 渲染了md 文件之后,那就是渲染ejs 主题了
return ctx.render.render({
      text: data.content,
      path: source,
      engine: data.engine,
      toString: true,
      onRenderEnd: tagFilter
    }, options);

tag.渲染
tag.render(data.content, data);


让我看到ejs 渲染:


post.save() 完成渲染 保存

var theme = this.theme;
view = theme.getView(name)
view.render(locals).then(saveCache);

render = _render = hexo.render

这里的view难以理解,

只要证明hexo.render里有 调用了
ctx.extend.renderer;

累了!!

hexo的工作模式 是插件之间的协作,应该理解hexo的插件工作



### generator  生成tag archive 等

###           name = layout[i];
          view = theme.getView(name);

证明这一句调用了ejs
return view.render(locals).then(saveCache);

load 之后都存的cache里面


view.render 应该会把这存到db.cache
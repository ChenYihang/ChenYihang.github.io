---
title: MQTT数据持久化(二)
date: 2019-03-30 17:46:37
tags: MQTT
categories: MQTT
---

#### 如何对MQTT做数据的存储

##### **源码编译EMQ**

​	由于需要创建自己的插件，所以得重新编译EMQ。EMQ是erlang写的，所以还需要安装[erlang](http://www.erlang.org/)环境。

Linux环境下：

```shell
git clone -b emqx30 https://github.com/emqx/emqx-rel.git
cd emq-relx && make
cd _rel/emqx && ./bin/emqx console
```

​	分支选项`-b emqx30`为必须项，否则在编译时会出现错误提示：

<pre><font color="#88BED4">===&gt; Resolved emqx-3.1</font>
<font color="#88BED4">===&gt; Including Erts from /usr/local/lib/erlang</font>
<font color="#88BED4">===&gt; Errors generating release</font>
<font color="#88BED4">sqlparse: Missing parameter in .app file: registered</font>
</pre>

​	这个问题困扰了我很久，因为一些文档版本并没有说明这一点，一直以为是erlang版本的问题导致编译失败。

​	出现下列提示则表示编译成功：

<pre><font color="#88BED4">===&gt; Starting relx build process ...</font>
<font color="#88BED4">===&gt; Resolving OTP Applications from directories:</font>
<font color="#88BED4">          /home/cehang/emqx-rel/deps</font>
<font color="#88BED4">          /usr/local/lib/erlang/lib</font>
<font color="#88BED4">          /home/cehang/emqx-rel/apps</font>
<font color="#88BED4">===&gt; Resolved emqx-3.0.1</font>
<font color="#88BED4">===&gt; Including Erts from /usr/local/lib/erlang</font>
<font color="#88BED4">===&gt; release successfully created!</font>
</pre>

##### **EMQ插件入门**

​	编译完EMQ后，会在deps目录中生成系统依赖包以及插件包。其中*emq_plugin_template*为EMQ提供的插件模板。基于插件模板可以定制自己的功能插件，比如我们将要实现的emq kafka桥架功能插件。针对插件开发[官方文档](http://docs.emqtt.cn/zh_CN/latest/plugins.html#emq-x-r3-0)也给出了一些简略的说明。

​	将插件模板复制到同目录下，更改名字创建自己的插件，其目录列表如下：

<pre>├── TODO
├── <font color="#6987E0"><b>test</b></font>
│   └── emqx_plugin_template_SUITE.erl
├── <font color="#6987E0"><b>src</b></font>
│   ├── emqx_plugin_helloworld_sup.erl
│   ├── emqx_plugin_helloworld.erl
│   ├── emqx_plugin_helloworld_app.erl
│   ├── emqx_cli_helloworld.erl
│   ├── emqx_auth_helloworld.erl
│   └── emqx_acl_helloworld.erl
├── rebar.config
├── README.md
├── Makefile
├── LICENSE
├── <font color="#6987E0"><b>etc</b></font>
│   └── emqx_plugin_helloworld.config
└── erlang.mk
</pre>

​	其中*.config为配置文件，\*_SUITE为单元测试文件，/src/\*.erl是插件的源码。修改目录下源码中的模块名字，改问与文件名对应的插件名字。更改完执行make，如果报错则检查是否修改完整。

​	回到主目录下，修改`relx.config`文件，添加:

```json
{emqx_plugin_helloworld,load}
```

​	在主目录下make，编译成功后运行EMQ，然后可通过EMQ自带的Dashboard检查插件添加效果。

​	修改`emqx_plugin_helloworld.erl`文件，在load和unload中分别添加

```
io:format("load helloworld plugin~n",[]).
```

```
io:format("unload helloworld plugin~n",[]).
```

​	重新编译插件和工程，在控制台模式下启动emq

```
cd ~/emqx-rel/_rel/emqx/bin/
./emqx console
```

​	在dashboard中启动和关闭helloworld插件

<pre>Starting emqx on node emqx@127.0.0.1
Start http:management listener on 8080 successfully.
Start http:dashboard listener on 18083 successfully.
Start mqtt:tcp listener on 127.0.0.1:11883 successfully.
Start mqtt:tcp listener on 0.0.0.0:1883 successfully.
Start mqtt:ws listener on 0.0.0.0:8083 successfully.
Start mqtt:ssl listener on 0.0.0.0:8883 successfully.
Start mqtt:wss listener on 0.0.0.0:8084 successfully.
EMQ X Broker b461e26f is running now!
Eshell V10.0  (abort with ^G)
(emqx@127.0.0.1)1&gt; 
(emqx@127.0.0.1)1&gt; load helloworld plugin
(emqx@127.0.0.1)1&gt; unload helloworld plugin
</pre>

​	
---
layout: post
title: "Octopress设置"
date: 2013-01-22 17:01
comments: true
categories: [计算机, 博客相关]
---
<!--more-->

Git设置
-------------
用以下命令会生成～/.ssh/id_rsa.pub文件：
``` sh
$ ssh-keygen -C "username@email.com" -t rsa
```
在github网站选择“Account Settings”>>“SSH Public Keys”>>“Add another public key”，粘贴~/.ssh/id_rsa.pub里的内容。   

安装octopress
-------------
下载并设置octopress
``` sh
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress
$ ruby --version # 应该>=1.9.2
$ gem install bundler # 可以把Gemfile的源修改成taobao：http://ruby.taobao.org/
$ bundle install
$ rake install # 安装默认主题，找其他主题可以用rake install['themename']
$ rake setup_github_pages # 设置你有读写权限的git repo，格式它有提示，就是你repo的ssh选项(HTTP SSH Git Read-Only)那几个选项里的ssh
```
完成这一步以后你就可以用`rake new_post["title"]`创建新博文，在./source/\_posts底下编辑你的文章，使用
`rake generate`生成页面，`rake deploy`来发表；
或者直接`rake gen_deploy`；每当你修改主题或者发表文章什么的都需要这么做。**注意修改_config.yml里对应的信息，比如url就是你的xxxx.github.com等等。**

新浪微博插件
------------
去[微博秀](http://weibo.com/tool/weiboshow)生成一下代码，注意看里面对应的各种信息比如userid等等。然后在`./source/_includes/asides/`下建立一个weibo.html：
``` html weibo.html
{% raw %}
{% if site.weibo_uid %}  
<section>  
  <ul id="weibo">  
    <li>  
      <iframe   
        width="100%"   
        height="110 默认550，我只要一个头像～"
        class="share_self"   
        frameborder="0"   
        scrolling="no"
        src="http://widget.weibo.com/weiboshow/index.php?width=0&height=550&ptype={% if site.weibo_pic %}1{% else %}0{% endif %}&speed=0&skin={{weibo_skin}}&isTitle=0&fansRow=2&noborder=0&isWeibo={% if site.weibo_show %}1{% else %}0{% endif %}&isFans=0&uid={{site.weibo_uid}}&verifier={{site.weibo_verifier}}">  
      </iframe>  
    </li>  
  </ul>  
</section>  
{% endif %}
<!--src=这一行除了一些改为可以在_config.yml里设置的，其余的配置比如边框什么的具体对照新浪的生成，e.g. isfans=0表示不显示粉丝-->
{% endraw %}
```
然后在./_config.yml文件里添加
``` sh
weibo_uid: userid               # 填入uid  
weibo_verifier: verifier         # 填入verifier
weibo_show: false      # 是否显示最近微博内容  
weibo_pic: true          # 是否显示微博中的图片  
weibo_skin: 2          # 使用哪种配色风格，数字为从1开始的微博秀风格序号
```
具体参数包括边框显示等等看微博秀给你生成的代码；接下来在_config.yml里的`default_asides: [.....]`一行里添加asides/weibo.html，其他用逗号隔开。

评论窗口
--------
评论窗口默认是disqus，这个是octopress默认支持的，**不需要任何其他配置**，只要去disqus注册一个账户，然后添加你的网址（比如xxxx.github.com）然后写一个博客名，接下来就是第三个**Site Shortname**窗口，这个会生成一个**your_short_name.disqus.com**的网址，供你管理评论，这个是唯一的。
然后在_config.yml里的
``` sh
# Disqus Comments
disqus_short_name: your_short_name
disqus_show_comment_count: true
```
里填写刚才的那个shortname，就可以了。

**评论似乎还可以用多说，不过很多人不推荐，而且disqus是不需要什么设置的，只要填入shortname就可以了，比较方便，但它是全英文的界面。**

**目前貌似disqus会挂……所以附上多说的部分，其实还有灯鹭和友言，不过大同小异——**			
首先在`./source/_layouts/post(还有page).html`里均**添加如下内容在原有disqus那节下面**(page.html是控制页面上是否出现评论，post.html是博文里是否出现评论，两个文件都需要添加)：
``` html page&post.html
{% raw %}
<div>
<article class="hentry" role="article">
<!-->......上面接原有<!-->
{% if site.disqus_short_name and page.comments == true %}
  <section>
    <h1>Comments</h1>
    <div id="disqus_thread" aria-live="polite">{% include post/disqus_thread.html %}</div>
  </section>
{% endif %}
<!-->接disqus~~添加如下部分：<!-->
{% if site.youyan_show_comment_count == true and page.comments == true %}
    <section>
        <h1>发表评论</h1>
        {% include youyan.html %}
    </section>
{% endif %}
{% if site.denglu_show_comment_count == true and page.comments == true %}
    <section>
        <h1>发表评论</h1>
        {% include denglu.html %}
    </section>
{% endif %}
{% if site.duoshuo_show_comment_count == true and page.comments == true %}
    <section>
        <h1>发表评论</h1>
        {% include duoshuo.html %}
    </section>
{% endif %}
<!-->添加部分结束<!-->
</div> <!-->整个在<div><article....一节里，不然“发表评论”四个字会顶格。<!-->
{% endraw %}
```
然后在`./_config.yml`里添加
``` sh
youyan_show_comment_count: false
denglu_show_comment_count: false
duoshuo_show_comment_count: true # 我用了多说
```
接下来在`./source/_includes/`里新建一个duoshuo.html(或者youyan.html或者denglu.html，具体看你)		
因为这些都需要注册，代码里都带有userid，我就不贴出了；注册完毕以后网站会提供你代码，分别粘贴到这些html文件里就行。

分享按钮
--------
然后是分享按钮，这个因为octopress默认支持的都是Twitter，FB和Google，想要国内主流网站的分享按钮还需要稍微设置一下。你有两个方法：**找各个网站的分享按钮生成器：**比如[新浪微博分享按钮](http://open.weibo.com/sharebutton)**或者使用bshare：**

修改你的`./source/_includes/posts/sharing.html`为
``` html sharing.html
<div class="bshare-custom"><div class="bsPromo bsPromo1"></div><a title="分享到QQ空间" class="bshare-qzone"></a><a title="分享到新浪微博" class="bshare-sinaminiblog"></a><a title="分享到人人网" class="bshare-renren"></a><a title="分享到腾讯微博" class="bshare-qqmb"></a><a title="分享到豆瓣" class="bshare-douban"></a><a title="更多平台" class="bshare-more bshare-more-icon more-style-addthis"></a><span class="BSHARE_COUNT bshare-share-count">0</span></div><script type="text/javascript" charset="utf-8" src="http://static.bshare.cn/b/button.js#style=-1&amp;uuid=&amp;pophcol=2&amp;lang=zh"></script><script type="text/javascript" charset="utf-8" src="http://static.bshare.cn/b/bshareC0.js"></script>
```
或者你可以自己去[bshare](http://www.bshare.cn/)注册一下，功能很多，还可以选择按钮样式。

**两者的不同似乎在于，如果你使用新浪的分享按钮生成器，那么可以使用你的userid，在别人分享的时候会出现@yourname的形式；至于bshare，我没发现他有这个功能，可能是我比较菜的缘故，如果有人知道可以，请不吝告知；我自己也会抽空详细了解一下bshare的各种服务的。**

**再提一点，最好把`_config.yml`里关于twitter之类的东西全都false掉～我是图方便直接重写了sharing.html；而bshare里有twitter和facebook，但是不能单独开/关了，只能是像以前那样加入if语句来控制整个bshare的开/关。**

域名绑定
--------
在./source文件夹下新建一个文件CNAME，写入你要绑定的域名比如www.yourname.com什么的，然后到你的dns服务那里把www.yourname.com设置CNAME到你的yourname.github.com即可。

Octopress更新
-------------
定期更新一下octopress，官方有详细的[更新说明](http://octopress.org/docs/updating/)			
如果`$ git pull octopress master`报错：		
`fatal: 'octopress' does not appear to be a git repository`的话		
原因可能是没有相应的远程分支，在`./.git/config`里应该没有`[remote "octopress"]`块，需要添加一下		
`$ git remote add octopress https://github.com/imathis/octopress.git`

添加回到顶部按钮
----------------
是jquery的插件，动画效果不错。首先修改`./source/_includes/custom/head.html`文件，加入如下行：
``` html head.html
<!-- jquery -->	
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js" type="text/javascript"></script>
<!-- easing plugin ( optional ) -->
<script src="https://raw.github.com/sksmatt/UItoTop-jQuery-Plugin/master/js/easing.js" type="text/javascript"></script>
<!-- UItoTop plugin -->
<script src="https://raw.github.com/sksmatt/UItoTop-jQuery-Plugin/master/js/jquery.ui.totop.js" type="text/javascript"></script>
<!-- Starting the plugin -->
<script type="text/javascript">
	$(document).ready(function() {
		
		var defaults = {
	  		containerID: 'toTop', // fading element id
			containerHoverID: 'toTopHover', // fading element hover id
			scrollSpeed: 1200,
			easingType: 'linear' 
	 	};
		
 
		$().UItoTop({ easingType: 'easeOutQuart' });
 
	});
</script>
```
然后修改`./source/stylesheets/screen.css`加入如下行：
``` css screen.css
#toTop {
	display:none;
	text-decoration:none;
	position:fixed;
	bottom:150px;
	right:100px;
	overflow:hidden;
	width:51px;
	height:51px;
	border:none;
	text-indent:100%;
	background:url(https://raw.github.com/sksmatt/UItoTop-jQuery-Plugin/master/img/ui.totop.png) no-repeat left top;
}

#toTopHover {
	background:url(https://raw.github.com/sksmatt/UItoTop-jQuery-Plugin/master/img/ui.totop.png) no-repeat left -51px;
	width:51px;
	height:51px;
	display:block;
	overflow:hidden;
	float:left;
	opacity: 0;
	-moz-opacity: 0;
	filter:alpha(opacity=0);
}

#toTop:active, #toTop:focus {
	outline:none;
}
```
貌似默认`screen.css`只有一长长长长长……行，所以gvim打开都会顿一下，这个新建一行就行。还有上面的地址是我找的按钮图标，不喜欢可以自己去找。		
最后当然`$ rake generate;rake preview`看看效果～～


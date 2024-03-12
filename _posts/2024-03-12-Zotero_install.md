---
layout: post
title: Zeotero 6.0 安装指南  
date: 2024-03-12
description: 记录了使用zotero软件的过程，综合了多方资料。后续若进一步有补充，再来改进。
categories: Install
tags: Zotero
author: NMt
mathjax: true
---

* content
{:toc}

因为之前使用的论文阅读软件都逐步需要收费了，所以现在转来使用免费资源zotero，就是上手适应会有点难，所以在这里记录一下。  

<div style='display: none'>
@@@@
</div>





{% raw %}
## 安装Zotero  

Zotero的安装比较简单，下载之后按照提示一步一步安装即可。  
[Zotero下载地址][link_01]: https://www.zotero.org/  

**注：自定义版本可以更改安装位置，标准版本不能更改。**  

个人感觉zotero只实现了pdf论文管理功能，想要轻松的阅读文献，需要安装一些插件。  

### 坚果云的同步设置  

1. 首先进入坚果云的官网注册账号；  
2. 然后在【账户信息（账号下拉框中选择）】-> 【安全选项】中点击【添加应用】；  
3. 根据提示填写【名称】->【生成密码】，将生成的密码记录下来；  
   ![][pt_03]  
4. 在zotero中创建账号，【设置】->【首选项】->【同步】，输入zotero账号登录；  
5. 再把之前的坚果云服务器地址、用户名、密码输入进去，点击【验证服务器】成功即可。  
   ![][pt_04]  


## 相关安装和设置  

### 1.Zotero Connector下载

[zotero Connector下载地址][link_02]: https://www.zotero.org/download/connectors  

这个插件是安装在chrome上的，也可以直接在chrome的插件商店中进行检索安装。  

说明：可以利用浏览器插件Zotero Connector收藏网页、期刊等。提取信息。  

### 2.文字处理软件插件：Microsoft Word插件下载

1. 打开【Zotero】  
2. 进入【编辑->首选项】（Mac快捷键command+,）  
3. 进入【引用】->【文档编辑软件】  
4. 安装加载项【Microsoft Word】或【LibreOffice】  

![][pt_01]  

**注：我安装zotero的时候自动安装了Microsoft word加载项，可以打开word检查选项卡是否多出了一个zotero，如果是则表明安装成功。**  

### 3.安装插件步骤  

zotero插件的后缀是.xpi，安装各种插件的基本步骤如下：  

1. 打开【Zotero】  
2. 进入【工具->附加组件】  
3. 点击设置(齿轮图案)-> Install Add-on From File / 或者直接将.xpi插件文件拖入到该界面也可。  

![][pt_02]  

### 4.搜索引擎：搜索引擎环境配置  

1. 进入Zotero根目录下/locate位置 可进入【首选项】—【高级】—【文件和文件夹】—【打开数据文件夹】进入Zotero根目录  
2. 替换【locate】中的engines.json文件  

**注：替换内容附在文章最后。**  


## 各种插件的配置  

### 1.ZotFile  

[下载地址][link_03]: http://zotfile.com/  

**功能介绍**：
1. 自动修改附件名  
2. 提取附件中的笔记  
3. 【Attaching New Files】—添加新附件  
4. 【Send to Tablet】—多端同步阅读  

### 2.ZoteroQuickLook  

[下载地址][link_04]: https://github.com/404neko/ZoteroQuickLookReload  

**注：该地址是适合6.0版本的插件。**  

**功能介绍**：按空格键即可实现QuickLook预览。  

这个插件需要先安装一个其他的软件，才可以使用，具体步骤如下:  

1. 先下载[QuickLook.msi][link_05]，在电脑上进行安装；  
2. 下载[ZoteroQuickLook][link_04]插件，用安装插件的步骤安装在Zotero软件中；  
3. 打开Zotero 编辑->首选项->高级->设置编辑器；  
4. 搜索 extensions.zoteroquicklook.customviewcommand ，搜到之后双击打开(右键修改也可以)，在对话框中填入QuickLook.exe的完整路径。  

### 3.茉莉花-Jasminum  

利用Zotero Connector直接获取题录及对应PDF等附件内容。但是不能满足从Zotero软件直接导入中文PDF文件的元数据读取需求。  
Jasminum就是为了解决这一问题而出现的，具体介绍看Jasminum github官网主页。  
插件可从Jasminum github官网下载.xpi文件  

[下载地址][link_06]: https://github.com/l0o0/jasminum  


**功能介绍**：  
1. 拆分或合并 Zotero 中条目作者姓和名；  
2. 根据知网上下载的文献文件来抓取引用信息（根据文件名）；  
3. 添加中文PDF/CAJ时，自动拉取知网数据，该功能默认关闭。需要到设置中开启，注意添加的文件名需要含有中文，全英文没有效果（根据文件名）；  
4. 为知网的学位论文 PDF 添加书签；  
5. 更新中文翻译器。  

**注：在我没有安装这个插件的时候，导入中文文献时会出现未找到匹配文献的报错信息，安装了之后能够识别出中文文献。**  

具体安装步骤：  
1. 按照基础步骤安装Jasminum插件；  
2. 安装PDFtk server：若想使用Jasminum的书签添加功能，需要提前安装好PDFtk server，[下载地址][link_07]: https://www.pdflabs.com/tools/pdftk-server/；  
3. 配置Jasminum插件: Zotero首选项中的茉莉花选项卡可以进行相关的配置，将PDFtk的路径填写进去；  
   ![][pt_05]  
4. Jasminum插件配合PDFtk server主要有三个功能：  
	一是可以使用该插件抓取中文文献的元数据，如将下载好的文献拖入可以直接右击查找元数据  
	二是可以生成PDF的目录（一般下载学位论文的PDF版本是没有目录的）  
	三是可以将作者姓名进行合并  

### 4.zotero-pdf-translate 插件（内置阅读器划线翻译插件）  

[下载地址][link_08]: https://github.com/windingwind/zotero-pdf-translate  

下载之后按照常规步骤安装即可，安装完成之后有的翻译源需要API密钥才能够使用。  

#### 百度API密钥获取步骤  

1. 登录[百度翻译开放平台][link_09]: https://fanyi-api.baidu.com/api/trans/product/index  
2. 在首页的【产品服务】->【通用文本翻译】->【立即使用】  
3. 按照步骤填写信息，最后一步填写申请表格的时候相关信息如下：  
   * 您的应用名称是什么：  
   Zotero PDF Translate  
   * 有无应用相关的介绍链接（40个字符的限制，粘贴的不完整也通过了）：  
   https://github.com/windingwind/zotero-pdf-translate  
   * 请简单介绍下您的应用：  
   使用机器翻译功能，翻译文献  
   * 服务器地址: 留空不填  
4. 在账号下拉框中点击开发者信息，就可以看到APP ID以及密钥，按照格式填写`APPID#密钥#Action`（Action可选, 见https://api.fanyi.baidu.com/doc/21, 默认0）；例如，你的APPID是112233，密钥是aabbcc，则应该填入112233#aabbcc。如果你需要设置自定义术语干预（ACTION），例如你的action是999，则填入112233#aabbcc#999。action可省略，默认是0。  

### 5.zotero-reference插件  

**功能介绍**：自动获取参考文献  

[下载地址][link_10]: https://github.com/MuiseDestiny/zotero-reference  

### 6.zotero-doi-manager插件  

**功能介绍**：自动获取doi  

[下载地址][link_11]: https://github.com/bwiernik/zotero-shortdoi/releases  

### 7.zotero-better-notes  

**功能介绍**：一个写笔记的插件。还没有使用过，等使用一段时间之后再来补充记录。  

[下载地址][link_12]: https://github.com/windingwind/zotero-better-notes/releases  

参考资料：  

https://zhuanlan.zhihu.com/p/347493385?utm_campaign=shareopn&utm_medium=social&utm_oi=35221906915328&utm_psn=1621162682614226944&utm_source=wechat_session  
https://www.zywvvd.com/notes/tools/zotero/zotero-translate/zotero-translate/  
https://zotero.yuque.com/staff-gkhviy/pdf-trans  
https://blog.csdn.net/qq_61529681/article/details/134517969  
https://www.cnblogs.com/smileglaze/p/16742252.html  


engines.json文件内容：  

```json
[
	{
		"_name": "CNKI新版",
		"_alias": "CNKI",
		"_description": "CNKI新版",
		"_icon": "http://kns8.cnki.net/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "http://kns.cnki.net/kns8/DefaultResult/Index?dbcode=SCDB&kw={z:title}&korder=SU",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "http://kns8.cnki.net/favicon.ico"
	},
	{
		"_name": "南京师范大学图书馆",
		"_alias": "南京师范大学图书馆",
		"_description": "南京师范大学图书馆",
		"_icon": "http://lib.njnu.edu.cn/images/njnulogo.png",
		"_hidden": false,
		"_urlTemplate": "http://opac.njnu.edu.cn/opac/openlink.php?strSearchType=title&match_flag=forward&historyCount=1&strText={z:title}&doctype=ALL&with_ebook=on&displaypg=20&showmode=list&sort=CATA_DATE&orderby=desc&location=ALL&csrf_token=%29lW1vOCx%7B%2F",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "http://lib.njnu.edu.cn/images/njnulogo.png"
	},
		{
		"_name": "豆瓣读书",
		"_alias": "豆瓣读书",
		"_description": "豆瓣读书",
		"_icon": "https://book.douban.com/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://search.douban.com/book/subject_search?search_text={z:title}&cat=1001",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://book.douban.com/favicon.ico"
	},		
		{
		"_name": "国学数典",
		"_alias": "国学数典",
		"_description": "国学数典",
		"_icon": "https://bbs.ugxsd.com/static/image/common/logo_gxsd.png",
		"_hidden": false,
		"_urlTemplate": "https://bbs.ugxsd.com/search.php?mod=forum&searchid=2634&orderby=lastpost&ascdesc=desc&searchsubmit=yes&kw={z:title}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://bbs.ugxsd.com/static/image/common/logo_gxsd.png"
	},		
		{
		"_name": "读秀图书",
		"_alias": "读秀图书",
		"_description": "读秀图书",
		"_icon": "https://book.duxiu.com/images/small0408.jpg",
		"_hidden": false,
		"_urlTemplate": "https://book.duxiu.com/search?Field=all&channel=search&sw={z:title}&ecode=utf-8&edtype=&searchtype=&view=0",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://book.duxiu.com/images/small0408.jpg"
	},
	{
		"_name": "Obsidian",
		"_alias": "Obsidian",
		"_description": "在Obsidian中搜索",
		"_icon": "https://obsidian.md/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "obsidian://search?vault=Zotero&query={z:title}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://obsidian.md/favicon.ico"
	},	
	{
		"_name": "Open Notes",
		"_alias": "Open Notes",
		"_description": "笔记路径放在Rights字段",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201017133213.png",
		"_hidden": false,
		"_urlTemplate": "file:///{z:rights}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201017133213.png"
	},
		{
		"_name": "TOC of Notes",
		"_alias": "TOC of Notes",
		"_description": "打开所有笔记的目录",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201025110921.png",
		"_hidden": false,
		"_urlTemplate": "file:////Users/a40781/Zotero/Obsidian",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201025110921.png"
	},
		{
		"_name": "N-Connected Papers",
		"_alias": "Connected Papers文献网络",
		"_description": "Connected Papers文献网络",
		"_icon": "https://www.connectedpapers.com/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://www.connectedpapers.com/search?q={z:title}+{z:year}",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://www.connectedpapers.com/favicon.ico"
	},
	{
		"_name": "CrossRef",
		"_alias": "CrossRef",
		"_description": "CrossRef Search",
		"_icon": "https://www.crossref.org/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "http://crossref.org/openurl?{z:openURL}&pid=zter:zter321",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "http://crossref.org/favicon.ico"
	},
	{
		"_name": "Google Scholar",
		"_alias": "Google Scholar",
		"_description": "Google Scholar Search",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016230633.png",
		"_hidden": false,
		"_urlTemplate": "http://scholar.google.com/scholar?as_q=&as_epq={z:title}&as_occt=title&as_sauthors={rft:aufirst?}+{rft:aulast?}&as_ylo={z:year?}&as_yhi={z:year?}&as_sdt=1.&as_sdtp=on&as_sdtf=&as_sdts=22&",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016230633.png"
	},
	{
		"_name": "Google Scholar - Title Only",
		"_alias": "Google Scholar",
		"_description": "Google Scholar Search - Title Only",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016230633.png",
		"_hidden": false,
		"_urlTemplate": "http://scholar.google.com/scholar?as_q=&as_epq={z:title}&as_occt=title&as_sdt=1.&as_sdtp=on&as_sdtf=&as_sdts=22&",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016230633.png"
	},
		{
		"_name": "Google Patent",
		"_alias": "Google Scholar",
		"_description": "Google Patent - Search",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201017210030.png",
		"_hidden": false,
		"_urlTemplate": "https://patents.google.com/?q=({z:title})&oq=({z:title})",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201017210030.png"
	},
		{
		"_name": "Semantic Scholar - DOI",
		"_alias": "Semantic Scholar",
		"_description": "Semantic Scholar",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201017165044.png",
		"_hidden": false,
		"_urlTemplate": "https://api.semanticscholar.org/{z:DOI}",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201017165044.png"
	},
	{
		"_name": "Google",
		"_alias": "Google",
		"_description": "Google Search",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016230909.png",
		"_hidden": false,
		"_urlTemplate": "https://www.google.com/#q={z:title}+{rft:aufirst?}+{rft:aulast?}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016230909.png"
	},
	{
		"_name": "Web of Science",
		"_alias": "WOS",
		"_description": "Web of Science Search",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016231207.png",
		"_hidden": false,
		"_urlTemplate": "http://ws.isiknowledge.com/cps/openurl/service?url_ver=Z39.88-2004&{z:openURL}",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016231207.png"
	},
	{
		"_name": "WOS Citing Articles",
		"_alias": "WOS Citing Articles",
		"_description": "Web of Science Citing Articles Search",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016231207.png",
		"_hidden": false,
		"_urlTemplate": "http://ws.isiknowledge.com/cps/openurl/service?url_ver=Z39.88-2004&svc_val_fmt=info:ofi/fmt:kev:mtx:sch_svc&svc.citing=yes&{z:openURL}",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016231207.png"
	},
	{
		"_name": "WOS Related Articles",
		"_alias": "WOS Related Articles",
		"_description": "Web of Science Related Articles Search",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016231207.png",
		"_hidden": false,
		"_urlTemplate": "http://ws.isiknowledge.com/cps/openurl/service?url_ver=Z39.88-2004&svc_val_fmt=info:ofi/fmt:kev:mtx:sch_svc&svc.related=yes&{z:openURL}",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016231207.png"
	},
	{
		"_name": "Sci-Hub DOI",
		"_alias": "Sci-Hub DOI",
		"_description": "SciHub Lookup Lookup",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016200912.png",
		"_hidden": false,
		"_urlTemplate": "http://sci-hub.shop/{z:DOI}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016200912.png"
	},
	{
		"_name": "Sci-Hub URL",
		"_alias": "Sci-Hub URL",
		"_description": "SciHub URL Lookup",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016200912.png",
		"_hidden": false,
		"_urlTemplate": "http://sci-hub.shop/{z:url}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016200912.png"
	},
			{
		"_name": "Sci-Hub最新网址",
		"_alias": "Sci-Hub最新网址",
		"_description": "Sci-Hub最新网址",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016200912.png",
		"_hidden": false,
		"_urlTemplate": "https://iseex.github.io/scihub/",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016200912.png"
	},
		{
		"_name": "Connected Papers（手动输入doi）",
		"_alias": "Connected Papers文献网络",
		"_description": "Connected Papers文献网络",
		"_icon": "https://www.connectedpapers.com/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://www.connectedpapers.com",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://www.connectedpapers.com/favicon.ico"
	},
		{
		"_name": "Connected Papers（Title Only）",
		"_alias": "Connected Papers文献网络",
		"_description": "Connected Papers文献网络",
		"_icon": "https://www.connectedpapers.com/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://www.connectedpapers.com/search?q={z:title}",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://www.connectedpapers.com/favicon.ico"
	},
		{
		"_name": "iJournal期刊影响因子（首选1）",
		"_alias": "iJournal期刊影响因子",
		"_description": "期刊影响因子查询（仅支持搜索ISSN）",
		"_icon": "https://ijournal.topeditsci.com/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://ijournal.topeditsci.com/search?keywordType=title&keyword={rft:jtitle}&ifStart2019=&ifEnd2019=&jcr=&sub=&isDomestic=&selfCitingRate=all&compatriotRate=all&totalReviewRatio=all&categoryId=&pageNum=1&order=&orderType=",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://ijournal.topeditsci.com/favicon.ico"
	},
		{
		"_name": "JustScience影响因子（首选2）",
		"_alias": "JustScience期刊影响因子（首选2）",
		"_description": "JustScience期刊影响因子（首选2）",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201017105103.png",
		"_hidden": false,
		"_urlTemplate": "https://sci.justscience.cn/index.php?q={rft:jtitle}&sci=1",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201017105103.png"
	},
		{
		"_name": "中文期刊搜索",
		"_alias": "中文期刊搜索",
		"_description": "JustScience中文期刊搜索",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201017105103.png",
		"_hidden": false,
		"_urlTemplate": "https://sci.justscience.cn/index.php?q={rft:jtitle}&sci=0",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201017105103.png"
	},
		{
		"_name": "Letpub期刊搜索",
		"_alias": "Letpub期刊搜索",
		"_description": "Letpub期刊搜索（仅支持搜索ISSN）",
		"_icon": "https://www.letpub.com.cn/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "http://www.letpub.com.cn/index.php?page=journalapp&view=search&searchname={rft:jtitle}",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://www.letpub.com.cn/favicon.ico"
	},
		{
		"_name": "期刊影响因子查询",
		"_alias": "期刊影响因子查询",
		"_description": "期刊影响因子查询（仅支持搜索ISSN）",
		"_icon": "https://www.xueky.com/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://www.xueky.com/rank.php?qk={rft:jtitle}",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://www.xueky.com/favicon.ico"
	},
			{
		"_name": "期刊近五年影响因子",
		"_alias": "期刊近五年影响因子",
		"_description": "期刊近五年影响因子",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016200307.png",
		"_hidden": false,
		"_urlTemplate": "http://www.greensci.net/search?kw={rft:jtitle}",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016200307.png"
	},
		{
		"_name": "Unpaywall",
		"_alias": "Unpaywall",
		"_description": "Unpaywall Lookup",
		"_icon": "https://oadoi.org/static/img/favicon.png",
		"_hidden": false,
		"_urlTemplate": "https://oadoi.org/{z:DOI}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "https://oadoi.org/static/img/favicon.png"
	},
		{
		"_name": "LibGen",
		"_alias": "LibGen",
		"_description": "Look up books on Library Genesis",
		"_icon": "http://gen.lib.rus.ec/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "http://gen.lib.rus.ec/search.php?req={rft:aufirst?}+{rft:aulast?}+{rft:title?}+{z:ISBN?}",
		"_urlParams": [],
		"_method": "GET",
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:book",
			"xmlns": "http://a9.com/-/spec/opensearch/1.1/",
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "http://gen.lib.rus.ec/favicon.ico"
	},
		{
		"_name": "Wikipedia",
		"_alias": "Wikipedia",
		"_description": "Wikipedia",
		"_icon": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016232045.png",
		"_hidden": false,
		"_urlTemplate": "https://zh.wikipedia.org/wiki/{z:title}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://figurebed-iseex.oss-cn-hangzhou.aliyuncs.com/img/20201016232045.png"
	},
	{
		"_name": "ProQuest",
		"_alias": "ProQuest",
		"_description": "ProQuest Search",
		"_icon": "https://www.proquest.com/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "http://proquest.umi.com/pqdweb?SQ=TI(+{z:title}+)+YR(+{z:year?}+)&RQT=305&querySyntax=PQ&searchInterface=1&moreOptState=CLOSED&TS=1139706667&JSEnabled=1&DBId=-1",
		"_urlParams": [],
		"_urlNamespaces": {
			"rft": "info:ofi/fmt:kev:mtx:journal",
			"z": "http://www.zotero.org/namespaces/openSearch#"
		},
		"_iconSourceURI": "http://www.proquest.com/favicon.ico"
	},
			{
		"_name": "Zlibrary Book",
		"_alias": "Zlibrary Book",
		"_description": "Zlibrary Book",
		"_icon": "https://z-lib.org/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://b-ok.cc/s/{z:title}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://z-lib.org/favicon.ico"
	},
		{
		"_name": "Zlibrary Articles",
		"_alias": "Zlibrary Articles",
		"_description": "Zlibrary Articles",
		"_icon": "https://z-lib.org/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://booksc.xyz/s/{z:title}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://z-lib.org/favicon.ico"
	},
		{
		"_name": "SooPAT专利搜索",
		"_alias": "SooPAT专利搜索",
		"_description": "SooPAT专利搜索",
		"_icon": "http://www.soopat.com/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "http://www.soopat.com/Home/Result?SearchWord={z:title}&FMZL=Y&SYXX=Y&WGZL=Y&FMSQ=Y",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "http://www.soopat.com/favicon.ico"
	},
	{
		"_name": "百度学术搜索",
		"_alias": "BaiDu",
		"_description": "百度学术搜索",
		"_icon": "http://xueshu.baidu.com/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://xueshu.baidu.com/s?wd={z:title}&rsv_bp=0&tn=SE_baiduxueshu_c1gjeupa&rsv_spt=3&ie=utf-8&f=8&rsv_sug2=0&sc_f_para=sc_tasktype%3D%7BfirstSimpleSearch%7D",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "http://xueshu.baidu.com/favicon.ico"
	},
			{
		"_name": "万方搜索",
		"_alias": "万方搜索",
		"_description": "万方搜索",
		"_icon": "http://www.wanfangdata.com.cn/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "http://www.wanfangdata.com.cn/search/searchList.do?searchType=all&showType=&pageSize=&searchWord={z:title}&isTriggerTag=",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "http://www.wanfangdata.com.cn/favicon.ico"
	},
	{
		"_name": "谷粉学术",
		"_alias": "谷粉学术",
		"_description": "谷粉学术",
		"_icon": "https://img.99lb.net/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://cc.gufenxueshu.com/scholar?q={z:title}",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://img.99lb.net/favicon.ico"
	},
		{
		"_name": "Glgoo学术",
		"_alias": "Glgoo学术",
		"_description": "Glgoo学术",
		"_icon": "https://xue.glgoo.org/favicon.ico",
		"_hidden": false,
		"_urlTemplate": "https://xue.xiayige.org/scholar?hl=zh-CN&q={z:title}&btnG=&lr=",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://xue.glgoo.org/favicon.ico"
	},
	{
		"_name": "CrossRef Lookup",
		"_alias": "CrossRef",
		"_description": "CrossRef Search Engine",
		"_icon": "file:///C:/Users/86153/Zotero/locate/CrossRef%20Lookup.ico",
		"_hidden": false,
		"_urlTemplate": "https://crossref.org/openurl?{z:openURL}&pid=zter:zter321",
		"_urlParams": [],
		"_urlNamespaces": {
			"z": "http://www.zotero.org/namespaces/openSearch#",
			"": "http://a9.com/-/spec/opensearch/1.1/"
		},
		"_iconSourceURI": "https://crossref.org/favicon.ico"
	},
]
```

转载请注明：[南梦婷的博客](https://norah2.github.io) » [点击阅读原文](https://norah2.github.io/2023/03/12/Zotero_install/) 


<!--本文用到的链接-->

[pt_01]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/83_Zotero_install/01.png  
[pt_02]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/83_Zotero_install/02.png  
[pt_03]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/83_Zotero_install/03.png  
[pt_04]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/83_Zotero_install/04.png  
[pt_05]: https://nora-blogimg.oss-cn-hangzhou.aliyuncs.com/BlogImage/83_Zotero_install/05.png  


[link_01]: https://www.zotero.org/  
[link_02]: https://www.zotero.org/download/connectors  
[link_03]: http://zotfile.com/  
[link_04]: https://github.com/404neko/ZoteroQuickLookReload  
[link_05]: https://github.com/QL-Win/QuickLook/releases  
[link_06]: https://github.com/l0o0/jasminum  
[link_07]: https://www.pdflabs.com/tools/pdftk-server/  
[link_08]: https://github.com/windingwind/zotero-pdf-translate  
[link_09]: https://fanyi-api.baidu.com/api/trans/product/index  
[link_10]: https://github.com/MuiseDestiny/zotero-reference  
[link_11]: https://github.com/bwiernik/zotero-shortdoi/releases  
[link_12]: https://github.com/windingwind/zotero-better-notes/releases  

{% endraw %}

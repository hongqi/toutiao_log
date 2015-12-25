# toutiao_analytics

##1.代码部署

###安装代码
在html头部安装如下代码：

	
	<script type="text/javascript">
		var _taq = _taq || [];
		_taq.push(['trackinit', device, app]);
		//device: 设备类型，值有mobile,pc。该项必选。
		//app: app的类型，值有web,wap。该项必选。
		(function() {
    		var ta = document.createElement('script'); ta.type = 'text/javascript'; ta.async = true;
    		ta.src = document.location.protocol + '//' + 's0.pstatp.com/adstatic/resource/landing_page_log/dist/1.0.1/toutiao-analytics.js';
    		var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ta, s);
		})();
	</script>
	
安装实例：

	<!DOCTYPE html>
	<html>
	<head>
    	<title>New Document</title>
    	<meta charset=utf-8>
    	<meta name=description content="">
    	<meta name=keywords content="">
    	<!-- 您网站的样式表/脚本 -->
    	<script type="text/javascript">
        	var _taq = _taq || [];
        	_taq.push(['trackinit', 'mobile', 'wap']);//手机客户端
        	(function() {
            	var ta = document.createElement('script'); ta.type = 'text/javascript'; ta.async = true;
            	ta.src = document.location.protocol + '//' + 's0.pstatp.com/adstatic/resource/toutiao_log/dist/1.0.1/toutiao-analytics.js';
            	var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ta, s);
        	})();
    	</script>
	</head>
	<body>
    	<!-- 您网站的页面代码 -->
	</body>
	</html>

###安装说明

	1. 请将代码添加到网站全部页面的</head>标签前；
	2. 建议在header.htm类似的页头模板页面中安装，以达到一处安装，全站皆有的效果；
	3. 如需在JS文件中调用统计分析代码，请直接去掉以下代码首尾的<script type="text/javascript">与</script>后，放入JS文件中即可。

###注意
 	代码中会有1.0.1的版本号，如果统计代码有大的改动，会通知大家更新为新的版本。这个版本号不会频繁变动的。
 
##2.事件跟踪

###API介绍
	用于触发某个事件，如某个按钮的点击，或播放器的播放/停止，以及游戏的开始/暂停等。
	Flash中所有的的事件都可以通过该接口来统计，只要在响应用户操作时，通过Flash调用JS接口就可以了。
	事件跟踪的数据不会被记入到页面PV中，适合用来统计所有的不需要看做PV的页面事件。
	
###适用场景
	- 所有点击事件
	- 在页面加载时发送页面的PV事件
	
###设置步骤 
1. 在dom元素上添加属性
	
	目前支持的属性有：data-view, data-click。data-view是当前页面的PV统计，data-click是页面中点击事件的统计
	注: 一个页面有且只有一个data-view属性，可以有多个data-click属性
		
	data-view参数列表：

	参数     | 值        | 含义      | 是否必须
	:--------: | :--------: | :--------: | :--------:
	page     | 用户自定义	 | 页面的名称 | 是	
	etype     | pv	 | view| 是	
	option     | 用户自定义扩展参数	 | 可选参数 | 否
	
		使用示例
		<section data-view="{"page":"index","etype":"view","option":{"a":"http://m.toutiao.com","b":2}}">
		<!-- 您页面的代码 -->
		</section>	
	
	---
	data-click参数列表：
	
	参数     | 值        | 含义      | 是否必须
	:--------: | :-------- | :--------: | :--------:
	page     | 用户自定义	 | 页面的名称 | 是	
	etype     | click:点击 <br> dbclk: 双击 <br> zoom: 放大缩小 <br> scroll: 滑动 | view| 是
	concrete     | search: 搜索 <br> register: 注册 <br> download: 下载 <br> login: 登陆 <br>等| 可选参数 | 否
	option     | 用户自定义扩展参数 | 其他可选参数	| 否

		使用示例
		<div data-click="{"page":"index","etype":"view",concrete:"search","option":{"a":"http://m.toutiao.com","b":2}}">
		</div>

2. 在js代码中添加
			
	* _taq.push
	
			在合适的位置调用如下跟踪代码:
		
			_taq.push(['trackevent', page, etype, concrete, target, option]);
			page: 页面名称，用户自定义值。该项必选。
			etype: 事件类型，值有pv,click,dbclk,zoom,scroll。其中pv指：通过js代码发送的页面展现，替代data-view的方式。该项必选。
			concrete: 具体事件，值有search,register,login, download等由用户自定义。该项必选。
			target: 触发事件的dom节点对象。除了etype是pv以外的所有类型都需要传递。
			option: 可选参数。值可以为字符串，如:"tn=1&pn=2",也可以为对象,如:{tn:1,pn:2}。该项可选。
			
		---
	
			使用示例：
		
			_taq.push(['trackevent', 'index', 'pv', 'login']);
			_taq.push(['trackevent', 'index', 'click', 'login', '{"tn":"aa","pn":"bb","from":"http%3A%2F%2Ftest.com"}']);
			_taq.push(['trackevent', 'index', 'click', 'login', $target,{"tn":"aa","pn":"bb","from":"http%3A%2F%2Ftest.com"}]);
		
	* ta.send
			
			注：使用send方法需要写到window.onload事件里
			参数定义见上面的跟踪代码,send方法在全局对象ta上
			
		---
		
			使用示例：
			
			var data = {
				page: "index",
    			etype: "click",
    			concrete: "login",
    			target: target,//除了etype是pv的其他事件都需要发送target
    			option: {
        			tn: 1,
        			pn: 2
    			}
			}
 
			window.onload = function(){
    			ta.send(data);
    		} 
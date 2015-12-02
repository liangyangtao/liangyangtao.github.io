---
layout: post
title: Httpclient模拟登陆新浪微博
category: 网络爬虫
date: 2015-08-26
---

标签： 网络爬虫 信息采集 新浪微博

<!-- more -->


###代码地址：

####2015年11月重新整理了下代码

代码地址：

https://github.com/liangyangtao/ubk_weiboSpider

查看src/main/java/com/unbank/weibo/login/WeiboLoginByHttpClinet.java即可

具体代码已上面的为主
###实现步骤

###第一步：

访问 http://weibo.com/login.php  使得Cookie 里包含login_sid_t ，TC_Ugrow_G0

###第二步：
访问

http://login.sina.com.cn/sso/prelogin.php?entry=weibo&callback=sinaSSOController.preloginCallBack&su=Njc0NjEzNDM4JTQwcXEuY29t&rsakt=mod&checkpin=1&client=ssologin.js(v1.4.18)&_=
加时间戳

获取servertime ，pcid，pubkey，rsakv，nonce 这四个值

###第三步：
 将用户名需要 base 64 ecode

 让后将密码需要加密，这里调用新浪的js代码进行加密

###第四步:

   提交参数进行登陆
   获取 校验 url

###第5步

   访问第4步得到的URL 操作后如果看到自己的uid 说明登陆成功，就可以采集了

###代码地址：

https://github.com/liangyangtao/ubk_weiboSpider


####关键代码

>
{% highlight  java %}

 public boolean login(String username, String password) {
		/**
		 * 第一步：访问 http://weibo.com/login.php
		 *
		 *
		 * 使得Cookie 里包含login_sid_t ，TC_Ugrow_G0
		 *
		 *
		 *
		 * */
		String html = getHtml(httpClient, loginUrl, getCookiesString());

		// String login_sid_t = StringUtils.substringBetween(cookies,
		// "login_sid_t=   ", ";");
		// System.out.println("login_sid_t    :  " + login_sid_t);
		//
		// String TC_Ugrow_G0 = StringUtils.substringBetween(cookies,
		// "TC-Ugrow-G0=", ";");
		// System.out.println("TC_Ugrow_G0     " + TC_Ugrow_G0);

		long DateTime = new Date().getTime();
		// System.out.println(DateTime);
		callUrl = callUrl + DateTime;

		/**
		 * 第二步：http://login.sina.com.cn/sso/prelogin.php?entry=weibo&callback=
		 * sinaSSOController
		 * .preloginCallBack&su=Njc0NjEzNDM4JTQwcXEuY29t&rsakt=mod
		 * &checkpin=1&client=ssologin.js(v1.4.18)&_=
		 *
		 * 获取servertime ，pcid，pubkey，rsakv，nonce
		 *
		 * */
		html = getHtml(httpClient, callUrl, getCookiesString());
		// sinaSSOController.preloginCallBack({"retcode":0,"servertime":1440403744,"pcid":"xd-29af00e904fa0c7e2736ea8cc6b7fcd99717","nonce":"ULSRFO","pubkey":"EB2A38568661887FA180BDDB5CABD5F21C7BFD59C090CB2D245A87AC253062882729293E5506350508E7F9AA3BB77F4333231490F915F6D63C55FE2F08A49B353F444AD3993CACC02DB784ABBB8E42A9B1BBFFFB38BE18D78E87A0E41B9B8F73A928EE0CCEE1F6739884B9777E4FE9E88A1BBE495927AC4A799B3181D6442443","rsakv":"1330428213","showpin":0,"exectime":10})
		String servertime = StringUtils.substringBetween(html, "servertime\":",
				",");
		// System.out.println("servertime        " + servertime);
		String pcid = StringUtils.substringBetween(html, "pcid\":\"", "\",\"");
		// System.out.println("pcid    " + pcid);
		String pubkey = StringUtils.substringBetween(html, "pubkey\":\"",
				"\",\"");
		// System.out.println("pubkey     " + pubkey);
		String rsakv = StringUtils.substringBetween(html, "rsakv\":\"", "\"");
		// System.out.println("rsakv       " + rsakv);
		String nonce = StringUtils.substringBetween(html, "nonce\":\"", "\"");
		// System.out.println("nonce        " + nonce);
		/**
		 *
		 * 第三步： 1 用户名需要 base 64 ecode
		 *
		 * 2 密码需要加密
		 *
		 *
		 *
		 * */
		// 1
		String su = encodeAccount(username);
		// System.out.println("____" + su + "____dfdsfds");
		// 2
		// String messageg = servertime + "\t" + nonce + "\n" + password;
		String sp = ssorskPassword(pubkey, servertime, nonce, password);
		// System.out.println(sp.trim());
		/**
		 * 第四步 ：提交参数进行登陆
		 *
		 * 获取 校验 url
		 *
		 * */
		Map<String, String> params = new HashMap<String, String>();
		params.put("entry", "weibo");
		params.put("gateway", "1");
		params.put("from", "");
		params.put("savestate", "7");
		params.put("useticket", "1");
		params.put("pagerefer", "");
		params.put("vsnf", "1");
		params.put("su", su.trim());
		params.put("service", "miniblog");
		params.put("servertime", servertime);
		params.put("nonce", nonce);
		params.put("pwencode", "rsa2");
		params.put("rsakv", rsakv);
		params.put("sp", sp.trim());
		params.put("sr", "1920*1080");
		params.put("encoding", "UTF-8");
		params.put("prelt", "35");
		params.put(
				"url",
				"http://weibo.com/ajaxlogin.php?framelogin=1&callback=parent.sinaSSOController.feedBackUrlCallBack");
		params.put("returntype", "META");
		html = post(httpClient, ssologinUrl, params, "utf-8",
				getCookiesString());
		// System.out.println(html);
		/**
		 * 获取到结果 <html> <head> <title>Sina Passport</title> <meta
		 * http-equiv="Content-Type" content="text/html; charset=GBK" />
		 *
		 * <script charset="utf-8"
		 * src="http://i.sso.sina.com.cn/js/ssologin.js"></script> </head>
		 * <body> Signing in ... <script>
		 * try{sinaSSOController.setCrossDomainUrlList({"retcode":0,"arrURL":[
		 * "http:\/\/crosdom.weicaifu.com\/sso\/crosdom?action=login&savestate=1477987419","http:\/\/passport.97973.com\/sso\/crossdomain?action=login&savestate=1477987419","http:\/\/passport.weibo.cn\/sso\/crossdomain?action=login&savestate=1"]});}catch(e){}try{sinaSSOController.crossDomainAction('login',function(){location.replace('http://passport.weibo.com/wbsso/login?ssosavestate=1477987419&url=http%3A%2F%2Fweibo.com%2Fajaxlogin.php%3Fframelogin%3D1%26callback%3Dparent.sinaSSOController.feedBackUrlCallBack&ticket=ST-MjQ3ODk5MzQ2NQ==-1446451419-xd
		 * - 4 9 B B 0 2 8 C A F C 0 3 9 E 1 2 E B 9 9 9 C A A 6 0 3 F A 4 A & r
		 * e t c o d e = 0 ' ) ; } ) ; } c a t c h ( e ) { } </script> </body>
		 * </html>
		 *
		 * */

		String url = StringUtils.substringBetween(html, "location.replace('",
				"');");
		// System.out.println(url);
		// url =
		// "http://weibo.com/ajaxlogin.php?framelogin=1&callback=parent.sinaSSOController.feedBackUrlCallBack&sudaref=weibo.com";
		/**
		 * 第5步： 访问URL 操作后如果看到自己的uid 说明登陆成功，就可以采集了
		 */
		html = getHtml(httpClient, url, getCookiesString());
		System.out.println(html);
		System.out.println(getCookiesString());
		return false;

	}


{% endhighlight %}





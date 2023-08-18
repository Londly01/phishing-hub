![微信截图_20230818221924](https://github.com/Londly01/phishing-hub/assets/118274389/8bcb30b9-c5f5-44fa-927c-c9c45892ffec)



## 0x00 收集思路

        常规方法：目标根域、控股子公司根域名、分支机构根域 -> 子域名 -> ip全端口 -> 连续ip -> web爬虫及web路径
        
        PS：连续ip意思是：比如收集到的ip是xx.xx.xx.109和xx.xx.xx.111 那么xx.xx.xx.110大概率也是目标单位资产

## 0x01 根域收集

        enscan工具:https://github.com/wgpsec/ENScan
        
        记得加上分支机构根域获取参数，也可以自己写个脚本获取某某查的信息，下面是自己写的部分核心源码，获取根域信息。
        
        
```
        def getGId(companyName):
	try:
		company = ""
		gid = ""
		header = {
		"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:101.0) Gecko/20100101 Firefox/101.0",
		"Referer":"https://www.tianyancha.com/",
		"Cookie":"XXXX"}
		url = f"https://www.tianyancha.com/search?key={quote(companyName)}"
		html = requests.get(url=url, headers=header, verify=False).text

		doc = pq(html)

		aDivs = doc('.index_list-wrap___axcs .index_name__qEdWi').items()

		if aDivs:
			div = list(aDivs)

			if div:
				url = div[0].find('a').attr("href")

				company = div[0].find('a span').text()

				gid = url.split("/")[-1]
		while True:
			if company=='' or gid=='':
				company = input("$ Input company > ")
				getGId(company)
			else:
				break
		return company,gid
	except Exception as e:
		return company,gid
	except KeyboardInterrupt:
		exit()

# 获取控股子公司
def subsidiary(company,gid,rate,delay):
	try:
		myDatas = []
		if rate<90:
			rate = "50"
		elif rate<100:
			rate = "90"
		else:
			rate = "100"
		rateList = {"50":"4","90":"5","100":"6"}
		header = {
		"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36",
		"Referer":"https://www.tianyancha.com/",
		"content-type":"application/json",
		"version":"TYC-Web",
		"x-tycid":"1edbe4006ea111eca2d6fdac49e21176",
		"Cookie":"aliyungf_tc=XXXX"}
		url = f"https://capi.tianyancha.com/cloud-company-background/company/investListV2?_={int(round(time.time() * 1000))}"
		data = {"gid":gid,"pageSize":10,"pageNum":1,"province":"-100","percentLevel":rateList[rate],"category":"-100"}
		resData = requests.post(url=url,json=data,headers=header,verify=False).json()["data"]
		total = resData['total']
		results = resData['result']
		for i in results:
			with open(r'kggs.txt', 'a') as f:
				f.write(str(i['id']) + '\n')

		#先获取第一页
		if results:
			for item in results:
				myDatas.append([company,item['percent'],item['name'],item['regStatus']])

			#页数
			if total%10==0:
				pageNums = total//10
			else:
				pageNums = total//10 + 1
			#遍历页数获取
			if pageNums >= 2:
				for page in range(2,pageNums+1):
					time.sleep(delay/10)
					data = data = {"gid":gid,"pageSize":10,"pageNum":page,"province":"-100","percentLevel":rateList[rate],"category":"-100"}
					resData = requests.post(url=url,json=data,headers=header,verify=False).json()["data"]
					results = resData['result']

					if results:
						for item in results:
							myDatas.append([company,item['percent'],item['name'],item['regStatus']])
					# for i in results:
					# 	with open(r'kggs.txt', 'a') as f:
					# 		f.write(str(i['id']) + '\n')
		return myDatas
	except Exception as e:
		return myDatas
	except KeyboardInterrupt:
		exit()
```
        
## 0x02 子域名获取

        推荐自己写的项目：https://github.com/Londly01/Londly02 可以把里面的API全部配置上
        
## 0x03 全端口扫描

       
        推荐自己写的项目：https://github.com/Londly01/Londly01-safety-tool 
        
        masscan有丢包的情况，goby效果好一些

## 0X04 WEB路径爆破及爬虫
        
        爬虫：https://github.com/Qianlitp/crawlergo
        
        目录扫描：https://github.com/maurosoria/dirsearch
        
        
        
        
## 0x04 其他收集技巧

        谷歌语法：

        site:*.XXX.COM  intext: vpn | 用户名 | 密码 | 帐号 | 默认密码  | 后台 | 管理 | 登录
        
        site:*.XXX.COM intext:管理|后台|登陆|用户名|密码|验证码|系统|帐号|admin|login|sys|managetem|password|username
        
        site:*.XXX.COM  intext:后台 | 管理 | 登录
        
        site:*.XXX.COM intitle:后台 | 管理 | 登录
        
        site:*.XXX.COM inurl:admin|login|logon|username|manager
        
        site:.XXX.COM inurl:file|load|editor|Files
        
        site:*.XXX.COM ext:xml | ext:conf | ext:cnf | ext:reg | ext:inf | ext:rdp | ext:cfg | ext:txt | ext:ora | ext:ini  配置文件泄露

        site:*.XXX.COM ext:log 日志泄露
        
        site:*.XXX.COM inurl:log

        site:*.XXX.COM ext:bkf | ext:bkp | ext:bak | ext:old | ext:backup 备份和历史文件泄露
        
        site:*.XXX.COM intext: vpn | 用户名 | 密码 | 帐号 | 默认密码
        
        空间测绘各种语法：自己探索吧，包括后台，开放端口，敏感信息，js，等等

## 0x05 漏洞检测及利用

       我写的2款工具一套打下来
       
       https://github.com/Londly01/Londly01-safety-tool 
       
       https://github.com/Londly01/Londly02
       
       关于弱口令：可以通过空间测绘找到各种后台，然后爆破账户密码，后台想办法拿shell
       
       在线字典生成网站：https://www.ddosi.org/pass8/index.html
       
       

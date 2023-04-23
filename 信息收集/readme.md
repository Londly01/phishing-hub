## 0x00 收集思路

        常规方法：目标根域、控股子公司根域名、分支机构根域 -> 子域名 -> ip全端口 -> 连续ip -> web爬虫及web路径
        
        PS：连续ip意思是：比如收集到的ip是xx.xx.xx.109和xx.xx.xx.111 那么xx.xx.xx.110大概率也是目标单位资产

## 0x01 根域收集

        enscan工具:https://github.com/wgpsec/ENScan
        
        记得加上分支机构根域获取参数
        
## 0x02 子域名获取

        推荐自己写的项目：https://github.com/Londly01/Londly02 可以把里面的API全部配置上
        
## 0x03 全端口扫描

       
        推荐自己写的项目：https://github.com/Londly01/Londly01-safety-tool 
        
        masscan有丢包的情况，goby效果好一些

## 0X04 WEB路径爆破及爬虫
        
        爬虫：https://github.com/Qianlitp/crawlergo
        
        目录扫描：https://github.com/maurosoria/dirsearch
        
        
        
        
## 0x04 其他收集技巧

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



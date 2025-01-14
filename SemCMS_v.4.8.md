Versions of Semcms before 4.8 can upload arbitrary files on windows servers via NTFS to cause code execution hazards

First in the background of the cms image library management there is an interface for file upload

![image-20240310163505101](https://github.com/XiLitter/cms/blob/main/1.png)

Use the packet capture tool to capture packets and upload a file containing malicious code

![image-20240310163634777](https://github.com/XiLitter/cms/blob/main/2.png)

The packet has been uploaded successfully

![image-20240310163746525](https://github.com/XiLitter/cms/blob/main/3.png)

The server already has a php file, but the content is empty, and then re-send the package, modify some of the contents as follows

![image-20240310163921447](https://github.com/XiLitter/cms/blob/main/4.png)

Send the package, can also upload successfully

![image-20240310164025663](https://github.com/XiLitter/cms/blob/main/5.png)

At this point, the uploaded content has overwritten the previously uploaded php file

To access the target address is: http://localhost:7080/images/prdoucts/111.png.php

![image-20240310164232450](https://github.com/XiLitter/cms/blob/main/6.png)

Able to execute code successfully
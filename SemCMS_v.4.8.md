Versions of Semcms before 4.8 can upload arbitrary files on windows servers via NTFS to cause code execution hazards

First in the background of the cms image library management there is an interface for file upload

![image-20240310163505101](C:\Users\86183\AppData\Roaming\Typora\typora-user-images\image-20240310163505101.png)

Use the packet capture tool to capture packets and upload a file containing malicious code

![image-20240310163634777](C:\Users\86183\AppData\Roaming\Typora\typora-user-images\image-20240310163634777.png)

The packet has been uploaded successfully

![image-20240310163746525](C:\Users\86183\AppData\Roaming\Typora\typora-user-images\image-20240310163746525.png)

The server already has a php file, but the content is empty, and then re-send the package, modify some of the contents as follows

![image-20240310163921447](C:\Users\86183\AppData\Roaming\Typora\typora-user-images\image-20240310163921447.png)

Send the package, can also upload successfully

![image-20240310164025663](C:\Users\86183\AppData\Roaming\Typora\typora-user-images\image-20240310164025663.png)

At this point, the uploaded content has overwritten the previously uploaded php file

To access the target address is: http://localhost:7080/images/prdoucts/111.png.php

![image-20240310164232450](C:\Users\86183\AppData\Roaming\Typora\typora-user-images\image-20240310164232450.png)

Able to execute code successfully
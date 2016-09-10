# 设置xampp的mail服务

xampp使得windows下搭建web服务器环境变得异常简单。但是在邮件服务器这块还需要一些配置才能正常运作。

首先需要配置的事php.ini,在[mail function]里去掉邮件功能的注释。并对服务器、端口等做相应配置。

下面是基于XAMPP for Windows Version 1.7.1的配置信息：
```
[mail function]
; For Win32 only.
SMTP = localhost
smtp_port = 25

; For Win32 only.
sendmail_from = me@example.com
```
一旦把sendmail_from设置为一个email地址，PHP会强制所有发送方地址为设置值。在XAMPP Control Panel里开启mail服务Mercury后，在PHP中仍然可能会遇到这样的错误
```
553 We do not relay non-local mail
```
这需要对Mercury的配置做些修改
Configuration->Mercury SMTP Server->Connection control
去掉Do not permit SMTP relaying of non-local mail前的钩。最后一步可能不是必要的。

如果发送邮件时从Mercury SMTP client里看到类似找不到XXX.com的信息的话，可能是DNS没有配置好的关系。找到xampp\MercuryMail\MERCURY.INI，配置相应的DNS。
```
Nameservers : 192.168.1.100,172.16.119.12
```
现在，应该可以收发邮件了。sohu的邮箱有反向域名解析，如果域名不合法是拒收邮件的。QQ邮箱则会收取，但会直接丢到垃圾邮件里去。

另外，也可以使用xampp捆绑的sendmail.exe来发送邮件。这个sendmail.exe是一个win32的命令行程序，其 '-t' 参数通过stdin投递邮件。这个命令行位于xampp/sendmail/sendmail.exe，其配置文件时同目录下的sendmail.ini。在sendmail.ini中：
```
[sendmail]

; you must change mail.mydomain.com to your smtp server,
; or to IIS's "pickup" directory.  (generally C:\Inetpub\mailroot\Pickup)
; emails delivered via IIS's pickup directory cause sendmail to
; run quicker, but you won't get error messages back to the calling
; application.

smtp_server=localhost

; smtp port (normally 25)

smtp_port=25
```
smtp_server和smtp_port分别表示SMTP服务器主机和端口号。
如果SMTP服务器要求认证，或者是认证前POP3，则还要修改下面的配置信息。
```
; if your smtp server requires authentication, modify the following two lines

auth_username= swordair
auth_password= swordair

; if your smtp server uses pop3 before smtp authentication, modify the 
; following three lines

pop3_server= pop.swordair.com
pop3_username= swordair
pop3_password= swordair
```
最后需要要配置PHP来使用sendmail.exe，在php.ini中：
```
; For Unix only.  You may supply arguments as well (default: "sendmail -t -i").
sendmail_path = "C:\xampp\sendmail\sendmail.exe -t"
```
把sendmail_path的值设置为sendmail.exe的路径，并且在调用时带上-t参数。

如果使用sendmail.exe，那么之前一开始配置的[mail function]应该重新注释起来。
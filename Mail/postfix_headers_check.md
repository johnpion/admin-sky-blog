# Postfix: header_checks

**/etc/postfix/main.cf**
```
header_checks = regexp:/etc/postfix/header_checks
```

**/etc/postfix/header_checks**
```
/^Received:(.*?)with ESMTPSA(.*?)/ REPLACE Received: from mail.com (mail.com [1.1.1.1]) by mail.com (Postfix) with ESMTPSA$2
/^User-Agent:/ IGNORE
/^Received: from localhost/ IGNORE
/^Received: from zmail.example.com/ IGNORE
/^Received: from zmail.corp.example.com/ IGNORE
/^X-Virus-Scanned: amavisd-new at zmail.corp.example.com/ IGNORE
```

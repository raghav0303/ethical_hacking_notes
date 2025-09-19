**Missing Encoding**
* why in the missing encoding challenge of juice shop the # was not being encoded while the cat emoji got encoded automatically?
  Ans - The behavior you're seeing in the **Missing Encoding** challenge of OWASP Juice Shop relates to how URLs are encoded and how different characters are treated in URLs — particularly the `#` symbol and characters like emojis.
* The `#` character in a URL has **special meaning** — it is used to indicate the **start of a fragment** identifier. For example:
* https://example.com/page#section2

Here, #section2 is not sent to the server at all — it is used only by the browser to navigate within the page.
* - So, if you enter `https://juice-shop.example/?search=#exploit`, the browser sees the `#` and **stops sending** anything after it to the server.
    
- Because of this, URL encoding libraries often **do not encode `#` by default** when building URLs, unless explicitly told to.
- Emojis and other non-ASCII characters (like `😺`) are **not valid in raw URLs**, so browsers **automatically percent-encode** them using UTF-8.
- 😺 → %F0%9F%98%BA
- This happens because:

- Emojis are **outside the ASCII range**.
    
- Any character outside `A-Z`, `a-z`, `0-9`, and a few others must be encoded in URLs.
- \# must be encoded with %23 manually
- https://medium.com/@aashutos.katare/exploiting-juice-shop-improper-input-validation-8296a9b4faea - in this method we can see the change on web page too
- https://www.w3schools.com/charsets/ref_html_ascii.asp
- https://daniel-schwarzentraub.medium.com/juice-shop-1-star-missing-encoding-b5121dd473cd   --- this one is a better way of solving but doesnt display the response image on webpage

**Repetitive registration**
* the confirm password can be intercepted using burp suite and changed in between

**Web3 Sandbox**
* A "code sandbox" is ==a contained environment where code can be run and tested without affecting the main system==. It's like a "playground" for code, isolating potential issues and allowing for experimentation without risking the stability or integrity of the larger system. It is a virtual machine.
* In the context of Web3, smart contracts are self-executing agreements written as code and deployed on a blockchain. They automate predefined actions when specific conditions are met, facilitating trustless and transparent transactions without the need for intermediaries. Essentially, they are digital contracts that automatically enforce their terms and conditions, much like a vending machine dispensing an item when the correct payment is made. Self Executing Agreements.

**Outdated Allowlist**
* In simple terms, a redirect in a website is ==a way to automatically send users and search engines from one URL to another==. This is useful when a page has moved, a link needs to be updated, or a broken URL needs to be fixed. Redirects ensure that visitors and search engines are directed to the correct, active page, preventing broken links and improving the overall user experience.

**Reflected XSS(difficulty 2)**
* We have to reload the page 'track order'(after editing the url and pressing enter for encoding to be performed for the payload that we entered) for seeing the XSS vulnerability to be present.

**Login Admin (Blind SQL Injection Used)**
* https://portswigger.net/support/using-sql-injection-to-bypass-authentication
* https://learn.microsoft.com/en-us/sql/relational-databases/security/sql-injection?view=sql-server-ver17
* sql injection used in username without password-> ' or 1=1 --
* Entering a **single quote (`'`)** into a form field—like "Name"—and submitting it is a **basic test** to check for **SQL injection vulnerabilities**. Here's how it works: 
What's the Goal?
You're trying to see whether the input is **properly sanitized** or if it's being **directly used in an SQL query**—which could let an attacker manipulate the SQL.
If you enter `'` as your name, the query becomes:

`SELECT * FROM users WHERE name = ''' AND password = '<password_input>';`

This breaks the query's syntax because now there are **unbalanced quotes**. SQL engines will typically throw an error like:

`Syntax error near 'AND'`

---
### What the Application Might Be Doing

Imagine the backend has SQL like this (bad practice):

`SELECT * FROM users WHERE name = '<input>' AND password = '<input>';`

Now, if you enter this in the **Name** field:

`' OR '1'='1`

The query becomes:

`SELECT * FROM users WHERE name = '' OR '1'='1' AND password = '<input>';`

---

### 📌 Why the Quotes?

- The application already **wraps user input in quotes**:
    
    sql
    
    CopyEdit
    
    `name = '<user_input>'`
    
- So, when we input `' OR '1'='1`, we’re **closing the original quote**, inserting our own **SQL logic**, and then starting a **new string**.
    

> Without the quotes, SQL wouldn’t understand that `'1'='1'` is a valid condition—it needs to treat those as string literals.


Password Strength
* https://curiositykillscolby.com/2020/11/15/pwning-owasps-juice-shop-pt-19-password-strength/


**Security Policy**

-> https://securitytxt.org/
this site tells about /.well-known/security.txt
-> https://curiositykillscolby.com/2020/10/29/pwning-owasps-juice-shop-pt-1-security-policy/
-> https://github.com/securitytxt/security-txt

**Weird Crypto**(Important)

-> https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API
-> search tokenDecode in main.js file in source of developer tools.
This function decodes the Base64 encoded token stored in the local storage as JWT(JSON Web Token). The problem is the token is encoded form of the json response of the user meta data which server returns after successful login of that user. And that too the token is stored in the local storage which is very insecure way of storing it. The token should be stored in the form of HTTPCookies. So we can get all the details about that user from the local storage by decoding the token stored in it. Furthermore, the password field in the decoded token's user data contains hash of password. Using online tools we can easily know which hash is used and then find the password using that hash if the hash algorithm is weak which here is indeed weak, it being MD5.

So two answers are Base64 and MD5.

-> https://curiositykillscolby.com/2020/11/11/pwning-owasps-juice-shop-pt-15-meta-geo-stalking-weird-crypto/

**Deprecated Interface**(still not understood)

-> https://curiositykillscolby.com/2020/11/14/pwning-owasps-juice-shop-pt-18-deprecated-interface/

**NFT Takeover**(Important)

-> https://steemit.com/bitcoin/@lovelyloon/retrieve-private-key-from-seed-phrase
for getting the private key using the seed phrase(secret recovery phrase)
-> https://www.youtube.com/watch?v=XjaxI4MELLE

**Logging in as Jim using sql injection**
-> in username jim@juice-sh.op' --
password - anything can be written, it doesnt matter since the sql query after username/email will be commented out
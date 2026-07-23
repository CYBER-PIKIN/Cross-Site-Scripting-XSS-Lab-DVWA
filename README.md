# Cross-Site-Scripting-XSS-Lab-DVWA (Low Security)


## Objective
The purpose of this lab is to understand two common types of Cross-Site Scripting (XSS) attacks:
- Reflected XSS
- Stored XSS

**By the end of this lab, you will understand:**
- What each attack does
- How it works
- Why it happens
- Why Stored XSS is generally more dangerous

### Lab Environment
- DVWA (Damn Vulnerable Web Application)
- Security Level: Low
- Browser: Firefox/Chrome
- Local machine only (Never test these attacks on real websites.)

## What is Cross-Site Scripting (XSS)?

Cross-Site Scripting (XSS) is a web security vulnerability that allows an attacker to inject JavaScript code into a website. Instead of treating the input as plain text, the website mistakenly treats it as code and executes it inside the visitor's browser.

**An attacker could use XSS to:**
1. Steal session cookies
2. Redirect users to fake websites
3. Display fake login pages
4. Capture user input
5. Perform actions on behalf of users

## Reflected XSS
### Step 1 – Open the Reflected XSS Module
1. Open DVWA.
2. Log in:

*username: admin*

*password: password*

3. Set the Security Level to Low.
4. From the left menu, click XSS (Reflected).

You should see a page asking:

*"What is your name?"*

There will be a text box and a Submit button.


(Insert screenshot of the Reflected XSS page.)

### Step 2 – Test for XSS

Instead of entering your name, type:

*<script>alert('XSS')</script>*

Click Submit.

**What happened?**

Instead of displaying the text normally, the browser opens a popup saying:

*XSS*

**This confirms that the website executed your JavaScript instead of displaying it as text.**


(Insert screenshot showing the alert popup.)

**Why did this happen?**

Normally, websites should treat anything typed into a form as plain text.

For example:

*John*

should simply display:

*Hello John*

However, DVWA does not filter user input at the Low security level.

So when JavaScript is entered, the browser executes it.

### Step 3 – Read the Browser Cookie

**Now replace the previous payload with:**

<script>alert(document.cookie)</script>

Click Submit.

A popup appears showing something similar to:

*PHPSESSID=o3tiup20lg7jg47hmtfh2p1nm3; security=low*

(Insert screenshot showing the cookie alert.)

**What is a Cookie?**

A cookie is a small piece of information stored by your browser.

Websites use cookies to:

- Keep users logged in
- Remember preferences
- Track sessions

If an attacker steals a session cookie, they may be able to impersonate the logged-in user.

### Step 4 – Create a Malicious Link

A reflected attack usually arrives through a specially crafted URL.

After submitting the payload, your browser address bar will look something like:

*http://10.113.134.175/vulnerabilities/xss_r/?name=<script>alert(document.cookie)<%2Fscript>#*

Because URLs cannot contain certain special characters, they must be URL encoded.

The encoded version becomes something like:

*http://10.113.134.175/vulnerabilities/xss_r/?name=%3Cscript%3Ealert%28document.cookie%29%3C%2Fscript%3E#*

An attacker could send this link through:

- Email
- WhatsApp
- Social media
- SMS

Once the victim clicks the link, the browser executes the JavaScript.


(Insert screenshot showing the crafted URL.)

**Why is it called Reflected XSS?**

The malicious script is:

1. Sent to the website.
2. Immediately reflected back.
3. Executed in the victim's browser.

Nothing is permanently stored on the server.

The attack only works if someone opens the malicious link.

## Part 2 – Stored XSS
### Step 1 – Open the Stored XSS Module

From the DVWA menu, click:

XSS (Stored)

You will see a Guestbook with:

- Name
- Message

fields.


(Insert screenshot of the Stored XSS page.)

### Step 2 – Inject the Payload

Enter any name.

Example:

*Silas*

For the message, type:

<script>alert('Stored XSS')</script>

Click Sign Guestbook.

### Step 3 – Observe the Result

Immediately after saving, a popup appears saying:

*Stored XSS*


(Insert screenshot showing the popup.)

### Step 4 – Refresh the Page

Press F5 or refresh the browser.

Notice something different.

The popup appears again, even though you did not submit the form again.


(Insert screenshot after refreshing.)

**Why did this happen?**

Unlike Reflected XSS, the payload has been saved in the guestbook database.

Every time someone opens the guestbook:

1. The server loads the saved message.
2. The browser reads it.
3. The browser executes the JavaScript automatically.

Every visitor becomes a victim.

## Reflected XSS vs Stored XSS
|Reflected XSS	| Stored XSS |
|----------------|----------------|
|Script is not saved.	| Script is permanently saved. |
|Victim must click a malicious link.	| No link is required after the payload is stored. |
|Affects one victim at a time.	| Affects every visitor automatically. |
|Temporary attack.	| Persistent attack. |
|Less dangerous.	| More dangerous. |

### Why is Stored XSS More Dangerous?

**Stored XSS is more dangerous because:**

1. The malicious code is stored on the website.
2. Every visitor automatically runs the attack.
3. The attacker does not need to send individual malicious links.
4. The attack continues until the vulnerable content is removed.

One successful injection can impact many users.

## Conclusion

This lab demonstrated two common Cross-Site Scripting attacks using DVWA at the Low security level. The Reflected XSS attack required the victim to click a specially crafted link before the malicious script executed. The Stored XSS attack was more dangerous because the malicious script was saved on the server and automatically executed whenever any user viewed the affected page. This exercise highlights the importance of proper input validation, output encoding, and secure coding practices to prevent XSS vulnerabilities.

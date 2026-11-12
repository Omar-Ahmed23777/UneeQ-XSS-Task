# Web App Security: Reflected Cross-Site Scripting (XSS) Assessment & Remediation

**Author:** Omar Ahmed  
**Task:** Vulnerability Assessment & Patching (Reflected XSS)  
**Platform:** DVWA (Damn Vulnerable Web App) - Security Level: Low  

## 1. Introduction
This report documents the discovery, exploitation, and remediation of a Reflected Cross-Site Scripting (XSS) vulnerability within the DVWA environment. 

## 2. Baseline Validation
To establish the expected application behavior, a standard string input (`Omar`) was submitted. The application successfully reflected the input without any errors.

![Baseline Behavior](Screenshot%202026-11-12%20113536.png)

## 3. Vulnerability Exploitation
To test for XSS, the following JavaScript payload was injected into the input field:
`<script>alert('UneeQ XSS Task by Omar');</script>`

The application executed the script, triggering a browser pop-up. This confirmed the presence of a Reflected XSS vulnerability due to a lack of input sanitization.

![Exploitation Proof](Screenshot%202026-11-12%20113710.png)

## 4. Remediation (Code Fix)
The source code (`low.php`) was modified to prevent the execution of injected scripts. The PHP function `htmlspecialchars()` was implemented to convert special characters (such as `<` and `>`) into HTML entities, effectively neutralizing the payload.

**Vulnerable Code:**
```php
$html .= '<pre>Hello ' . $_GET[ 'name' ] . '</pre>';
```

**Secured Code:**
```php
$html .= '<pre>Hello ' . htmlspecialchars($_GET[ 'name' ], ENT_QUOTES, 'UTF-8') . '</pre>';
```

![Remediation Code](image_7a121c.png)

## 5. Verification
The exact same payload (`<script>alert('UneeQ XSS Task by Omar');</script>`) was submitted again after applying the patch. The application safely rendered the payload as plain text rather than executing it, confirming that the vulnerability has been successfully remediated.

![Verification](image_7a1204.png)

---
*Note: The complete modified source code can be found in the attached `low.php` file in this repository.*

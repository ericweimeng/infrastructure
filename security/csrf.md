# CSRF

[Cross-Site Request Forgery \(CSRF\)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29) is a type of attack that occurs when a malicious web site, email, blog, instant message, or program causes a user's web browser to perform an unwanted action on a trusted site when the user is authenticated. A CSRF attack works because browser requests automatically include any credentials associated with the site, such as the user's session cookie, IP address, etc. Therefore, if the user is authenticated to the site, the site cannot distinguish between the forged or legitimate request sent by the victim. We would need a token/identifier that is not accessible to attacker and would not be sent along \(like cookies\) with forged requests that attacker initiates. For more information on CSRF, see OWASP [Cross-Site Request Forgery \(CSRF\) page](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29).

The impact of a successful CSRF attack is limited to the capabilities exposed by the vulnerable application. For example, this attack could result in a transfer of funds, changing a password, or making a purchase with the user's credentials. In effect, CSRF attacks are used by an attacker to make a target system perform a function via the target's browser, without the user's knowledge, at least until the unauthorized transaction has been committed.

Impacts of successful CSRF exploits vary greatly based on the privileges of each victim. When targeting a normal user, a successful CSRF attack can compromise end-user data and their associated functions. If the targeted end user is an administrator account, a CSRF attack can compromise the entire web application. Using social engineering, an attacker can embed malicious HTML or JavaScript code into an email or website to request a specific 'task URL'. The task then executes with or without the user's knowledge, either directly or by using a Cross-Site Scripting flaw. For example, see [Samy MySpace Worm](https://en.wikipedia.org/wiki/Samy_%28computer_worm%29).

## Login CSRF

Login CSRF is a type of attack where the attacker can force the user to log in to the attacker’s account on a website and thus reveal information about what the user is doing while logged in.

Most developers tend to ignore CSRF vulnerability on login forms as they assume that CSRF would not be applicable on login forms because user is not authenticated at that stage. That assumption is false. CSRF vulnerability can still occur on login forms where the user is not authenticated, but the impact/risk view for it is quite different from the impact/risk view of a general CSRF vulnerability \(when a user is authenticated\).

With a CSRF vulnerability on login form, an attacker can make a victim login as them and learn behavior from their searches. For more information about login CSRF and other risks, see section 3 of [this](https://seclab.stanford.edu/websec/csrf/csrf.pdf) paper.

Login CSRF can be mitigated by creating pre-sessions \(sessions before a user is authenticated\) and including tokens in login form. You can use any of the techniques mentioned above to generate tokens. Pre-sessions can be transitioned to real sessions once the user is authenticated. This technique is described in [Robust Defenses for Cross-Site Request Forgery section 4.1](https://seclab.stanford.edu/websec/csrf/csrf.pdf).

If sub-domains under your master domain are treated as not trusty in your threat model, it is difficult to mitigate login CSRF. A strict subdomain and path level referrer header \(because most login pages are served on HTTPS - no stripping of referrer - and are also linked from home pages\) validation \(detailed in section 6.1\) can be used in these cases for mitigating CSRF on login forms to an extent.

### What can happen

The risk varies depending on the application and is hard to evaluate from a black-box perspective.

PayPal was once vulnerable to login CSRF and the attacker could make a user log in to the attacker’s PayPal account. When the user later on paid for something online, they unknowingly added their credit card to the attacker's account.

Another, less obvious, example is a login CSRF that once existed in Google, which made it possible for the attacker to make the user log in to the attacker’s account and later retrieve all the searches the user had made while logged in.

If public registration for the application exists, the risk of attacks drastically increases as it’s very easy for the attacker to create an account and thus know the credentials for it.

### Example of Login CSRF

Login CSRF is like any other CSRF, the only difference is that it occurs on the login form. If you need more information on this topic, you can just search for CSRF in general.

An example of this vulnerability in PHP could look something like this:

```php
<?php
    if (isset($_POST["user"], $_POST["pass"])){
        // code for checking the user and password
    } else {
        echo '
            <form method="post">
                <input name="user">
                <input name="pass" type="password">
                <input type="submit">
            </form>
        ';
    }
?>
```

As the PHP code doesn’t validate where the credentials come from, it would be possible to trick the user to visit a page with following code:

```markup
<form id="LoginForm" action="http://target/login.php" method="post">
    <input name="user" value="foo">
    <input name="pass" type="password" value="bar">
    <input type="submit">
</form>

<script>
    document.getElementById("LoginForm").submit();
</script>
```

The browser would send a POST request with the login credentials to the PHP page which checks if they are correct and then log in the user.

### Remediation

You need to implement a token system in your code to prevent Login CSRF - see [the OWASP CSRF Prevention Cheat Sheet](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29_Prevention_Cheat_Sheet) for different recommended methods. The important thing is to make sure the token is something the user has \(but not the attacker\), so that you can make sure it really is the user submitting a login request.

### **The easiest remediation for CSRF**

The following snippet of code shows what is probably the easiest of the recommended methods, [Double submit cookies](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29_Prevention_Cheat_Sheet#Double_Submit_Cookies).

```php
<?php
    if (isset($_POST["user"], $_POST["pass"]){
        // Make sure the token from the login form is the same as in the cookie
        if (isset($_POST["CSRFtoken"], $_COOKIE["CSRFtoken"])){
            if ($_POST["CSRFtoken"] == $_COOKIE["CSRFtoken"]){
                // code for checking the user and password
            }
        }
    } else {
        $token = bin2hex(openssl_random_pseudo_bytes(16));
        setcookie("CSRFtoken", $token, time() + 60 * 60 * 24);
        echo '
            <form method="post">
                <input name="user">
                <input name="pass" type="password">
                <input name="CSRFtoken" type="hidden" value="' . $token . '">
                <input type="submit">
            </form>
        '
    }
?>
```

The code will generate a random token and set it both as a cookie and as a hidden-value in the form. When the form is submitted, the code will check if the CSRFToken in the form is the same as the CSRFToken in the cookie and the user will be logged in only if they are the same.

This works as a protection because the attacker can’t know the value of the cookie CSRFToken and therefore can’t send that value in the form. This method is also valid for other kinds of CSRF vulnerabilities, not just login CSRF.

### Samesite Cookie Attribute <a id="samesite-cookie-attribute"></a>



### **Reference**

* [https://support.detectify.com/customer/portal/articles/1969819-login-csrf](https://support.detectify.com/customer/portal/articles/1969819-login-csrf)
* [https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site\_Request\_Forgery\_Prevention\_Cheat\_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)


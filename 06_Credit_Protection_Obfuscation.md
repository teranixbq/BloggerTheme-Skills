# Credit Link Protection (Obfuscation)

This document explains how to secure copyright (credit links) in a Blogger template using JavaScript **obfuscation** techniques.

## Securing the Credit Link

Since Blogger templates run on the client-side (in the browser), we cannot perform true "encryption" (the browser still needs to read and execute the code). However, we can use **Obfuscation** (scrambling) techniques to protect the JavaScript logic.

### Concept of the Protection System:
1. Place an HTML element in the footer with a specific ID, for example: `<a id="my-credit" href="https://your-domain.com">Created by Developer</a>`.
2. Create a JavaScript snippet that periodically checks the existence of the `#my-credit` element and validates its URL.
3. If the element is removed or its `href` attribute is modified by the template user, the script executes a penalty (e.g., forcibly redirecting the visitor back to the creator's website or wiping the `<body>` content).
4. This JavaScript is then obfuscated/packed so that average users cannot easily read or delete the logic.

### A. Original Script (Decrypted - To be kept by you)
This is the **UN-OBFUSCATED** source code. Keep this safe. If you ever want to create a premium, credit-free version for a client, you just need to remove the obfuscated script from your template. Make sure to keep this clean version for documentation purposes.

```javascript
// --- ORIGINAL SCRIPT (DECRYPTED) ---
// Keep this safe. Do not put this clean version inside the template if you want to protect it.
(function() {
    // Function to verify the credit link
    function checkCredit() {
        var creditElement = document.getElementById("my-credit");
        var creatorLink = "https://your-domain.com"; // Replace with your actual domain

        // If the element is missing, hidden, or the href has been altered
        if (!creditElement || creditElement.getAttribute("href") !== creatorLink || creditElement.style.display === "none" || creditElement.style.visibility === "hidden") {
            // Penalty: Redirect the page to the creator's website
            window.location.href = creatorLink; 
        }
    }

    // Execute when the page loads
    window.onload = function() {
        checkCredit();
    };

    // Execute every 3 seconds to prevent manipulation via browser developer tools (Inspect Element)
    setInterval(checkCredit, 3000);
})();
```

### B. Obfuscated Script (Encrypted - To be injected into the template)
To make it difficult for standard users to locate the redirect logic, use a packer (like base64 evaluation or a hexadecimal array).

Here is an example of how you should write the obfuscated script into the Blogger XML. Make sure it is always wrapped in a `CDATA` block!

```xml
<script type='text/javascript'>
//<![CDATA[
/* DO NOT REMOVE THIS SCRIPT */
eval(function(p,a,c,k,e,d){e=function(c){return c.toString(36)};if(!''.replace(/^/,String)){while(c--){d[c.toString(a)]=k[c]||c.toString(a)}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('!(3(){3 e(){4 c=0.b("a");4 8="9://7-6.5";2(!c||c.1("f")!==8||c.d.e==="g"||c.d.h==="i"){0.j.k=8}}0.l=3(){e()};m(e,n)})();',24,24,'document|getAttribute|if|function|var|com|domain|your|creatorLink|https|my|getElementById|creditElement|style|display|href|none|visibility|hidden|location|href|onload|setInterval|3000'.split('|'),0,{}))
//]]>
</script>
```

### Implementation Instructions for Credit Protection:
When implementing the template with protection, ensure the following:

- Create a credit element in the footer. 
- Create a JavaScript credit-protection script that prevents the removal of the credit link. 
- Write this protection script directly into the XML template, but it MUST be **obfuscated** using an eval packer or hex encoding technique. 
- However, keep the original, un-obfuscated JS code separately so that you can understand it and have the ability to remove it later if you need to generate a premium version.

With this approach, the generated template will be protected right out of the box, while you retain the "master key" (the un-obfuscated version) if you ever need to perform further customizations.

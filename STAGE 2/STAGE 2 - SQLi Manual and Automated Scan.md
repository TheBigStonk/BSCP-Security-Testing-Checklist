- [ ] Use the SQLi injection fuzzer string `'"--{};--//` in inputs you suspect to check, don't discount using burp scanner as it might find things you miss. ![[Technical/Web/Methodology/BSCP Security Testing Checklist/Images/Pasted image 20250928133129.png]]
- [ ] Find a field that searches for records and **Scan at Selected Insertion Point** (similar to [[Technical/Web/Methodology/BSCP Security Testing Checklist/STAGE 1/STAGE 1 - Scan and Manually Explore|STAGE 1 - Scan and Manually Explore]])
- [ ] If you are confident with an input, test it with SQLMap by right clicking a request with a POST body or URL parameter, selecting the save value, and then running the command line SQLMap program `sqlmap -r saved_request_file_name --dump --batch`

## SQLMap Examples
**SQL Injection in `organize_by` Field - Practice Exam 1**
- Started by using our fuzzer string to identify that the organize_by field within the advance search is vulnerable to SQL injection potentially.
- Started by trying the SQLiPy Extension in Burp which didn't find anything and ran a simple request as above with `-p organize_by` to test that value.
- Once discovered there was a time based sleep enumeration, we filtered the query down to `sqlmap -r /mnt/c/Users/maxof/request_to_test -p organize_by -D public -T users -C password -U administrator --batch --dump` to make this work. ![[Technical/Web/Methodology/BSCP Security Testing Checklist/Images/Pasted image 20250928133335.png]]![[Technical/Web/Methodology/BSCP Security Testing Checklist/Images/Pasted image 20250928133232.png]]
  ![[Technical/Web/Methodology/BSCP Security Testing Checklist/Images/Pasted image 20250928134213.png]]
- 
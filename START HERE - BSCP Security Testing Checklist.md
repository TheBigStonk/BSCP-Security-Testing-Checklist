> Although resources like `botesjuan BSCP Notes` and the Portswigger Academy itself exists, I found there was no clear methodology laid out on the net. This guide if used correctly will nigh guarantee a pass if it is followed to the letter. 

### The Exam Format
You have four hours to complete the BSCP exam. There are two applications, so two hours per app averaged. Each application are completed in three stages:
* **Foothold**: Access any user account.
* **Privilege Escalation**: Use your user account to access the admin interface at /admin, perhaps by elevating your privileges or compromising the administrator account.
* **Data Exfiltraion**: Use the admin interface to read the contents of /home/carlos/secret from the server's filesystem, and submit it using "submit solution".
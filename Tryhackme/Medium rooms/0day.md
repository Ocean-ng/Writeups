first as always I will scan port then directory, find admin, js, uploads and have nothing

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/8af4ea04-3a09-4051-bf82-068c49028038" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/bf2cd6b6-fcf3-4f67-b71b-db67eef1081b" />

go to the backup that we will find the rsa key but we dont know the username

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/65c10c48-39c0-4b4f-8a5a-9983839765f5" />

then I download turtle.png to find if there is steg but still nothing

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/ef12954a-6192-4630-b3f2-4d0d78e3bc70" />

the web we will focus on is cgi-bin, they use cgi-bin as directory to store script file with +x permission

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/55e180a6-3054-44f0-a22d-c0beaaa71297" />

use nuclei to find more vulnerabilities, find the CVE-2014-6271

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/dcb29134-711a-40f1-b76c-47ca6572930c" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/00ce94d3-8499-4f99-87b4-6191cf2cc659" />

with this information I use metaploit, set lhost, rhost, targeturi then exploit 

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/1396ac79-c2bd-485b-ab32-5a238e3fd35b" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/abf4750d-31c4-4c3e-a05d-e733c7f32d03" />

now we find the user flag

check the version of ubuntu, its Ubuntu 14.04

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/c17ce008-2947-4dd8-bc34-8b865051c636" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/9124f0fe-020a-4e1a-a9e3-8bac3558cb07" />

then use searchsploit to find payload

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/e6d35941-a308-4ab5-b1ae-f3f5715d582c" />

run dos2unix 37292 that will make file from windows to linux environment. Open web port to download this file with target shell (change dir to tmp)

<img width="1577" height="679" alt="image" src="https://github.com/user-attachments/assets/50d8e771-2758-420c-b12f-0e04a7df3e64" />

bang bang now we find the root flag


















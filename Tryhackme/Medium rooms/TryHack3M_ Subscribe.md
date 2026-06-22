port 8000 and 8089 is splunk

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/d4185cad-13f4-4ed0-a4d4-7b8bca68680f" />

use dirsearch that we will find js directory here

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/77d41d94-9779-4c26-bc5a-46984975a715" />

in the invite.js, the function only executes when the hostname matches capture3millionsubscribers.thm. Once on the correct domain, it sends a POST request to inviteCode1337HM.php to retrieve an invite code

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/f9acde66-3061-4f8d-aa33-961d32f03b06" />

so you need to add domain: echo "10.49.186.14 capture3millionsubscribers.thm" >> /etc/hosts
then go to domain u added before, press f12 to go to console, type script then get invite code

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/905bfd30-09c3-4342-a9f9-325bfd90c0a8" />

we get the email and password to login

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/a8f939b0-f132-4bef-8098-1b7fd50289b8" />

our purpose now is how to get into admin panel? press f12 again and check storage, there is a isVip that value set "false", lets change to "true" value

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/b7bf5730-e40e-4442-8e0e-c641d60f2d51" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/304ade49-1575-4429-bbce-208fbb292246" />


we can start the machine if we r vip users, check the source. This is a pure client-side check. The server is never consulted the code simply reads the value of a DOM element. Run the script then click start machine to bypass the restriction

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/919f5c83-814c-4015-bcf0-b4e98166601e" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/49496e70-e065-4965-8ced-96b461dba7c3" />

go to console page and check source code. We will see its just a few command to do, note that cat "accessed jointly by dev and prod team" meaning the server has filter and the page when white empty when u r not special permission to read or cannot bypass the filter
remember the config.php?, its white, now we will check if it has something 

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/06641144-64f7-485e-95a6-dcd0b2ff7ec8" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/d48b7f70-16c0-4d3a-84c1-c7b3d75d0674" />

alright there we r, add domain to ur host file. Add SECURE_TOKEN in storage but the page still forbiden, let see we can search directory

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/e225ff0c-5bef-43ee-9869-8bd44074c0b9" />

its time to us using token. Try some sql injection but theres nothing work so we will use sqlmap, copy ur info from burpsuite then store it on txt file

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/612bfd70-0e50-4912-8b46-3d1124c70d49" />

type the command: sqlmap -r <urfile.txt> --dump --batch
--dump: Dump all discovered data from the database
--batch: Auto-answer all prompts without interaction
thats how we find admin's username and password
Change the option from invite code to sign up, comeback main page (not domain, type ur ip) and bum bum flag appears

<img width="1381" height="610" alt="image" src="https://github.com/user-attachments/assets/9a8135e6-2bb5-4062-b7d5-435647225cb1" />


In the splunk, press "search your data", set All time and type "index *" in search bar. Click user_agent let us know what tool did hacker use to attack. We find the hacker ip, add it to search bar. Press uri, now we know which databases hacker use

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/4157fd34-a596-4f9d-a09d-ee1a61f0f5d8" />















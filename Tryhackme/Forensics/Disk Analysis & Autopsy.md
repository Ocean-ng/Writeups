1. What is the MD5 hash of the E01 image?

data sources -> hasan2.e01 -> file metadata -> md5

<img width="2190" height="1114" alt="Screenshot 2026-05-18 124839" src="https://github.com/user-attachments/assets/7cce4a95-dc67-4fd1-883f-089202b211c5" />


2. What is the computer account name?

operating system information -> system -> result -> name 

<img width="2186" height="1110" alt="Screenshot 2026-05-18 124912" src="https://github.com/user-attachments/assets/de817add-7446-4a11-bac9-0ad318d7a50d" />


3. List all the user accounts. (alphabetical order)

operating system user account -> username 

<img width="2185" height="1106" alt="Screenshot 2026-05-18 125102" src="https://github.com/user-attachments/assets/6cf006b5-d393-43cf-889c-69905c8eb470" />


4. Who was the last user to log into the computer?

operating system user account -> data accessed

<img width="2192" height="1109" alt="Screenshot 2026-05-18 125130" src="https://github.com/user-attachments/assets/eb484a0c-2933-414b-aa96-08ea7723ee77" />


5. What was the IP address of the computer?

data sources -> hasan2.e01 -> vol3 -> program files -> program files (x86) -> look@lan -> irunin.ini -> %LANIP%

<img width="2184" height="1104" alt="image" src="https://github.com/user-attachments/assets/67158d88-8491-4e08-9f66-b03c7bf72b79" />


6. What was the MAC address of the computer? (XX-XX-XX-XX-XX-XX)

data sources -> hasan2.e01 -> vol3 -> program files -> program files (x86) -> look@lan -> irunin.ini -> %LANNIC%

<img width="2184" height="1104" alt="image" src="https://github.com/user-attachments/assets/e16333c3-920f-413c-8544-c18f545d7970" />


7. What is the name of the network card on this computer?

vol3 -> windows -> programs -> amcache.hve

<img width="2151" height="729" alt="image" src="https://github.com/user-attachments/assets/8072a124-dae1-4bae-9208-283e8b078d1b" />



8. What is the name of the network monitoring tool?

results -> extracted content -> installed programs -> program name -> look@lan 2.50 build 35

<img width="2185" height="1107" alt="image" src="https://github.com/user-attachments/assets/c200b20e-acb6-4c04-a9db-2610e852ad68" />


9. A user bookmarked a Google Maps location. What are the coordinates of the location?

results -> extracted content -> web bookmarks -> title -> 12°52'23.0"N 80°13'25.0"E 
<img width="2183" height="1105" alt="image" src="https://github.com/user-attachments/assets/967a4b38-92b6-4ade-9df8-193e75cf42b6" />


10. A user has his full name printed on his desktop wallpaper. What is the user's full name?

web downloads -> url -> images/videos -> user download -> show content viewer (idk what happened with my vm)

<img width="2297" height="1280" alt="image" src="https://github.com/user-attachments/assets/84b2f929-09c4-4de0-9255-48295c3e3f78" />


11. A user had a file on her desktop. It had a flag but she changed the flag using PowerShell. What was the first flag?

vol3 -> users -> shreya -> roaming -> microsoft -> windows -> powershell -> psreadline -> consolehost_history.txt

<img width="2186" height="1104" alt="image" src="https://github.com/user-attachments/assets/490f5888-8663-4fb7-820b-6e6d8e9a7751" />


12. The same user found an exploit to escalate privileges on the computer. What was the message to the device owner?

vol3 -> users -> shreya -> desktop -> exploit.ps1 

<img width="2185" height="1108" alt="image" src="https://github.com/user-attachments/assets/e4491658-a282-49d4-add5-a2b3180fb9c8" />


13. 2 hack tools focused on passwords were found in the system. What are the names of these tools? (alphabetical order)

program data > microsoft > windows defender > scans > history > service > detectionhistory -> 02

<img width="2181" height="1106" alt="image" src="https://github.com/user-attachments/assets/ce30158a-e4fe-40b4-89b9-9dabb5c645d0" />

<img width="2180" height="1104" alt="image" src="https://github.com/user-attachments/assets/f16762e5-1654-4c15-ab80-e560226002dd" />


14. There is a YARA file on the computer. Inspect the file. What is the name of the author?

search .yar 

<img width="2182" height="1106" alt="Screenshot 2026-05-18 143311" src="https://github.com/user-attachments/assets/f89fed64-d500-494b-b386-067351953a45" />

search kiwi_passwords.yar

<img width="2184" height="1101" alt="image" src="https://github.com/user-attachments/assets/c6550b30-bdee-4ca1-8926-05453a7a06e2" />


15. One of the users wanted to exploit a domain controller with an MS-NRPC based exploit. What is the filename of the archive that you found? (include the spaces in your answer)

search zerologon

<img width="2181" height="1105" alt="image" src="https://github.com/user-attachments/assets/9e595783-4a73-4d23-be09-f01d84379c60" />


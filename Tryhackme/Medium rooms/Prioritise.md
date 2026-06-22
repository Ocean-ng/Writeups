First, I went to scan the gates and found 2 ports: 22 and 80.
<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/41bf1ac7-5827-48b4-bb90-5f1e23aa5e39" />

here is a to-do list, which is usually sorted by day, month, year, or by name, title, or level of importance.

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/21bdd91e-3fa5-4773-93c4-9c5c786584a9" />

So I added the item, and according to Burpsuite, it shows two variables, title and date. Furthermore, there's an order variable here, which further confirms that this is an order by SQLi

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/3cb68ad2-6319-490c-87d0-a08b3b695ce2" />

Now I'll try ordering with the parameters title and date, and as you can see in the image, the order is reversed.

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/2e4978f6-f258-4964-bf40-17bc27485649" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/d0d79e88-2f36-4cb5-b9de-f57e9797b06a" />

Lets use SQLmap and find flag

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/c9aedbc2-7890-4e96-9f9e-53602387a736" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/5c5087e5-1e9a-4fe0-b4aa-4a5736fb216d" />






# Lab-Week-6---Lian_Yu

## 1. `nmap -sC -sV -Pn 10.49.175.3`
<img width="735" height="486" alt="image" src="https://github.com/user-attachments/assets/82ec8de3-f6ad-4a71-8507-1b8fdaa679d7" />

- Open port 80

## 2. Landing page of `10.49.175.3`
<img width="626" height="360" alt="image" src="https://github.com/user-attachments/assets/04cdca5b-d27d-4d0d-8acb-521fca09f1fe" />

## 3. Directory search with `gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u http://10.49.175.3/`
<img width="817" height="434" alt="image" src="https://github.com/user-attachments/assets/068c41e0-967d-457a-8364-cc5f18176415" />

- Found **/island**

## 4. Landing page of /island
<img width="913" height="828" alt="image" src="https://github.com/user-attachments/assets/7f189ae8-ef33-4d3a-b6dc-e8068b5d9af0" />

- Had a hidden code word **"vigilante"** which can only be seen by highlighting the whole page.

## 5. `gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u http://10.49.175.3/island/`
<img width="817" height="417" alt="image" src="https://github.com/user-attachments/assets/0e391848-c627-475b-84ff-00b504450f8d" />

- Found **/2100**

## 6. Landing page of /island/2100
<img width="913" height="831" alt="image" src="https://github.com/user-attachments/assets/5aff3122-3346-4d41-8e1c-7eea4a8243d4" />

- It had a Youtube Video but it is unavailable

## 7. Inspect /2100
<img width="921" height="829" alt="image" src="https://github.com/user-attachments/assets/c44db11b-4f46-4007-99f8-97eeaea033a2" />

- Had the comment "you can avail your **.ticket** here but how?"
- **.ticket** might be a file

## 8. Try running `gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u http://10.49.175.3/island/2100 -x .ticket` to find files that end with .ticket
<img width="817" height="413" alt="image" src="https://github.com/user-attachments/assets/6839b5a2-367c-41f7-9acb-2e76dfc03f6b" />

- Found **green_arrow.ticket**

## 9. green_arrow.ticket
<img width="914" height="831" alt="image" src="https://github.com/user-attachments/assets/f6e1c265-2de4-41e4-99a1-abf862173fcc" />

- Had the token **"RTy8yhBQdscX"**
- Might need to be decoded

## 10. Decode using CyberChef
<img width="912" height="828" alt="image" src="https://github.com/user-attachments/assets/bcc0faad-da93-474c-8e0c-a96788c7de0a" />

- Thought it was Base64 but instead it was **Base58**
- Output is **!#th3h00d**
- Might be a password for something

## 11. Try logging into ftp with `ftp 10.49.175.3`
<img width="459" height="168" alt="image" src="https://github.com/user-attachments/assets/f9eeb669-2b1c-4409-b3b0-6f3c7dacd3ab" />

- Username was **"vigilante"**
- Password was **"!#th3h00d"**
- Login successful

## 12. Run `ls` to list files
<img width="730" height="125" alt="image" src="https://github.com/user-attachments/assets/062fdabf-a845-4f87-9d9d-63d1145d926b" />

- Found 3 image files
- Run `get` for each file to download to local machine
- 2 files could be opened but **"Leave_me_alone.png"** couldn't because of file format error.

## 13. Run `hexedit` to look at the png's hex
<img width="813" height="590" alt="image" src="https://github.com/user-attachments/assets/a7efe248-ff77-4cfc-ad01-5ce243dccee5" />

- File header is incorrect, it says **"XEo"** when it should be **".PNG"**

## 14. Fixed by changing the header to "89 50 4e 47 0d 0a 1a 0a"
<img width="813" height="592" alt="image" src="https://github.com/user-attachments/assets/1f518ead-a627-4f17-afb4-0542b246afab" />

- Now says **.PNG**

## 15. **"Leave_me_alone.png"** now shows a preview  
<img width="798" height="574" alt="image" src="https://github.com/user-attachments/assets/7e5cf207-61f2-4942-a542-56aee4064bf6" />


## 16. Contents of "Leave_me_alone.png" 
<img width="541" height="474" alt="image" src="https://github.com/user-attachments/assets/18dc257f-c609-452e-8168-7f6ce36f34d6" />

- **"password"** is in red text
- Could be a password for something

## 17. Run `steghide extract -sf aa.jpg` to extract data from aa.jpg
<img width="517" height="71" alt="image" src="https://github.com/user-attachments/assets/71da1ccc-252e-4621-98b7-f7bf761f864d" />

- Enter the password from before ("password")
- Extracted **ss.zip**

## 18. Unzip ss.zip
<img width="732" height="485" alt="image" src="https://github.com/user-attachments/assets/126de574-b4ff-4b76-83fa-b8a79273be1d" />

- Contained 2 files: **passwd.txt** and **shado**

## 19. `cat passwd.txt`
<img width="730" height="226" alt="image" src="https://github.com/user-attachments/assets/d94c4c86-5ef5-4395-9dbe-e1aed3eee79e" />

- The info in **"passwd.txt"** might not be relevant

## 20. `cat shado`
<img width="360" height="54" alt="image" src="https://github.com/user-attachments/assets/cf88a101-146a-402f-91ae-5b2d0407cd76" />

- Contained the string **"M3tahuman"**
- Might be a password for ssh

## 21. Go back to `ftp` to recheck the whole directory
<img width="732" height="520" alt="image" src="https://github.com/user-attachments/assets/eaed1f7b-7112-4000-ac5c-0a6359321009" />

- Run `cd ..`
- Run `dir`
- Saw that there was another user **"Slade"**

## 22. Run `ssh slade@10.49.175.3`
<img width="874" height="484" alt="image" src="https://github.com/user-attachments/assets/c17306f1-9f15-430c-9089-945b939a2e9f" />

- Password was **"M3tahuman"**
- Welcome to Lian_Yu

## 23. List all files with `ls`
<img width="393" height="105" alt="image" src="https://github.com/user-attachments/assets/db4955f7-61f4-4942-8888-d54118f2f0db" />

- Found **user.txt**
- Run `cat user.txt`
- It containted the flag **"THM{P30P7E_K33P_53CRET5__C0MPUT3R5_D0N'T}"**

## 24. Run `sudo -l` to list the user's specific sudo privileges
<img width="779" height="164" alt="image" src="https://github.com/user-attachments/assets/f871a5fe-3307-47a3-a54f-07b09455fe26" />

- User Slade can run the command : `/usr/bin/pkexec`
- pkexec allows users to execute commands with the privileges of another user

## 25. sudo pkexec /bin/bash
<img width="962" height="392" alt="image" src="https://github.com/user-attachments/assets/35d179dd-b6ce-4244-8132-fde95b90a3fd" />

- Run `ls`
- Found **root.txt**
- Run `cat root.txt`
- Got the final flag: **"THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}"**

## 26. Completed the Room
<img width="1238" height="750" alt="image" src="https://github.com/user-attachments/assets/1142429c-e758-41ea-8475-fa0e825b8646" />


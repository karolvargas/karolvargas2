---
title: Honey Encryption Python implementation
date: "2019-05-04"
featuredImage: './featured.jpg'
---

Honey Encryption is the future of encryption. Now you may ask what is encryption? It is
the process of converting information or data into a code, especially to prevent unauthorized access.
Now the key word that we want to focus on here is unauthorized access.
SOO. We are also aware that hacking and cybersecurity exist for a reason. Although there is
encrypted data there is almost always a way to decrypt that data. With the processing power
of computers increasing by the day we can see how more and more iterations to enter incorrect
passwords and eventually 'crack' the password or key.
#Now
What does Honey Encryption do that's different? And is it as delicious as Honey? Well for the latter
i'm sorry to inform you no. But it can help capture hackers and identity theft. **So how does it work Karol!?**
Okay my bad.\n
What it does is basically when a certain number of iterations has been hit on incorrect password attempts. The system
is notified of this. At this number of counts the system will then rather than output the correct information, will output similar but completely wrong data. That is why it is called Honey. The picture above probably explains it better.
\n

##So Can you show me Anything with this or are you informing me...

I have shown a very simple implementation of using the cryptography library in Python to create a program which will lock certain file restrictions and creating a key from a password you will be able to encrypt any file you wish. The way the cryptography encrypts data is through binary format. This allows us to encrypt images, csv files, txt, pdf, you name it. The code below demonstrates encrypting a file named test.txt. **Yes it's called test im not an expert sorry**:

Runs with Python 2.7^:

```
import os, sys, stat
import fcntl
import time
import Crypto
import base64
import os
from cryptography.fernet import Fernet
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC
#Must import all above, no exceptions


#locks file permission
"""os.chmod("test.txt", stat.S_IXGRP)"""

#unlocks file permission
"""os.chmod("test.txt", stat.S_IRWXU)"""


"""Global Variables"""
filename = "test.txt"



#Function that stores count and allows us to manipulate how many times someone can be incorrect before getting Honeyed
#Added function because Jesse thought this was a hole in the program
def storeCount():
    os.chmod("count.txt", stat.S_IRWXU)
    with open("count.txt",'r+') as f:
        value = int(f.read())
        f.seek(0)
        f.write(str(value + 1))
    os.chmod("count.txt", stat.S_IXGRP)

#Resets count after correct password entered
def resetCount():
    os.chmod("count.txt", stat.S_IRWXU)
    with open("count.txt",'r+') as f:
        value = int(f.read())
        f.seek(0)
        f.write(str(value - value))
    os.chmod("count.txt", stat.S_IXGRP)

#checks count to know if its time to break out the honey
def checkCount():
    os.chmod("count.txt", stat.S_IRWXU)
    with open("count.txt", 'r+') as f:
        values = int(f.read())
    value = values
    print(value)
    os.chmod("count.txt", stat.S_IXGRP)

    return value

#This is where it gets a little complicated, Ill be as in depth as I can
def keyGen():

    passss = raw_input("please enter password: ")
    passs = passss.encode()
    salt = b'\x97g(\x02\x9f\xe2\xd80!b\xd9Oo\xb7@\x9d'
    kdf = PBKDF2HMAC(
     algorithm=hashes.SHA256(),
     length=32,
     salt=salt,
     iterations=100000,
     backend=default_backend()
     )

    key = base64.urlsafe_b64encode(kdf.derive(passs))
    #print(key)
    file = open("key.key", "wb")
    file.write(key)
    file.close()

    file1 = open("private.txt", "w+")
    file1.write(passs)
    file1.close()

    encrypti(filename)


def encrypti(filename):
    #print("Please place file you wish to encrypt into input directory")
    #file_name = "../input/%s" % (file_name)
    file = open("key.key", "rb")
    key = file.read()
    file.close()

    cipher_suite = Fernet(key)

    with open("test.txt") as f:
        with open("test.encrypted", "wb") as f2:
            for line in f:
                f2.write(cipher_suite.encrypt(line))
            f2.close()
        f.close()

    with open("test.txt", "w+") as f:
        for line in f:
            f.write(cipher_suite.encrypt(line))
        f.close()
    lockfile(filename)
    os.chmod("private.txt", stat.S_IXGRP)


#    if len(stored) != 0:
#        parser(file_name)
#        lockfile(file_name)
#    else:
#        print("ERROR 404: THAT PASSWORD SUCKS")
#    return stored

def decrypt(filename):
    inputfile = "test.encrypted"
    honeyfile = "/input/honey.txt"
    outputfile = "test.txt"

    print("time to decrypt!")
    new = raw_input(b"Enter password: ")

    file = open("key.key", "rb")
    key1 = file.read()
    file.close()
    #print(key1)

    os.chmod("private.txt", stat.S_IRWXU)
    file1 = open("private.txt", "r+")
    oldpas = file1.read()
    file1.close()

    cipher = Fernet(key1)

    count = checkCount()

    if new == oldpas and count != 10:
        unlockfile(filename)
        with open(inputfile, 'rb') as f:
            data = f.read()

        newdata = cipher.decrypt(data)
        os.remove(inputfile)

        with open(outputfile, 'wb') as f:
            f.write(newdata)
        resetCount()
        os.remove("private.txt")
        os.remove("key.key")

    elif new != oldpas and count != 3:
        print("That is not the password")
        storeCount()

    elif new == oldpas and count == 3:
        unlockfile(filename)
        os.chmod("private.txt", stat.S_IRWXU)
        with open(honeyfile) as f:
            with open("test.txt", "w+") as f2:
                for line in f:
                    f2.write(line)
                f2.close()
            f.close()

        os.remove(inputfile)
        os.remove("private.txt")




#cipher_text = ciphdder_suite.encrypt(b"A really secret message. Not for prying eyes.")
#plain_text = cipher_suite.decrypt(cipher_text)

def lockfile(filename):
    os.chmod("test.txt", stat.S_IXGRP)

def unlockfile(filename):
    os.chmod("test.txt", stat.S_IRWXU)



def main(filename):
    """Follow Hashtag instructions before correctly running program"""
    os.chmod("count.txt", stat.S_IRWXU)#comment out unless pushing
    unlockfile(filename)#comment out unless pushing
    #os.chmod("count.txt", stat.S_IXGRP) #UNCOMMENT to correctly store count
    print("Please enter whether you want to encrypt or decrypt?")
    inp = raw_input("")

    if inp == 'encrypt':
        keyGen()
    elif inp == 'decrypt':
        decrypt(filename)

if __name__ == "__main__":
    main("test.txt")

```

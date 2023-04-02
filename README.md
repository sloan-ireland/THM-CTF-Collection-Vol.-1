# TryHackMe CFT Collection Vol.1 Walkthrough

>*In this walkthrough I will go through the steps needed to complete the challenges CFT Collection Vol.1 Walkthrough. It’s an easy room, all the theory you’ll need is laid out very thoroughly by the creators, but in case you do get stuck, let’s go through the steps together.**


The room consists of a couple of tasks, touching on a different skill in some way so  you will have to find a flag that is hidden in some way. The flag format is THM{some_text_here}. Let’s start with the first task: cryptography.

## Task 2: What does the `base` say?
![Question 1](https://raw.githubusercontent.com/sloan-ireland/Images/main/27.03.2023_12.23.12_REC.png "Da Question")

The flag for this task is in some form of `base` which according to the hint is `base64`. We can also assume the form is proabably `base32` or `base64` because of the = at the end which these base forms commonly have due to the base form of `}` having an equal sign. This one of the more prevalent binary-to-text encoding schemes but there are many forms of ```base``` that can be used. 

If another `base` was used for encoding, you can determine the `base` by examining the characters used in the encoded string. Each `base` has its own set of characters that are used for encoding, so you can look for those specific characters to determine which `base` was used. Here are some examples of different bases and their associated characters:

 * ```base 16``` (hexadecimal): Uses the characters 0-9 and A-F to represent values from 0 to 15.
 * `base 32`: Uses the characters A-Z and 2-7 to represent values from 0 to 31.
* `base 58`: Uses a subset of the characters A-Z, a-z, and 1-9 to represent values from 0 to 57.
* `base 62`: Uses the characters A-Z, a-z, and 0-9 to represent values from 0 to 61.
* `base 85`: Uses a set of 85 ASCII characters to represent values from 0 to 84.

To actually get the plain text from base form for this question you can use the terminal command:
```
echo -n <base_text> | base64 -d
```
This command can be applied to some other base forms. Just switch out the   `base64` to `base<some_number>`. You may have to install this using `sudo` if it doesn't exist on your machine. 

If you can't figure out what base encoding was used or your computer doesn't have that base decode software installed, you can use a website such as [Cyberchef](https://cyberchef.org/).

## Task 3: Meta meta
![Q2](https://raw.githubusercontent.com/sloan-ireland/Images/main/29.03.2023_12.22.57_REC.png)
Given the name of this task we can assume we need to look at the metadata of the file. The [Findme.jpg](https://raw.githubusercontent.com/sloan-ireland/Images/main/Findme.jpg) file doesn't reveal anything at first glance so off to the terminal we go. We can use an EXIF tool to look at the metadeta using the following command:
```
exiftool Findme.jpg
```
You might have to install this tool via a `sudo` command. Alternatively, one can use an online tool such as [exifinfo](https://exifinfo.org/). 

Upon looking at the metadata we can clearly see the flag listed as the owner's name as seen below:\
<img src="https://raw.githubusercontent.com/sloan-ireland/Images/main/Screenshot%202023-03-29%20123508.png" alt="QR Code" width="`00%" height="75%"/>

Because the flag is in ASCII characters you can also use the command from task seven. Just make sure to switch out the file name correctly. 

## Task 4: Mon, are we going to be okay?
![Question 4](https://raw.githubusercontent.com/sloan-ireland/Images/main/29.03.2023_13.13.28_REC.png)
Yet another image for us to work with. There are mutliple ways to hide data within an image whether it be in the photo info (like the header), metadata, the pixels, or the bits that encode the color. Here is the image is hidden via bit manipulation. Using a tool called steghide we can extract any hidden message to a text file using the following command: 
```
steghide extract -sf Extinction.jpg
```
You will then be promopted for a password. When data is encrypted it becomes much harder to gain access to the hidden message. Lucky for us the person who encryted this message didn't set a password so we can just hit enter. 

You should see the following message
```
wrote extracted data to "Final_message.txt".
```
All thats left to do is to `cat` Final_message.txt and see what message awaits us (see below)
![yollo](https://raw.githubusercontent.com/sloan-ireland/Images/main/30.03.2023_10.45.02_REC.png)

Visit this [biOs wiki](https://wiki.bi0s.in/steganography/steghide/) entry to read more about steghide.

## Task #5: Erm......Magick
![Question2](https://raw.githubusercontent.com/sloan-ireland/Images/main/27.03.2023_16.44.47_REC.png)
As there is no given file to download or text to work with a safe guess is that the flag is hidden somewhere on the web page in the HTML. You can right click on different elements on the webpage to try and find the flag.
Click and highlight with the mouse. You never know what you can find. 

## Task #6: QRrrrr
![Qestion 5](https://raw.githubusercontent.com/sloan-ireland/Images/main/27.03.2023_16.55.00_REC.png)
We are given a file to download as seen in the image. Upon downloading the file, we can see it is a QR code which is shown below. Find out where it goes!
<img src="https://raw.githubusercontent.com/sloan-ireland/Images/main/QR%20COde.png" alt="QR Code" width="100%" height="25%"/>

## Task #7: Reverse it or read it
![Question 7](https://raw.githubusercontent.com/sloan-ireland/Images/main/28.03.2023_12.43.16_REC.png)
Once again we are provided a file with no prompt on any sort of process or action to do with it. The first logical thing to do with it is open it. Depending on the text editor you use, the file may not render meaning there are non-ASCII characters in the file. We can use the `cat` command to view the file in terminal. The flag is hidden somewhere in the readable characters. Searching the terminal can be a bit tedious so we can print only the wanted flag using the grep command below:
```
grep -o --binary-file=text -E 'THM{.*}' hello.hello
```
The `-o` flag returns only the matching segment of the line that matches the given pattern. The `-E` flag specifies the pattern using a regex expression that starts with THM followed by curly brackets with any number of characters in between them. The `--binary-file=text` segment forces grep to run on a binary file as grep will return an error otherwise. 

## Task #8: Another Decoding Stuff
![Question 8](https://raw.githubusercontent.com/sloan-ireland/Images/main/28.03.2023_13.04.27_REC.png)
Another base<some_number> flag! Yay! Now you get to practice using the terminal to decode this. Through eirther your skilled observation or the provided hint you have figured out this is in `base58`. Refer back to Task 2 to try and figure out the command for yourself.

## Task #9: Left or right
![Question 9](https://raw.githubusercontent.com/sloan-ireland/Images/main/30.03.2023_13.56.17_REC.png)
The answer to this task is given, but is encrypted. Rot13 is mentioned in the prompt which is a Ceasar Cipher, but is not the encryption method. This is a hint that the encryption is one of the Ceasar Ciphers. A Ceasar cipher works by rotating each letter in the text by a certain number of letters. If the shift was one then A -> B, B -> C, C -> D and so on. Letters wrap back around so Z -> A (shift = 1). You can write a program to do this pretty easily and then analyze the freuquecny analaysis of each rotated string. By comparing the letter frequcny of each string to that of normal english you can decode the string. You can also just print out all possible rotated strings (there are only 25) and see which one makes sense. Check out this link from [Tutorials point](https://www.tutorialspoint.com/caesar-cipher-in-cryptography#) to read more about Ceasar ciphers and see how to write a decoder in python. 

For those that are too lazy to write their own code, you can use the [Dcode](https://www.dcode.fr/caesar-cipher) Ceasar cipher decoder.

![dcoe 14](https://raw.githubusercontent.com/sloan-ireland/Images/main/02.04.2023_17.30.59_REC.png)

## Task 10: Make a Comment
![Task 10](https://raw.githubusercontent.com/sloan-ireland/Images/main/30.03.2023_13.55.44_REC.png)
Nothing is provided. No text, image or data to work with. But we are provided with a hint. The title of the task. Comments made in the HTML of a webpage are not visible unless one looks at the raw webpage code. Right click on some text in the task, hit inspect to view the HTML. Happy hunting!

![HTML](https://raw.githubusercontent.com/sloan-ireland/Images/main/30.03.2023_13.53.59_REC.png)

## Task 11: 
A broken PNG file. After downloading the image, it becomes clear we cannot view the contents of the image. This means the file data must be corrupted. Let's take a look at the bytes of the file using `xxd`. Using the command below we can write the hexdump to a file (name it whatever) without any metadata.
```
xxd -p spoil.png > hexdump.txt
```
Now when we `cat` the file we can see that the file is corrupted. PNG type files start with an eight byte signature shown below in hex: 
```
89 50 4E 47 0d 0a 1a 0a
```
The first line of the hexdump file is shown below and the first couple of bytes clearly don't match the signature. 
```
2333445f0d0a1a0a0000000d4948445200000320000003200806000000db
```
We can use a text editor to change the first eight bytes so they match the PNG file signature. Then using this next command we can reverse the hexdump back to binary and saved it as the original image. The image can now be opened without any problems.
```
xxd -p -r hexdump.txt > spoil.png
```

## Task 12: Read it
![q12](https://raw.githubusercontent.com/sloan-ireland/Images/main/30.03.2023_17.57.36_REC.png)
This is a very difficult task. The flag is hidden on a THM social media which according to the hint is Reddit. Once you find the tryhackme subreddit, search for a post called "New Room Coming Soon." The flag is under that post. 

## Task 13: Spin my head
![q13](https://raw.githubusercontent.com/sloan-ireland/Images/main/31.03.2023_10.46.19_REC.png)
As can be seen by looking at the hint, this language is known as Brainf_ck. Trying to decipher into plaintext by hand is a pain. Use the Brainf_ck interpreter on [dcode](https://www.dcode.fr/brainfuck-language) to the flag to plaintext. 

![Dcode](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*cXDmfpsgpsSt1O2bRAtZ6A.png)

## Task 14: An exclusive!
![task 24](https://raw.githubusercontent.com/sloan-ireland/Images/main/02.04.2023_17.46.56_REC.png)

S1 and S2. Two strings! Oh what to do? According to the hint we need to XOR the two strings together. XOR stands for 'exclsive or' and is an bitwise logical operator that uses two binary values. It returns a 1 if and only if between corresponding bits only one operand is a 1. Writing a program in any language to do this is pretty straightforward. An example of a python program that does this is below.
```python
#!/usr/bin/env python3
string1 = "44585d6b2368737c65252166234f20626d" 
string2 = "1010101010101010101010101010101010"

result = "" 
for i in range(0, len(string1), 2): # loop through the strings in pairs of 2
# Get the hexadecimal pairs from each string
pair1 = string1[i:i+2]
pair2 = string2[i:i+2]

# XOR the two pairs together
xor_val = int(pair1, 16) ^ int(pair2, 16)

# Convert the result to a character and add it to the result string
result += chr(xor_val)

print(result)
``` 
Like most tasks though, this can also be done with an online converter such as [this](https://xor.pw/#) one. 

![decode 14](https://raw.githubusercontent.com/sloan-ireland/Images/main/02.04.2023_17.47.18_REC.png)

## Task 15: 
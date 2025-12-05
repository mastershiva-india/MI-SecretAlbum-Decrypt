# MI-SecretAlbum-Decrypt
MIUI Cloud Decryptor is an open-source Python tool for decrypting Xiaomi MIUI Gallery's hidden cloud files in .lsa (photos) and .lsav (video) formats.

# MI-SecretAlbum-Decrypt makes up the loop holes of welknown as MIUI-Cloud-Decryptor tools By [ObikBobik](https://github.com/ObikBobik).

Why I'm saying Loop holes, BCz Provided [decryptor tool](https://github.com/ObikBobik/miui-cloud-decryptor). 
which I have used in my try. But this is not that easy at all. Here I'm Giving You the actual step by step guide and all files settings.

MI-SecretAlbum-Decrypt - Another Python tool for decrypting Xiaomi MIUI Gallery's hidden cloud files in .lsa (photos) and .lsav (video) .sa and .sav formats

## How does it work?
MIUI gallery app uses AES for encrypting its photos/videos in CTR mode using the IV `byte[] sAesIv = {17, 19, 33, 35, 49, 51, 65, 67, 81, 83, 97, 102, 103, 104, 113, 114};` and the first 16 bytes of the gallery apk's certificate as the key

NOTE : You Don't Need User's Passwords. Email ID, or mobile or key file.

# WHAT YOU NEED TO GO :

1. MI Gallery APK     :  [ 2.3.X Version ](https://www.apkmirror.com/uploads/page/8/?appcategory=miui-gallery)
2. Java JDK           :  [ Latest Version ](https://www.oracle.com/in/java/technologies/downloads/tools/)
3. OPENSSH            :  [ Latest Version ](https://slproweb.com/products/Win32OpenSSL.html)
4. GIT                :  [ Latest Version ](https://git-scm.com/install/windows)
5. PYTHON             :  [ Latest Version ](https://www.python.org/downloads/)
6. 7ZIP               :  [ Latest Version ]

# STEP BY STEP GUIDE TO  : I'm doing it on windows pc.

1. Install Python as normaly done
2. openssh normally
3. java jdk same way
4. git also same way
5. Extract MI Gallery APK to C: As like c:/miui-cloud-decryptor/com.miui.gallery_2.2.16.23
6. And copy com.miui.gallery_2.2.16.23 app as com.miui.gallery.apk in c:/miui-cloud-decryptor/
7. Download [All files](https://github.com/mastershiva-india/MI-SecretAlbum-Decrypt) to same path c:/miui-cloud-decryptor/
8. Extract .git.zip as c:/miui-cloud-decryptor/.git and hide it

# Important note / Check File details : ext. Should be .lsa / .lsav / .sav/ .sa 

Files names can be : Examples 

415748.f648a934d92779cf9b1b5f43db3a1e36.sa

0000_101022.3e751332435bfad27569ca4efed1b602.lsav

VID-00000225-xx0000.bed57c2aa376ffb6491fbcf7313a90e5.sav

IMG-00000022-xx0000.3e751332435bfad27569ca4efed1b602.lsa

The encrypted file must have the following name: <file name>.<md5 of key>.lsa and if the md5 of the key is 3e751332435bfad27569ca4efed1b602 it probably will be 100% decrypted but if not it probably won't decrypt the file correctly. 
You'll have to change secretKey to the first 16 bytes of the gallery apk's certificate:

# Now The Process : 

Goto that path and Shift + Right Click under That folder open command prompt :
```
cd miui-cloud-decryptor
```
Install dependencies: Execute to set up required Python packages
```
pip install -r requirements.txt
```
```
pip install filetype pycryptodome
```
Prepare files: Locate your encrypted .lsa / .lsav / .sav / .sa files, typically in MIUI/Gallery/cloud/secretAlbum on Xiaomi devices or you saved it in your device as secretAlbum.

Run decryption: Use
```
python miui-cloud-decrypt.py
```
Place your .lsa/.lsav/.sav/.sa files: Copy them into the miui-cloud-decryptor folder or note their full path (e.g., c:/miui-cloud-decryptor/<filename>.lsa)
Verify files: Run 
```
dir *.lsa *.lsav *.sa
```
To list them. They should follow format like photo.3e751332435bfad27569ca4efed1b602.lsa

Decrypt single file: 
```
python miui-cloud-decrypt.py "filename.lsa"
```
(use quotes if spaces in name).

Decrypt folder: 
```
python miui-cloud-decrypt.py "."
```
(current directory) or 
```
python miui-cloud-decrypt.py "c:/miui-cloud-decryptor/<filename>.lsa"
```
Check output: Decrypted JPG/PNG photos or MP4 videos appear in same folder with original timestamps.

Quick Test
Run dir to see current contents, then python miui-cloud-decrypt.py --help to confirm script works. Paste next output or errors for further help!

# You can see : LSAV AND SAV DONE BUT SA Files not done. Why ?

The .sa files from MIUI Gallery are another type of encrypted file format, similar but distinct from .lsa and .lsav files. Current tools like miui-cloud-decryptor primarily support .lsa and .lsav decryption
To decrypt .sa files, you generally need the same AES CTR mode decryption using the Xiaomi Gallery APK certificate key, but these files have a different internal structure, which makes decryption more complex. 
Community sources mention that .sa decryption can require specialized tools or updated scripts that handle this variant explicitly.

In command prompt ( C:\miui-cloud-decryptor\com.miui.gallery_2.2.16.23> ) use :
```
DIR
```
If you already did the same thing as i said before. 
Extract MI Gallery APK Certificate Key Perfect! You have the MIUI Gallery APK (com.miui.gallery_2.2.16.23) ready. The .sa files use the same AES key from this APK's certificate as .lsa/.lsav files

Now Check the Python Version :
```
python --version
```

Steps to Extract Key, Before that Install Java JDK (if not done) : Extract certificate (run from APK directory), Look for the CERTIFICATE block (base64 text between -----BEGIN CERTIFICATE----- and -----END CERTIFICATE-----).
Convert to hex key: Copy base64 certificate (exclude BEGIN/END lines) and open and edit extract_key.py with CERTIFICATE key and save as same name. ( What to copy ? just open extract_key.py file you will know what to paste there )

# Step 1: Extract APK Certificate
Run this command (copies certificate in RFC format):

If you closed cmd.exe : 
```
cd C:\miui-cloud-decryptor
keytool -printcert -rfc -jarfile com.miui.gallery.apk
```
If you not closed cmd.exe :
```
keytool -printcert -rfc -jarfile com.miui.gallery.apk
```
Expected output starts with: ( Don't Copy it its just output example )
```
-----BEGIN CERTIFICATE-----
MIIC... (long base64 lines)
-----END CERTIFICATE-----
```
Copy ONLY the base64 lines (everything between BEGIN/END, exclude the headers).​

# Step 2: Create Key Extractor
A. Open Notepad

B. Paste exactly:
```
python
import base64
import hashlib

# Paste certificate base64 here (exclude BEGIN/END lines)
cert_base64 = """MIIC...PASTE_YOUR_BASE64_HERE..."""
cert_bytes = base64.b64decode(cert_base64)
key = cert_bytes[:16]  # First 16 bytes = AES key
print("AES Key (hex):", key.hex())
print("Key MD5:", hashlib.md5(key).hexdigest())
```
C. File → Save As → extract_key.py → Save as type: All Files → Save in C:\miui-cloud-decryptor

# Step 3: Run & Decrypt
A. Paste base64 into script → Save

B. python extract_key.py → Should show MD5: 3e751332435bfad27569ca4efed1b602

C. Test: python miui-cloud-decrypt.py "20180303_223933.jpg.6a4ec4a5bdf52af4cc649010c9ded37d.sa"

Run keytool NOW and paste the full output!
```
keytool -printcert -rfc -jarfile com.miui.gallery.apk
```
This gives us the decryption key for all your .sa files!

AES Key Extracted Successfully!
Your MIUI Gallery APK certificate is valid FOR expires 2039. Here's the decryption key for your .sa files:

Run This NOW
Copy above code → Notepad → Save As extract_key.py (All Files) → C:\miui-cloud-decryptor

Run: 
```
python extract_key.py
```
Expected output:
```
AES Key (hex): 4e166d2a6ff249b5d...
Key MD5: 3e751332435bfad27569ca4efed1b602
```
Decrypt Your Files at once : 
```
python miui-cloud-decrypt.py *.sa
```
Run the decrypt command and check for JPG files!
```
dir *.jpg
```

All Done Then Give me a star to this repo. Follow me.

** If you want any help on Crypto or Programming:

You Can Join Our Community [ TG GROUP ](https://t.me/Web3brothersFamily)
You Can contact me on My [ TG ID ](https://t.me/mastershivay).



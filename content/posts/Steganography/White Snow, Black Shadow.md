---
title: White Snow, Black Shadow
blogshare: true
date: 2018-07-16
category: Steganography
ctf: Meepwn Quals 2018
---
The file downloaded is a jpg:
![[_attachments/evidence.jpg|_attachments/evidence.jpg]]
First thing that comes to mind is some steganography, let's check with StegSolve:
![[_attachments/evidence_steg.gif|_attachments/evidence_steg.gif]]Nope. Let's try binwalk:
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
30            0x1E            TIFF image data, big-endian, offset of first image directory: 8
217428        0x35154         End of Zip archive, footer length: 22
```
So there is a Zip footer but no header! Let's have a look at in a hex editor and look for a possible zip header. Lets search for "PK" as zip headers start with the initials of Phil Katz, the creator of the zip format.

And fair enough, we found one:
![[_attachments/hex.png|_attachments/hex.png]]
Hex:
```
50 4b 05 06
```

According to Wikipedia, this is the magic number for an empty zip file. Let's change it to `50 4b 03 04` for a regular one. Extract with binwalk and we get a valid zip file containing a pdf called message.pdf.

At this point I fiddled around with pdftools for about an hour, trying to repair this broken pdf, extracting any streams, etc.
But eventually, I decided to have another read of the document.

> “When you have eliminated the impossible, whatever remains, however improbable, must be the truth.” - Sir Arthur Conan Doyle`

Interesting! But it can't be that easy, right? I copied the whole document and pasted it in normal text editor and noticed something odd:
![[_attachments/textedit.png]]
That looks like a flag! Compare to the visible text and only keeping the differences, we get the flag:

`MeePwnCTF{T3xt_Und3r_t3Xt!!!!}`
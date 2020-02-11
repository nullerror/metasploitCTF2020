# metasploitCTF2020

3_of_spades.png

Someone else on my team popped port 25 and got hold of an /etc/passwd file, so I can't take credit for finding it. They kindly dumped some of the cracked hashes in our shared channel.

At this point, I kind of randomly just tried a few of the credentials on various services and hit one for port 22. Just a little credential spraying, I suppose. I actually forgot which one
got the initial foothold, and my note taking wasn't that good so my apologies :(

After logging into the box called 'finger' on port 22, I noticed, rather obviously, that there was a user 'ken' that had a home folder. After a bit of enumeration that did not turn up anything
too interesting (except for a mail daemon running localhost as root....) I wanted to see what his shell was, so I had another look at /etc/password and noticed his last name 'Thompson'. That's pretty interesting, I thought. He created Unix. I remember reading a while ago that someone cracked his original password from a dump of a BSD source tree.

A google search revealed a page that listed his password:
https://thehackernews.com/2019/10/unix-bsd-password-cracked.html
Thompson's password has been revealed as "p/q2-q4!a"

... but it didn't work, and I felt dumb! I typically feel dumb, so that's not the issue. That *must* be it. This is a CTF, and surely they wouldn't troll this hard. I looked for other pages to see if maybe someone just put a wrong password out there. People make mistakes typing all the tmei. I quickly came across another page with the correct password due to my impressive google-fu:

https://www.theregister.co.uk/2019/10/09/ken_thompsons_old_unix_password_cracked/
The page correctly revealed the password as "p/q2-q4!". Looks like someone just fat fingered an 'a' in the first page. Nice job, buddy :)

After switching to 'ken' and rummaging through his personal stuff, I found what I throught was the 3_of_spades.png. I thought this because that was the name of the file. I took the md5sum of it and the metasploit
CTF flag page was not happy. Not to prematurely 'file' anything, I went back and did my business. I 'file'd the heck out of that file, and it was 'data'. Crap. Must be encrypted, or must be a sick joke. I tried a few of the common decryption methods with openssl and a few different passwords and tried using to root key from 'finger'. None of them worked. Next I created a little for/awk/sed/grep monstrosity loop in bash to grab all the openssl
ciphers from openssl --help and try to decrypt it with a wordlist. Nothing worked and I kept getting an error about a magic number. It was taking too long anyway, so I figured that was a clue that dis was not dey way. Other voodoo is necessary here. Side note, I also took a look at the file and noticed 'MZ' and thought it might be a mixed up DOS executable. I made a script to swap some bytes around just for the heck of it, but then my CTF brain
told me to stop, there are no Windows boxes on this CTF.

I figured I'd read a little bit more about Ken Thompson and came across some random interview where he mentioned XOR. Ah that's nice. I remember doing this manually in a few years back in college. The next idea
was to use xxd to dump out the binary for the first 8 or so bytes of a valid PNG file, and then do the same for the data file. I loaded up both in excel and proceeded to do the binary operations on each bit myself. It
didn't take long, but the text was small and I was squinting and not really having a good time. Either way, once I was done and converted the new binary data to ASCII, it came out to "MZMZMZ..." Which was actually mentioned in the raw data file a few times, so this might have just been a clue, or a troll. Or both. I'll go with both.

I spent the next 45 minutes looking for a tool to actually XOR the whole file with the key. I never had to do this on a CTF before and I couldn't find anything that actually worked. A few tools claimed to do it, but none of the ones I tried accepted the key. I was probably using them wrong. Oh well. After some "XOR Online" google searches, I came across a tool called 'Cyberchef' that can not only decode a file you upload, given an XOR key, but it can actually find simple XOR keys like "MZMZMZ...". I came across CyberChef a while back, but it didn't enter my mind to use it. So, another lesson learned: You will quickly forget the best tool for the job.

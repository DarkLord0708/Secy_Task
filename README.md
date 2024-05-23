# Secy_Task

We were given a URL to the DVWA website `http://74.225.254.195:2324/` when we open it the first things we see are:

![Image 1](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0006.jpg)

![Image 2](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0007.jpg)

![Image 3](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0012.jpg)


The challenge stated that we need to get to the routes.txt file, but enetering it directly in the URL didn't work. So I tried to access robots.txt but it returned:

![Image 4](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0011.jpg)


But looking at the gallery page's HTML, we can see the path of the images that are being shsown there, so I copied that path and tried to access routes.txt,
`http://74.225.254.195:2324//getFile?file=/home/kaptaan/PClub-DVWA/routes.txt` gave  routes.txt

now the fisrt flag was here in routes.txt and it also had a few more directory routes:

![Image 5](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0013.jpg)


tried accessing all the pages but only, `/ipDetails` and `/secretary_login` were giving something useful.
The `/ipDetails` page looks like:
![Image 6](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0014.jpg)

now this page takes an IP address as input and returns it location, so I figured that it was using some kind of geolocation API

![Image 7](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0015.jpg)

also it was hinted in `/routes.txt` that we have to find the mac address of the machine using command injection. For command injection I tried using ampsand (&) which basically separate two commands.
so using `& whoami` worked:

![Image 8](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0016.jpg)

so for getting the mac address I used `& ifconfig` and this gave us:

![Image 9](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0017.jpg)

and on confirming with the PClub seniors, I founf out that the mac address was a flag itself i.e. pclub{mac_address} is the flag.

Now moving to the `/secretary_login` page. This takes two inputs, a username and a password for logging in the secretaries of PClub.
I tried `admin` and `admin` ass username and password and it gave:

![Image 10](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0019.jpg)

As I know that amansg22 and ritvikg22 are secys of PClub I tried their names as username and surprisingly this gave incorrect password, which means that the usernames are correct:

![Image 11](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0020.jpg)
![Image 12](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0022.jpg)


Also `kaptaan` was a valid username as well:

![Image 13](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0021.jpg)

But we don't have any passwords to get through. As we got the username `kaptaan` from the `/ipDetails` page, I tried entering some other commands.
Apparently most of the commands were not allowed like `cd`,`ls`.`echo`,`grep`,etc. So, I put this into burpsuite and tried all the commands I knew:

![Image 14](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0018.jpg)

and we got a lot of outputs but nothing useful here, `dir` worked as a alternative as `ls`, also `strings` was working, so we got:

![Image 15](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/WhatsApp%20Image%202024-05-23%20at%2007.52.46_71cecf09.jpg)

![Image 16](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/WhatsApp%20Image%202024-05-23%20at%2007.52.46_d63253a4.jpg)

And this gave us all the usernames and passwords for the login page also we get the list of all the commands that are not allowed. But looking at this output I though there might be some kind of database which would be containing all these usernames and passwords also strings gave us the .js script of blogs page. Now looking carefully at the js script of blogs page:

![Image 17](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0012.jpg)

if part == link the blogIndex is processed with setAttribute and it gets a link from the backend data, I thought it would be worth a try to see if we can do some SQL injection in this to leak the databases.So, I was wondering if `blog` could help us inject some queries, so I tried entering `'` this is blog and got a server error:

![Image](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0041.jpg)

So I opened `sqlmap` and the command was `sqlmap -u 'http://74.225.254.195:2324/getBlogdeatil?blog=2&part=link'`:

![Image 18](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0023.jpg)

so it says that it is susceptible to sql injection and the back end was rnning mySQL. So, I used `sqlmap -u 'http://74.225.254.195:2324/getBlogdeatil?blog=2&part=link' --dump`:

![Image 19](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0024.jpg)

![Image 20](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0025.jpg)

well this gave us the passwords as hashes, I used `https://crackstation.net` to crack the hashes:

![Image 21](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0026.jpg)

![Image 21](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0027.jpg)

![Image 22](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0030.jpg)

so the input pairs are like this:
`amansg22 -> SHAZISSXY`
`ritvikg22 -> cocacolaborasi`
`kaptaan -> 0123456789`
Logging in using these gave:
* As `kaptaan`:
 
  ![Image 23](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0029.jpg)
  
  this had a file which had the flag in it:
  
  ![Image 24](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0033.jpg)

* As `amansg22`:
  
  ![Image 25](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0028.jpg)
  
  this had an image:
  
  ![Image 26](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0032.jpg)
  
  so this looked like a stegano problem, I used `stegoveriats` for this problem, the command was `stegoveritas ariitk.jpeg`, This gave:
  
  ![Image 27](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0035.jpg)
  
  `stegoveritas` does all the stuff that `exiftool`, `binwalk` and `stegosolve` does plus some extra things. So the result gave me a QR code that was hidden in the ariitk.jpeg.
  
  ![Image 28](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0036.jpg)
  
  ![Image 29](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0037.jpg)
  
  On scanning the QR I got a base64 string `Y3B5aG97cW5nbl8xZl8zaTNlbGd1MWF0fQ==`, on decoding it we got the flag in rot13 and decrypting that gave the flag.
  The commands I used there are `echo "Y3B5aG97cW5nbl8xZl8zaTNlbGd1MWF0fQ==" | base64 -d` and used the output of this was `cpyho{qngn_1f_3i3elgu1at}` so for rot13 I used
  `echo "cpyho{qngn_1f_3i3elgu1at}" | tr "A-Za-z" "N-ZA-Mn-za-m"` and this gave the flag.

* As `ritvikg22`:
  
  ![Image 30](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0031.jpg)
  
  This file had a link:
  
  ![Image 31](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0034.jpg)
  
  this link led us to the next challenge:
  
  ![Image 32](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0040.jpg)




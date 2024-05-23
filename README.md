# Secy_Task

We were given a URL to the DVWA website `http://74.225.254.195:2324/` when we open it the first things we see are:
![Image 1](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0006.jpg)
![Image 2](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0007.jpg)
![Image 3](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0012.jpg)

The challenge stated that we need to get to the routes.txt file, but enetering it directly in the URL didn't work. So I tried to access robots.txt but it returned:
![Image 4](https://github.com/DarkLord0708/Secy_Task/blob/main/writeup_resources/IMG-20240519-WA0011.jpg)

But looking at the gallery page's HTML, we can see the path of the images that are being shsown there, so I copied that path and tried to access routes.txt,
`http://74.225.254.195:2324//getFile?file=/home/kaptaan/PClub-DVWA/routes.txt` gave  routes.txt

<h1>Azure Honeypot with Geolocation</h1>

<h2>Description</h2>
For this project I set up a juicy honeypot in Azure (using their free $200 credit of course) and logged all IP addresses that attempted to connect remotely. I then used an IP geolocation API to plot these attacks on a global map in Azure. The IPs were grabbed and automatically logged using a Powershell script.
<br />


<h2>Languages and Utilities Used</h2>

- <b>Microsoft Azure</b> 
- <b>Powershell</b>
- <b>Sentinel</b>
- <b>Log workspace</b>
- <b>IP Geolocation API</b>

<h2>Project walk-through:</h2>

<p>
<h3 align="center">Initial setup</h3> <br/>
I decided to opt for a physical server as that is my preference and I felt it would be more of a challenge to set up and get running, instead of using any preconfigurations with cloud software. I had an Acer laptop lying around that my father never used, so it seemed like the perfect piece of hardware to repurpose into something useful. The first thing I did was physically integrate it into my SOHO network by simply using a category 5e ethernet cable and plugging it into my nice little tplink switch. <br/>
 It had Windows 11 installed on it, but obviously that was not going to do. I opted for an Ubuntu server since it was completely free and it would give me more experience with Linux as a whole, also it would be more lightweight. Configuration was fairly simple, but I did have some <a href="https://www.youtube.com/watch?v=2Btkx9toufg">help</a> to make sure nothing would instantly destroy itself. I made sure to give it a static IP too, so I could always easily access it from my other computers. After creating another account, so that I was not always in root, and make sure all the packages were updated. it was pretty much sqaured away for me to start adding programs and experimenting. 
<br />
<br />
<h3 align="center">Programs</h3> <br/>
  SSH is the first thing I had to test out. I had only used it before in some tryhackme.com labs, but was eager to try it on my own creation. I know its relatively simple, but being able to securely access my server from my desktop or other laptop seemed (and still seems) so cool to me. I used the PuTTY application on my Windows desktop since I liked its simplicity. I made a profile for the server to make connecting even faster. Shortly after, I discovered I could add an RSA key pair to my desktop and server to make it even more secure. I used PuTTYgen to generate an RSA public and private key that I could share with the server. After moving my mouse around a bit to create some randomiziation, it was complete! It was a little tricky adding it to the server, but thanks to the video I mentioned earlier I found the corret configuration file and editied it with the SCP protocol. I also edited to configuration to disallow password connections, so it would only allow connections from those who had the RSA keys that I put in the configuration file. Being able to securely connect to the server from my desktop was cool and all, but I had a laptop which I used for studying at my workplace and I thought it would be even better if I could connect from my workplace to my server at home, securely of course. This brings me to....
<br />
<br />
 OpenVPN was what I chose to provide a secure tunnel from wherever my laptop may be back to my server at home. The first thing to do was install OpenVPN access server software on the server itself. Luckily, the OpenVPN website provides a very easy-to-follow guide for getting it set up quickly. I did so and used the credentials it gave me upon completion to access the web GUI it provides. From there I added a profile and it gave me a configuration file to download. I sent this to my laptop, which already had OpenVPN downloaded, and was able to use the configuration to start the connection between my laptop and the server through a VPN connection.
<br />
<br />
 Pi-hole definitely caught my eye with its ad-blocking abilities. I loathe the ad-infested hellscapes that are some website and wanted to provide a way around that. I already use Brave browser, but that is not as easy to use on my phone and tablet, nor does it help my family members who have probably never even heard of an ad-blocker. Installation was relatively simple, as I simply closed their repository and ran it as a bash script. I did make use of their web GUI, so I could easily connect and configure it. I kept nearly all the default settings and made sure to make note of it's IP address, as this was to be used at my home network's DNS server. So, once I got the software all set up, I had to go into my router's web GUI and set it to what Pi-hole had been assigned. After a couple reset connections, it seemed to all come working together just fine. This feature alone was well worth the time and effort.
 <br />
 <br />
 Docker is a virtualization software that would allow me to easily run 'containers' (basically applications or software that comes with all the correct configurations and dependencies). I have not actually utilized this yet, but I installed it through the command line just to have it in case I find a particular container I want to experiment with, try out, or have running on my server.
 <br />
 <br />
 Now if only I could have my own version of Google Drive set up soon...
 <br />
 <br />
More pictures and descriptions coming soom (TM)!
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

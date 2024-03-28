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

<h2>Project Walk-through:</h2>

<p>
<h3 align="center">Initial Setup</h3> <br/>
For the first part, I had to set up the Azure account. I had never used Azure before, so I used this time to get more familiar with the layout and services it provides. Azure has a lot more stuff than I ever realized, but thankfully it has a search function and decent tutorials. 
 <img src="https://i.ibb.co/Wpf6BPH/gettingstarted.png" alt="Initial setup">
<br />
<br />
<h3 align="center">Creating the Honeypot VM</h3> <br/>
 Next is getting the honeypot up in running in Azure with a VM. This was a fairly simple process. I searched for "virtual machine' in the search bar and began to fill out some of the settings. I created the initial resource group, chose US East for the region, and decided to go with a Windows 10 Professional image. Unfortunately the image does not reflect the options I chose, since I took the screenshot before configuring it. Very smart, I know.
<img src="https://i.ibb.co/p0011jq/createa-VM.png">
<br />
<br />
 <h3 align="center"> "Adjusting" the Network Security Group</h3>
Now I create a new, custom network security group under the "Networking" tab. I add a single rule that is basically an implicit allow. This will make it easy target for whichever person or machine tries to remotely connect to it. 
<img src="https://i.ibb.co/0DjFNpX/sketchyfirewall.png">
 <br />
 <br />
<h3 align="center"> Creating the Log Analystics Workspace </h3>
This is where the logs will be collected in order to be extracted to our heat map of attacks that I will eventually create. I put it in the same resource group and region just to be safe. I then had to connect it to the VM, which was quite easy as shown.
<img src="https://i.ibb.co/VCn1qr3/loganalyticsworkspace.png">
<img src="https://i.ibb.co/xGJ4qpN/connectlogsto-VM.png">
<br />
<br />
<h3 align="center">Setting up Sentinel</h3>
Now I add Sentinel to the VM to be able to monitor and map these attacks later. Fairly simple process as well, as I just search for Sentinel and add it to the honeypot workplace.
<img src="https://i.ibb.co/mNNfbfm/create-Sentinel.png">
<br />
<br />
 <h3 align="center">Connecting to the honeypot</h3>
Next step was to connect to the VM to tear down the security and check some stuff out. I simply used the Remote Desktop Connection tool from my personal desktop to connect to the public IP address given to me. Of course, I used my personal computer login information which caused me to the first failed remote connection to the honeypot. Once I used the correct credentials and adjusted the resolution, I was in. This screenshot thankfully illustrates the resource group name and the correct zone I mentioned that the VM is located in. 
 <img src="https://i.ibb.co/XtDrrRv/RDP-to-VM.png">
<br />
<br />
<h3 align="center">Making the Honeypot Sweet</h3>
 After waiting a few years for the VM to become responsive and setting up Windows, I started getting to work "adjusting" the firewall and security features first. I also checked event viewer to see the Windows security logs and get more familiar with them as a whole. After that, it was time to disable the firewall on all network types.
 <img src="https://i.ibb.co/GFPQDGj/firewalloff-VM.png">
 <img src="https://i.ibb.co/1qVykVv/eventviewlogonattemptoopsie.png">
 <br />
 <br />
 <h3 align="center">The Powershell Script</h3>
 Along with the whole guide of this project, <a href="https://www.youtube.com/@JoshMadakor">Josh Madakor</a> provided a handy Powershell script that we will be running to collect the IP addresses of the attackers. We will run this script on the VM itself. This script also includes an API from https://ipgeolocation.io/ to acquire the longitude and latitude from the IP addresses. This will also create a log file that includes information such as coordinates, source IP, country, username, and timestamps. Now we connect these logs to Azure so we can extract the data we get from running this script. 
 <img src="https://i.ibb.co/gFpFPhM/collectlogson-VM.png">
 <img src="https://i.ibb.co/KLcRSWz/samplelogfile.png">
 <br />
 <br />
<h3 align="center">Creating Our Custom Log</h3>
Now we have to bring the data into Azure so we can create our heat map. First, I simpyly copy the sample logs to recreate the file on my personal desktop. This is what we will use as a sample for our log in the Log Analytics Workspace. 
<img src="https://i.ibb.co/m9ynkQV/creatingcustomlogs-Azure.png">
 <img src="https://i.ibb.co/CtwLMRb/transferlogfile-To-Azure.png">
 <br />
 <br />
<h3 align="center">Extracting the Data into Sentinel</h3>
Now we need a way to extract the data in the correct format so we can finally plot everything out accurately. I created a workbook in Sentinel and used a custom scipt to extract each piece of infromation separately (and exclude the sample logs).
<img src="https://i.ibb.co/WGMcmGG/extractlogscript.png">
<br />
<br />
<h3 align="center">The Results</h3>
So, the IP geolocation API have 1000 free uses and I ran out in 10-15 minutes of running the script. This was mostly thanks to a guy in Seychelles as you can see below. It seems our top 3 offending countries were Seychelles, the Netherlands, and Panama. By the source IPs I saw running in the script, it seems these were results of brute force attacks as all of the attacks from these countries came from the same IP. Also, I believe my IP shows up as well because of some of my failed attempts near the beginning, whoops!
<img src="https://i.ibb.co/4dxGpJM/mapresults10minutes-APIranout.png">
<br />
<br />
<h3 align="center">Final Thoughts</h3>
This project really caught my eye initially due to the visualization of all the attacks at the end. Maybe its just me, but it helps connect evreything together when you can plot it out on a map that has more relevance to the real world than just seeing IP addresses and coordinates. This also let me get some much needed experience with Azure and Sentinel, two very prominent and powerful tools. It also introduced me into some powershell scripts which also look very handy. Logs are not a new thing, but seeing how they are created, organized, and manipulated in Azure was new, so that was also beneficial. Overall, I think this was a great project that helped introduce me to Azure, Sentinel, Powershell, Azure logs, and incident detection. It also only took a 2-3 hours. Thanks for reading.

 
</p>


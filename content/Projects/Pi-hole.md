![Pi-holeLogo](images/piholelogo.png)

## Deploying Pi-hole on my Home Network  
**Objective**: Protect Network Devices from Ads, Trackers, and Malicious Domains

### Backstory
Like everyone else, I hate to deal with ads, pop-ups, and trackers when surfing the internet. Naturally,
the question then became: **"Well, what can I do about it?"** Initially, I figured I could separately 
install tracker and ad-blocking software on each of my devices--definitely a bit tedious given the number 
of devices I would have to individually set-up, but still doable. I could always do this if I had to, but 
why not search for a better potential solution?  

### The Solution: Pi-hole
You can imagine my excitement then, when I came across **Pi-hole**: a network-wide ad-blocking software.
As an ad-blocking tool, Pi-hole offered a distinct set of advantages that made it a near-perfect solution in my eyes. Being a technology  that operates at the network level, Pi-hole confers its ad-blocking benefits 
on every device connected to the network Pi-hole is on. This completely bypasses the need to set-up one's
devices individually--my earlier concern. So if I could just get Pi-hole on my network, I would have a 
complete and efficient solution to my initial problem.  

### How it Works
The magic of Pi-hole is that it acts as a ***DNS sinkhole***, or filter, receiving any and every DNS request made on a network. Before Pi-hole allows a DNS request to proceed, it checks the domain name involved with the request against an internal block-list of domains associated with advertising, tracking, and malware. If it matches any one of these domains, Pi-hole returns a null IP address--preventing the unwanted content from loading, and effectively blocking the ad/tracker/malware. 

Network-wide, clean, and efficient.
<br> 
<div style="text-align:center;">

## Setting up Pi-hole: The Process
</div>


### Phase 1) Preparing the Host Device: Raspberry Pi

First of all, Pi-hole requires a device using a Linux-based OS to host its software. I elected to use a Raspberry Pi--a tiny, single-board computer--as the host device. I downloaded the RPi's operating system onto a microSD card using the Raspberry Pi Imager, which also allowed me to **a)** Enable SSH access to the RPi and **b)** Configure the RPi to connect to my network. I then inserted the microSD card into the RPi (which, again, contains the OS along with these pre-configured settings), and booted it up using a MicroUSB power supply. From this point onwards, through the command-line interface, I could access the system remotely via SSH using the RPi's IP address.

<img src="Images/RPi.jpg" width="300"> 
<p><strong><em>Image of a Raspberry Pi Zero 2 W.</em></strong></p>
<br>

### **Phase 2**) **Installing and Customizing Pi-hole**

The next critical step was to configure a static IP address for the device. This ensures that the IP address of the Raspbery Pi doesn't constantly change, and remains fixed, or static. This can be achieved either through assigning a "DHCP reservation" via router settings or by manually editing the network configuration on the Pi itself. I went with the first option, and through my router settings, assigned a fixed IP address to the RPi. The importance of this step will become clear later on in the process.

Then, I installed the actual Pi-hole software onto the RPi. This was accomplished by ssh'ing into the RPi's Linux-based CLI and running the following command: **curl -sSL https://install.pi-hole.net | bash**

This command fired off the installation wizard, which allowed me to configure a number of important Pi-hole settings. [**Note:** At this point, there are multiple reminders from the installation wizard to ensure that the user has set a static IP for the RPi, underscoring the critical importance of this step in ensuring Pi-hole functionality.] Firstly, I was able to choose the upstream DNS provider that Pi-hole would forward DNS lookups to. Secondly, I selected my preferred blocklist that Pi-hole would check DNS queries against. And thirdly, I was able to install and receive credentials for my Pi-hole's Admin Web Interface, which will later be used to access, manage, and monitor my network's DNS activity.
<img src="Images/installationwizard.png" width="450"> 
<p><strong><em>Pi-hole Installation Wizard</em></strong></p>
 
 <br>

### **Phase 3**) **Setting Pi-hole as the DNS Server**

At this point, Pi-hole is up and running, **but with one catch**: It is not yet operating on the network as my DNS server/sinkhole. Why? The answer is actually straightforward: Because nothing has yet been configured to make sure that DNS queries on my network are forwarded to Pi-hole. In other words, before initiating any DNS request, devices on my network don't know to forward requests to Pi-hole.

**There are two ways to solve this problem:**
1) Individually configure each of my devices to use Pi-hole as their DNS server.
2) Use my DHCP server settings to set Pi-hole as my network's primary DNS server.

**Option 2** is far more efficient because it allows me to solve the problem without having to address each network device individually (which is why I went with it). By completing this step, I was able to ensure that all DNS queries on my network are routed through Pi-hole, and are filtered through my preferred block-lists.

***Note:*** This step demonstrates the importance of giving the RPi a static IP on our network. Because: If our RPi's IP was dynamic and constantly changing, then the the Primary DNS IP address would also require constant updating for Pi-hole to remain functional on the network as our DNS server.
<br>
<div style="text-align:center;">
<strong><em>And there you go, Pi-hole is officially operational on my network!!</em></strong>
</div>
<br>
<img src="Images/dns.png" width="450"> 
<p><strong><em>Image of DNS Server Settings</em></strong></p>

<br>

### **Phase 3a**) **Enhancing Privacy: Unbound**

Although I had already set up Pi-hole successfully, I wanted to enhance privacy by removing reliance on third-party DNS providers, which I accomplished by installing ***Unbound*** on my Pi-hole. Configured to run locally, Unbound is a service that directly queries DNS root domain servers for any uncached FQDN requests. This allows me to avoid routing my network traffic through 3rd party DNS resolvers, thereby improving privacy. 
<br>
<br>

### **Phase 4**) **Verifying Functionality**

Now that Pi-hole was all set up, I just wanted to make sure that it was working as intended. I did this by logging into my Pi-hole's Admin Web Interface and verifying that DNS queries on my network were being routed through Pi-hole, which they were. 
<div style="text-align:center;">
<strong><em>Phase 4 marked the end of what was a long but rewarding project for myself.</em></strong> <strong><em>I now rest easier knowing that I am much more protected from ads, trackers, and malicious domains than I was before.</em></strong>
</div>

<br>
<img src="Images/RPiImage.jpg" width="300">
<p><strong><em>Image of my RPi Setup, powered by a MicroUSB power supply. The MicroSD Card Containing Pi-hole and all other programs is also visible.</em></strong></p>



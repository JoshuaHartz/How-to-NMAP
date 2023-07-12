# How-to-NMAP

[NMAP](https://nmap.org/download.html) is a free-to-use network security scanner for Windows, Mac, and Linux. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics.

## How to Download

I'll be going over how to download Nmap on Windows and Linux. 

### Windows

Install [here](https://nmap.org/dist/nmap-7.94-setup.exe) 

This installation is a GUI-based version of Nmap. 

Once you start the installation via the wizard, click continue and allow when needed.

Once finished downloading, type nmap in the Windows search bar and run. You will be met with this.

![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/d8bed309-c47a-4d9a-9a2e-51275208d37e)


### Linux (ubuntu)

NOTE: Kali Linux should already have Nmap installed

To install Nmap for Linux, run these commands.

```
sudo apt-get install nmap
```
and verify with 
```
nmap --version
```
## How to use Nmap

Before we begin, nmap is a scanning tool that can quickly create unwanted network traffic. If you are going to use it to scan and do recon, I encourage you to do it in your own closed environment.

For my usage, I'm repurposing my old laptop as my target host. If you can do the same, please install Wireshark on that device to track and see the packets in real time. I will use the Windows version (Zenmap) for simplicity and because it's very user-friendly. However, we do have to go over some preliminary things. Your computer's current state should only be able to scan and detect its immediate network traffic. For example, my apartment complex segments its internet by apartment, so I can only scan and listen to my apartment's devices.

Which is precisely what I'll do. 

First, we have to do some research about our network, mainly finding our IP address. In Windows, you can do this by running the command ipconfig in a terminal (WIN + R and then type cmd).

It will look like this.

![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/dca7a25c-d80b-4b82-8eef-3734bd577202)

We can see that our default gateway is 172.20.1.1, which I like to use as a starting point since we are somewhat in the dark here on how the network is set up. 

Now let's run our first command, a ping scan. A ping scan is a scan that goes around the network asking for who has what and to tell you where that address/host is. In Zenmap, under the profile section, choose "ping scan" It will autofill the command you are going to execute, but before we do that, we need to specify a target which in my case is 172.20.1.1/16 (it will be different for you). 

But where did we get the /16 from? 

If you look back to see the ipconfig listing, our subnet mask is 255.255.0.0, which using a [chart online](https://en.wikipedia.org/wiki/Wildcard_mask), translates to a /16. If we don't use this, we scan for 172.20.1.1, but we need to go through everything, so we specify /16.

![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/ac62ef74-63bb-435b-9c5d-55569c5d19e1)


![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/266bedd5-8652-419c-ae94-c05a2f9e3f3f)


now click scan and let it run its magic. While it's running, look at your device with wireshark open, you should see ARP packets flooding in asking for who has X address tell Y. This is Nmap checking for any valid hosts. I encourage you to do this on your own segmented network.

(it may take a while...)

Once you feel like enough hosts have been pinged, you can stop the scan whenever you want. Most likely, you are going to find them all right off the bat due to auto-addressing. My scan resulted in 2 hits, my PC, laptop, Roku TV, and Xbox. I know that because I turned them on for this example.

![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/91909c40-235e-44e8-8833-35fd39ae5e0e)

Now that we have these addresses. Let's intense scan them. An intense scan is a scan that not only tells us basic ip information but things about the computer like OS, open ports, version, and so on...
This command specifies an intense scan. 
```
nmap -T4 -A -v
```

Im going to run an intense scan on my Roku TV. 
![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/9f77a05b-f88f-48a1-a289-860602032710)

here are the results:

![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/5ef44fbd-521c-47f5-94ec-6997acc941ca)

It shows us: open ports, its traceroute, TCP/IP fingerprint, OS (if it can), and MAC address. 

That is only an example of what Nmap can do. There are many other options and ways to go about using Nmap. For other scan methods, look up the options you want to use and follow the same process.  

## Nmap options
[full list of options](https://nmap.org/book/man-briefoptions.html)

Options to know:

```
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery

TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks
  -iR <num hosts>: Choose random targets

SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans

PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  --exclude-ports <port ranges>: Exclude the specified ports from scanning

SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info

OS DETECTION:
  -O: Enable OS detection

TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)

OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect)

MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute
```

note: a simple port scan doesn't require any options and can just be done with the following:
```
nmap <target>
```

## Linux Basic tutorial

Using Nmap on Linux may be different visually, but all the commands and options are the same.

For example, a simple port scan on my laptop: 
```
nmap 172.20.2.137
```
However, you may want to redirect the Nmap output to a file for longevity so you don't have to sit through a scan every time you need the results. Can be done with this:
```
nmap 172.20.2.137 > nmapout.txt
```
This will send all the standard Nmap output to a file called nmapout.txt.

Another example:

```
nmap -T4 -A -v -Pn 172.20.14.74
```
Since I'm running Ubuntu out of a virtual machine, I encountered an issue where it would recognize the host. It wouldn't allow ping packets to go through, so explicitly specifying -Pn (treat all hosts as online) fixes that issue. 
Here is what results:

![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/726aa83e-0205-4525-b872-18e581a0fe13)


## Advanced section

Something you should 100% do is study what each scan looks like under Wireshark. Not only will it benefit you if you go into specific fields of cybersecurity, but it can also be highly beneficial in the Network Traffic Analysis section in NCL. Sometimes they like to have you look through captures of different types of port scans to find various artifacts and details that pertain to that scan. 





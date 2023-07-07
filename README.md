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

For my usage, I'm repurposing my old laptop as my target host. If you can do the same, please install Wireshark on that device to track and see the packets in real time. I will be using the Windows version (Zenmap) for simplicity and because it's very user-friendly. However, we do have to go over some preliminary things. Your computer's current state should only be able to scan and detect its immediate network traffic. For example, my apartment complex segments its internet by apartment, so I can only scan and listen to my apartment's devices.

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


now click scan and let it run its magic. While it's running, look at your device that has wireshark open, you should see ARP packets flooding in asking for who has X address tell Y. This is Nmap checking for any valid hosts. I encourage you to do this on your own segmented network.

(it may take a while...)

Once you feel like enough hosts have been pinged, you can stop the scan whenever you want. Most likely, you are going to find them all right off the bat due to auto-addressing. My scan resulted in 2 hits, my PC, laptop, Roku TV, and Xbox. I know that because I turned them on for this example.

![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/91909c40-235e-44e8-8833-35fd39ae5e0e)

Now that we have these addresses. Let's intense scan them. An intense scan is a scan that not only tells us basic ip information but things about the computer like OS, open ports, version, and so on...
An intense scan is specified by this command. 
```
nmap -T4 -A -v
```

Im going to run an intense scan on my Roku TV. 
![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/9f77a05b-f88f-48a1-a289-860602032710)

here are the results:

![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/5ef44fbd-521c-47f5-94ec-6997acc941ca)

It shows us: open ports, its traceroute, TCP/IP fingerprint, OS (if it can), and MAC address. 


## Nmap options
[options](https://nmap.org/book/man-briefoptions.html)

## Windows (Zenmap) basic tutorial
- port scanning
- anything else I can do legally

## Linux Basic tutorial
- port scanning
- anything else I can do legally

## Advanced section






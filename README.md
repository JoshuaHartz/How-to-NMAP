# How-to-NMAP

[NMAP](https://nmap.org/download.html) is a free to use network security scanner availiable for Windows, Mac, and Linux. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics.

## How to Download

I'll be going over how to download Nmap on Windows and Linux. 

### Windows

Install [here](https://nmap.org/dist/nmap-7.94-setup.exe) 

This installation is a GUI based version of Nmap. 

Once you start the installation via the wizard, click continue and allow when needed.

Once finished downloading type nmap in the windows search bar and run. You will be met with this.

![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/d8bed309-c47a-4d9a-9a2e-51275208d37e)


### Linux (ubuntu)

NOTE: Kali linux should already have nmap installed

To install Nmap for linux run these commands

```
sudo apt-get install nmap
```
and verify with 
```
namp --version
```
## How to use nmap

Before we begin, nmap is a scanning tool that can create alot of unwanted network traffic really fast, if you are going to use it to scan and do recon I encourage you to do it in your own closed enviroments.

For my usage Im repurposing my old laptop as my target host. If you can do the same I encourage you to install wireshark on that device to track and see the packets in real time. I will be using the windows version (Zenmap) for simplicity and because its very user friendly. However we do have to go over some preliminary things. In your computers current state it should only be able to scan and detect its immadiate networks traffic, for example, my apartment complex segments its internet by apartment so I will only be able to scan and listen to my apartments devices.

Which is exactly what Ill do. 

First we have to do some research about our network, mainly finding our ip address. In windows you can do this by running the command ipconfig in a terminal (WIN + R and then type cmd).

It will look like this.
![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/dca7a25c-d80b-4b82-8eef-3734bd577202)

We can see that our default gateway is 172.20.1.1 which I like to use as a start point since we are somewhat in the dark here on how the network is setup. 

Now lets run our first command a ping scan. In Zenmap under the profile section choose "ping scan" it will autofill the command you are going to execute, but before we do that we need to specify a target which in my case is 172.20.1.1/16 (it will be different for you). 

But where did we get the /16 from? 

If you look back to see the ipconfig listing our subnet mask is 255.255.0.0 which using a [chart online](https://en.wikipedia.org/wiki/Wildcard_mask) translates to a /16. If we dont use this we just scan for 172.20.1.1 but we need to go through everything so we specify /16.

![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/ac62ef74-63bb-435b-9c5d-55569c5d19e1)


![image](https://github.com/JoshuaHartz/How-to-NMAP/assets/102620766/266bedd5-8652-419c-ae94-c05a2f9e3f3f)


now click scan and let it run its magic. While its running look at your device that has wireshark open, you should see ARP packets flooding in asking for who has X address tell Y. This is nmap checking for any valid hosts. Which is why I encourage you to do this on your own segmented network.

(it may take awhile...)

## Nmap options


## Windows (Zenmap) basic tutorial
- port scanning
- anything else i can do legally

## Linux Basic tutorial
- port scanning
- anything else i can do legally

## Advanced section






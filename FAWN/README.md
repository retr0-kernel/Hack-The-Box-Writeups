# FAWN

My instance got created on `10.129.90.59`

> Note that this IP is different for everyone so replace your created instance IP with the IP mentioned by me throughout the walkthrough. 

I started with checking if the host is up by pinging it using: 

`ping 10.129.90.59`

After that a basic nmap scan gave me basic overview of the different open ports on the machine. 

`nmap -sC -sV 10.129.90.59`

The output was somewhat like this:

```
nmap scan report for 10.129.90.59
Host is up (0.23s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.15.204
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
Service Info: OS: Unix
```

The scan gave me a lot of information. Like a FTP (File Transfer Protocol) port is open (21). Its version, in this case - `vsftpd 3.0.3`.  So we can use ftp to gain privilege. Its a good practice to check that your machine has `ftp`.  This can be done by a simple command, `sudo apt install ftp -y`. Now we can connect using, 

`ftp 10.129.90.59` 

The output would be something like,

```
Connected to 10.129.90.59.
220 (vsFTPd 3.0.3)
Name (10.129.90.59:retr0):
```

So we can login using `username - anonymous`(Used over FTP when you want to log in without having an account) and a password of our choosing. The successful login would look like

```
Connected to 10.129.90.59.
220 (vsFTPd 3.0.3)
Name (10.129.90.59:retr0): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```

> 230 here is the response code for successful login.

Now a simple, 

`ftp> help`

would give out all possible ftp commands. The output would look like, 

```
Commands may be abbreviated.  Commands are:

!		epsv6		mget		preserve	sendport
$		exit		mkdir		progress	set
account		features	mls		prompt		site
append		fget		mlsd		proxy		size
ascii		form		mlst		put		sndbuf
bell		ftp		mode		pwd		status
binary		gate		modtime		quit		struct
bye		get		more		quote		sunique
case		glob		mput		rate		system
cd		hash		mreget		rcvbuf		tenex
cdup		help		msend		recv		throttle
chmod		idle		newer		reget		trace
close		image		nlist		remopts		type
cr		lcd		nmap		rename		umask
debug		less		ntrans		reset		unset
delete		lpage		open		restart		usage
dir		lpwd		page		rhelp		user
disconnect	ls		passive		rmdir		verbose
edit		macdef		pdir		rstatus		xferbuf
epsv		mdelete		pls		runique		?
epsv4		mdir		pmlsd		send
```

Now to list the files we can use `ls`. We get a output, 

```
29 Entering Extended Passive Mode (|||22472|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
```

Now we can see there is a file `flag.txt`. That can be downloaded to the local machine using, 

`get flag.txt`

The whole output would look like,

```
ftp> get flag.txt
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (|||31667|)
150 Opening BINARY mode data connection for flag.txt (32 bytes).
100% |*************************************************|    32       40.32 KiB/s    00:00 ETA
226 Transfer complete.
32 bytes received in 00:00 (0.14 KiB/s)
```

Now we close the ftp server, 

```
ftp> bye
221 Goodbye.
```

Now when we `ls` in our local machine we can see the file `flag.txt`. This can be opened using `cat flag.txt`.



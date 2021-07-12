---
layout: post
title: 'Connecting wifi from the command line, ubuntu or embedding ubuntu'
date: 2013-02-07 17:05:00 +0800
category: from_cnblogs
slug: p20130207170500
---
<p class="entry-title">ref:&nbsp;<a title="sonictl" href="https://linoxide.com/linux-how-to/connect-wifi-terminal-ubuntu-16-04/" target="_blank">How to Connect WiFi from Terminal on Ubuntu 16.04</a>&nbsp;(added on Dec, 2019)</p>
<p>reference:&nbsp; http://code.google.com/p/ohmm-sw/wiki/LinuxTips#Connecting_to_Networks_on_the_Ubuntu_Command_Line</p>
<p>and : http://www.ccs.neu.edu/course/cs4610/<br />
</p>
<p>&nbsp;</p>
<div id="wikiheader"><span style="font-size: 120%; font-weight: bold;">LinuxTips</span> &nbsp;
<div>
<div id="wikiauthor" style="float: right;">Updated <span title="Wed Feb  6 08:34:36 2013">
Yesterday (24 hours ago)</span> by <a class="userlink" href="http://code.google.com/u/101568621766426880142/">
martyv...@gmail.com</a> </div>


</div>


</div>
<div id="wikicontent">
<div id="wikimaincol" class="vt">
<ul>
<li><a href="http://code.google.com/p/ohmm-sw/wiki/LinuxTips#Setting_Up_Your_Own_Linux_Host">Setting Up Your Own Linux Host</a></li>
<li><a href="http://code.google.com/p/ohmm-sw/wiki/LinuxTips#Tweaks_for_a_Virtual_Machine_on_a_Flash_Drive">Tweaks for a Virtual Machine on a Flash Drive</a></li>
<li><a href="http://code.google.com/p/ohmm-sw/wiki/LinuxTips#Adding/Removing_Users">Adding/Removing Users</a></li>
<li><a href="http://code.google.com/p/ohmm-sw/wiki/LinuxTips#Connecting_to_Networks_on_the_Ubuntu_Command_Line">Connecting to Networks on the Ubuntu Command Line</a></li>
<li><a href="http://code.google.com/p/ohmm-sw/wiki/LinuxTips#Passwordless_SSH">Passwordless SSH</a></li>
<li><a href="http://code.google.com/p/ohmm-sw/wiki/LinuxTips#Remote_X_Window_Display">Remote X Window Display</a>
<ul>
<li><a href="http://code.google.com/p/ohmm-sw/wiki/LinuxTips#History">History</a></li>
<li><a href="http://code.google.com/p/ohmm-sw/wiki/LinuxTips#The_Old_Way">The Old Way</a></li>
<li><a href="http://code.google.com/p/ohmm-sw/wiki/LinuxTips#The_Modern_Way:_Connecting_from_Server_to_Client">The Modern Way: Connecting from Server to Client</a></li>
<li><a href="http://code.google.com/p/ohmm-sw/wiki/LinuxTips#The_Modern_Way:_Connecting_from_Client_to_Server">The Modern Way: Connecting from Client to Server</a></li>

</ul>


</li>

</ul>
<h1><a name="Setting_Up_Your_Own_Linux_Host"></a>Setting Up Your Own Linux Host</h1>
<p>We recommend using Ubuntu Precise (12.04) hosts for working with OHMM. If you are using OHMM in a course, the provided Ubuntu installations should already be configured. However, if you want to use your own installation, the recommended packages are (install
 with <tt>sudo apt-get install</tt>): </p>
<pre class="prettyprint"><span class="pln">build</span><span class="pun">-</span><span class="pln">essential yasm
subversion
avr</span><span class="pun">-</span><span class="pln">libc avrdude
minicom ckermit screen
openssh</span><span class="pun">-</span><span class="pln">server
openjdk</span><span class="pun">-</span><span class="lit">6</span><span class="pun">-</span><span class="pln">jdk
librxtx</span><span class="pun">-</span><span class="pln">java
libcv</span><span class="pun">-</span><span class="pln">dev libcvaux</span><span class="pun">-</span><span class="pln">dev libhighgui</span><span class="pun">-</span><span class="pln">dev python</span><span class="pun">-</span><span class="pln">opencv
libusb</span><span class="pun">-</span><span class="lit">1.0</span><span class="pun">-</span><span class="lit">0</span><span class="pun">-</span><span class="pln">dev
freeglut3</span></pre>
<p>Note: to fix a bug with current distributions of avrdude, <tt>sudo vi /etc/avrdude.conf</tt> and set<tt>chip_erase_delay</tt> to 55000 for m324p and m1284p. Or just run these commands (assuming you are running the default<tt>sh</tt>, <tt>bash</tt>, or <tt>dash</tt> shell) to make the edits automatically:</p>
<pre class="prettyprint"><span class="pln">avrparts</span><span class="pun">=</span><span class="str">"m324p m1284p"</span><span class="kwd">for</span><span class="pln"> p </span><span class="kwd">in</span><span class="pln"> $avrparts</span><span class="pun">;</span><span class="kwd">do</span><span class="pln">
&nbsp; sudo sed </span><span class="pun">-</span><span class="pln">i </span><span class="pun">-</span><span class="pln">e </span><span class="str">"/id.*=.*$p/,/part/ s/chip_erase_delay.*=.*/chip_erase_delay = 55000;/"</span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">avrdude</span><span class="pun">.</span><span class="pln">conf
</span><span class="kwd">done</span></pre>
<p>Some libraries, such as javacv, jscheme, and libpololu-avr that are needed by the OHMM code are not available from the standard repositories or require custom patches. We have packaged these as<tt>.deb</tt> packages for OHMM. You can install them by checking out the ohmm-sw open source repository and running these commands</p>
<pre class="prettyprint"><span class="pln">cd ohmm</span><span class="pun">-</span><span class="pln">sw</span><span class="pun">/</span><span class="pln">ext
make</span></pre>
<p>This will install all the packages in subdirectories of <tt>ext</tt>, except those subdirectories which contain a file named<tt>DISABLED</tt>, which are not currently needed.</p>
<p>Optional packages:</p>
<pre class="prettyprint"><span class="pln">gcc</span><span class="pun">-</span><span class="pln">doc make</span><span class="pun">-</span><span class="pln">doc cpp</span><span class="pun">-</span><span class="pln">doc libstdc</span><span class="pun">++</span><span class="lit">6</span><span class="pun">-</span><span class="lit">4.5</span><span class="pun">-</span><span class="pln">doc
cmake ant mercurial git</span><span class="pun">-</span><span class="pln">core
ntp traceroute x2x lynx
emacs23
eclipse
gimp inkscape transfig pstoedit
mplayer mencoder ffmpeg</span></pre>
<p>Other configuration:</p>
<ul>
<li>The default permissions of newly created users on Unbuntu don't include some important group memberships. In particular, membership in the<tt>adm</tt> group confers the ability to run <tt>sudo</tt>; the <tt>uucp</tt> and<tt>dialout</tt> groups enable you to connect to serial ports (as you do to connect directly to the LLP with a USB cable or to use a USB-to-serial adapter to connect to the HLP); and the<tt>video</tt> and <tt>plugdev</tt> groups can be required for using USB webcams. Run these commands to add these and other useful groups to the default for new users (i.e. run these commands before you create new users), and also to add the extra groups to the user you are currently logged in as:
<pre class="prettyprint"><span class="pln">extragrps</span><span class="pun">=</span><span class="str">"dialout cdrom floppy audio video plugdev users adm lpadmin sambashare uucp"</span><span class="pln">
sudo sed </span><span class="pun">-</span><span class="pln">i </span><span class="pun">-</span><span class="pln">e </span><span class="str">"s/^[# ]*EXTRA_GROUPS.*=.*$/EXTRA_GROUPS=\"$extragrps\"/"</span><span class="pln"> $addusrcnf
sudo sed </span><span class="pun">-</span><span class="pln">i </span><span class="pun">-</span><span class="pln">e </span><span class="str">"s/^.*ADD_EXTRA_GROUPS.*=.*/ADD_EXTRA_GROUPS=1/"</span><span class="pln"> $addusrcnf
sudo usermod </span><span class="pun">-</span><span class="pln">a </span><span class="pun">-</span><span class="pln">G </span><span class="str">`echo "$extragrps" | sed -e "s/ /,/g"`</span><span class="str">`whoami`</span></pre>
</li>
</ul>
<ul>
<li>You may want to install the <tt>iptables</tt>-based firewall we provide:
<pre class="prettyprint"><span class="pln">cd ohmm</span><span class="pun">-</span><span class="pln">sw</span><span class="pun">/</span><span class="pln">hlp</span><span class="pun">/</span><span class="pln">scripts</span><span class="pun">/</span><span class="pln">firewall
</span><span class="pun">./</span><span class="pln">install</span><span class="pun">.</span><span class="pln">sh</span></pre>
</li>
</ul>
<ul>
<li>You may want to install the minicom and kermit scripts we provide:
<pre class="prettyprint"><span class="pln">cd ohmm</span><span class="pun">-</span><span class="pln">sw</span><span class="pun">/</span><span class="pln">hlp</span><span class="pun">/</span><span class="pln">scripts
make kermitscr</span><span class="pun">.</span><span class="pln">install minicomscr</span><span class="pun">.</span><span class="pln">install</span></pre>
</li>
</ul>
<blockquote>This will allow you to run commands like <tt>minicom acm1</tt> to bring up a minicom serial terminal session on the<tt>/dev/ttyACM1</tt> port to communicate directly to the LLP, or <tt>minicom usb0</tt> to use the<tt>/dev/ttyUSB0</tt> port for a USB-to-serial adapter connected to the HLP.</blockquote>
<p>&nbsp;</p>
<h1><a name="Tweaks_for_a_Virtual_Machine_on_a_Flash_Drive"></a>Tweaks for a Virtual Machine on a Flash Drive</h1>
<p>If you specifically want to run ubuntu from a VMware virtual machine on a USB flash drive you may want to make a few optimizations that seem to significantly improve performance. These only apply to running from a flash drive; we do not recommend any of these for a regular virtual machine stored on your hard drive. First, set these values in the VMware VMX file:</p>
<pre class="prettyprint"><span class="pln">mainMem</span><span class="pun">.</span><span class="pln">backing </span><span class="pun">=</span><span class="str">"unnamed"</span><span class="pln">
mainMem</span><span class="pun">.</span><span class="pln">useNamedFile </span><span class="pun">=</span><span class="str">"FALSE"</span><span class="pln">
logging </span><span class="pun">=</span><span class="str">"FALSE"</span><span class="com"># suppress the "I moved it/I copied it question" (always moved)</span><span class="pln">
uuid</span><span class="pun">.</span><span class="pln">action </span><span class="pun">=</span><span class="str">"keep"</span></pre>
<p>Then boot the VM and make these edits in the indicated files:</p>
<pre class="prettyprint"><span class="str">/etc/</span><span class="kwd">default</span><span class="pun">/</span><span class="pln">grub</span><span class="pun">:</span><span class="pln"> GRUB_CMDLINE_LINUX_DEFAULT</span><span class="pun">=</span><span class="str">"quiet splash elevator=noop"</span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">fstab</span><span class="pun">:</span><span class="pln"> tmpfs &nbsp; &nbsp; </span><span class="pun">/</span><span class="pln">tmp &nbsp; &nbsp;tmpfs &nbsp; defaults</span><span class="pun">,</span><span class="pln">noatime</span><span class="pun">,</span><span class="pln">mode</span><span class="pun">=</span><span class="lit">1777</span><span class="pln"> &nbsp;</span><span class="lit">0</span><span class="pln"> &nbsp; </span><span class="lit">0</span></pre>
<p>The first one sets the "elevator" to "noop" which can help resolve stuttering behavior due to the default disk IO scheduler (elevator). The second puts<tt>/tmp</tt> on a ramdisk, which increases the speed of access to the temporary but frequently modified files that many programs automatically put there. Finally run</p>
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> sudo update</span><span class="pun">-</span><span class="pln">grub</span></pre>
<p>and reboot.</p>
<h1><a name="Adding/Removing_Users"></a>Adding/Removing Users</h1>
<p>To add a user:</p>
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> sudo adduser LOGIN</span></pre>
<p>where LOGIN is the desired username. <em>Note, use the command "adduser", not "useradd".</em> If you make a mistake, you can remove a user with</p>
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> sudo userdel </span><span class="pun">-</span><span class="pln">r LOGIN</span></pre>
<p>but be very careful doing that, of course (you may omit the <tt>-r</tt> to leave the user's files around at<tt>/home/LOGIN</tt>).</p>
<h1><a name="Connecting_to_Networks_on_the_Ubuntu_Command_Line"></a>Connecting to Networks on the Ubuntu Command Line</h1>
<p>You may be familiar with GUI tools, including NetworkManager in Ubuntu, to identify and connect to wireless networks. Normally you interact with NM graphically (via an icon in the task tray), but it is also possible to manipulate it from the command line. It runs as a daemon (background service) even when running headless (no GUI).</p>
<ul>
<li><strong>Listing available connections</strong>: the command
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> nmcli con list</span></pre>
</li>
</ul>
<blockquote>will show a list of the "connections" that NM knows about. "Auto eth0" is the wired ethernet network; the remainder are wireless networks that NM remembers; we have pre-configured some of these to get you started. (NM usually sets the name of a wifi connection to "Auto FOO" where FOO is the network SSID; the "Auto " prefix seems to tell NM to automatically connect to that network whenever it is available, without asking for confirmation.)</blockquote>
<ul>
<li><strong>Checking the current connection</strong>: NM is designed to automatically find for the best available connection and use it; to see what it has selected, run
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> nmcli con status</span></pre>
</li>
</ul>
<blockquote>which will show the connection that NM has currently selected, and
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> ifconfig</span></pre>
which will give the machine's IP address, if the connection was successful (look for<tt>inet addr</tt> in the section under <tt>wlan0</tt> for a wireless connection, or<tt>eth0</tt> for wired).</blockquote>
<ul>
<li><strong>Selecting a particular connection</strong> is done with the command
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> nmcli con up id </span><span class="str">'CONN'</span></pre>
</li>
</ul>
<blockquote>where CONN is one of the connection names from <tt>con list</tt>, verbatim. (The quotes allow spaces in the connection name; for example, to connect to wired ethernet use<tt>'Auto eth0'</tt>.) This will work for networks without security, or for those that use a "secret" (password) that is the same for all users of the network (in which case the secret is remembered along with the network). For networks that require username+password login, we have provided a script <tt>hlp/scripts/nm-connect-802-1x</tt>; run it at the command line to get a usage description. For networks that require you to click buttons or fill in forms on a web page, use<a href="http://en.wikipedia.org/wiki/Lynx_%28web_browser%29" rel="nofollow">lynx</a>. To bring down a connection, replace<tt>up</tt> with <tt>down</tt>.</blockquote>
<ul>
<li><strong>checking wifi signal strength</strong> of nearby networks can be done on the command line with a command like this:
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> sudo iwlist wlan0 scan </span><span class="pun">|</span><span class="pln"> grep </span><span class="pun">-</span><span class="pln">e </span><span class="typ">Quality</span><span class="pun">-</span><span class="pln">e ESSID </span><span class="pun">-</span><span class="pln">e </span><span class="typ">Mode</span></pre>
</li>
</ul>
<ul>
<li><strong>Adding new connections</strong> to NM on the command line is also possible, though somewhat arcane. We provide a script<tt>hlp/scripts/nm-create-system-connection</tt> to make it more convenient. First install it with<tt>sudo hlp/scripts/nm-scripts/install.sh</tt> (if you are using one of the pre-configured Ubuntu installations for a course, the installation step should have already been done for you). Then you can run<tt>nm-create-system-connection</tt> to get usage information. Some basic examples:
<pre class="prettyprint"><span class="com"># create a connetion to WPA protected network named ssid1, username foo, pass bar</span><span class="pln">
nm</span><span class="pun">-</span><span class="pln">create</span><span class="pun">-</span><span class="pln">system</span><span class="pun">-</span><span class="pln">connection wpa ssid1 foo bar
</span><span class="com"># now connect to it</span><span class="pln">
nmcli con up id </span><span class="str">'Auto ssid1'</span><span class="com"># create a connection to 802-1x protected network named ssid2</span><span class="com"># credentials are per-user in this case</span><span class="pln">
nm</span><span class="pun">-</span><span class="pln">create</span><span class="pun">-</span><span class="pln">system</span><span class="pun">-</span><span class="pln">connection </span><span class="lit">802</span><span class="pun">-</span><span class="lit">1x</span><span class="pln"> ssid2
</span><span class="com"># now connect to it with username foo, pass bar</span><span class="pln">
nm</span><span class="pun">-</span><span class="pln">connect</span><span class="pun">-</span><span class="lit">802</span><span class="pun">-</span><span class="lit">1x</span><span class="str">'Auto ssid2'</span><span class="pln"> foo bar</span></pre>
</li>
</ul>
<blockquote>802-1x protected networks, which require per-user credentials vs a single global password for an entire (e.g.) WPA network, are often used in institutional settings. The commands to set the username and password without a GUI are also pretty arcane so we provide the <tt>nm-connect-802-1x</tt> script to help with that.</blockquote>
<ul>
<li>If you do have the option to boot headful with a GUI, then instead of using the<tt>nm-create-system-connection</tt> command line script, you can use the more common NM graphical method to add a connection: click on the NM icon in the taskbar, select desired network from a list, and then enter your credentials. (You can also select "Connect to Hidden Wireless Network...", if the access point has been configured not to broadcast itself.) If you then go back to the NM icon and select Edit Connections -&gt; Wireless -&gt;<em>connection</em> -&gt; Edit... -&gt; Available to all users, NM will save the connection info to a file in<tt>/etc/NetworkManager/system-connections/</tt>, and it will automatically consider connecting to it at boot, even before any user has done a graphical login. This is called a<em>system</em> connection (vs a <em>user</em> connection that would only be available to the currently logged-in user).</li>
</ul>
<ul>
<li>Connecting the pandaboard HLP to a wifi network is a particularly important case for OHMM. One method to do that is to first attach a USB-to-serial adapter from the pandaboard to a PC or laptop. Bring up the connection in a serial terminal program like<tt>minicom</tt> or <tt>kermit</tt>, log in to the pandaboard, and then you can run all the commands above to configure its wifi connection. Once it is connected note down its IP address.</li>
</ul>
<blockquote>We have also provided a script <tt>ohmm-sw/hlp/scripts/nm-scripts/nm-panda-wifi-helper</tt> that can help connect the panda to wifi networks automatically (it should also already be installed for you if you are using OHMM in a course). It watches the user pushbutton on the panda. This is right next to the reset button, so be careful.</blockquote>
<blockquote><img src="http://ohmm-sw.googlecode.com/svn/wiki/images/panda-user-pushbutton.png" alt="" /></blockquote>
<blockquote>When the button is held down for about 1 second it displays information about the currently connected network (including the panda's IP address) on the LCD display on the LLP (it takes a few seconds to gather the info and display it after you press the button). Holding it down for 3 seconds triggers a procedure to try to automatically connect to a list of preferred networks from the file<tt>/etc/nm-panda-wifi-helper/ohmm-preferred-networks</tt>, with report about the effects on the LCD. Note that this currently only works for networks that don't require per-user authentication. Also, the LCD display functionality won't work if you are running other code on the pandaboard which has already connected to the orangutan LLP.</blockquote>
<h1><a name="Passwordless_SSH"></a>Passwordless SSH</h1>
<p>Say you want to ssh from a "client" machine, like your PC, to a "server", like the HLP on your robot.</p>
<ol>
<li>On the client machine run the command
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> ssh</span><span class="pun">-</span><span class="pln">keygen </span><span class="pun">-</span><span class="pln">t dsa</span></pre>
</li>
</ol>
<blockquote>If you have already done this at some point in the past, you do not want to do it again. If you are not sure, check if the file<tt>~/.ssh/id_dsa.pub</tt> exists (<tt>~</tt> is shorthand for your home directory) --- if it does, you had run<tt>ssh-keygen</tt> before.</blockquote>
<ol>
<li>Copy the generated public key to the server. On the client machine, run the command
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> scp </span><span class="pun">~</span><span class="str">/.ssh/</span><span class="pln">id_dsa</span><span class="pun">.</span><span class="pln">pub SERVER</span><span class="pun">:~/</span><span class="pln">CLIENT</span><span class="pun">-</span><span class="pln">id_dsa</span><span class="pun">.</span><span class="pln">pub</span></pre>
</li>
</ol>
<blockquote>whhere SERVER is the DNS name of the server and CLIENT is the hostname of the client.</blockquote>
<ol>
<li>ssh in to the server (using your password still) and run the commands
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> mkdir </span><span class="pun">.</span><span class="pln">ssh
</span><span class="pun">&gt;</span><span class="pln"> cat CLIENT</span><span class="pun">-</span><span class="pln">id_dsa</span><span class="pun">.</span><span class="pln">pub </span><span class="pun">&gt;&gt;</span><span class="pun">~</span><span class="str">/.ssh/</span><span class="pln">authorized_keys
</span><span class="pun">&gt;</span><span class="pln"> rm CLIENT</span><span class="pun">-</span><span class="pln">id_dsa</span><span class="pun">.</span><span class="pln">pub
</span><span class="pun">&gt;</span><span class="pln"> chmod </span><span class="lit">700</span><span class="pun">.</span><span class="pln">ssh
</span><span class="pun">&gt;</span><span class="pln"> chmod </span><span class="lit">600</span><span class="pun">.</span><span class="pln">ssh</span><span class="com">/*</span></pre>
</li>
</ol>
<blockquote>The <tt>mkdir</tt> command will give an error message if <tt>~/.ssh</tt> already exists, but this is harmless.</blockquote>
<ol>
<li>Return to the client and try to ssh in to the server again --- you should not be prompted for a password.</li>
</ol>
<h1><a name="Remote_X_Window_Display"></a>Remote X Window Display</h1>
<p>Because the onboard HLP typically runs headless (without a monitor, keyboard, and mouse attached), it can be very useful to run graphical programs on it that display on a remote computer. This is possible, though it can use a lot of network bandwidth. (For viewing images from the onboard camera we provide an HTTP image server that sends compressed JPEG or PNG images using much less bandwidth.)</p>
<h2><a name="History"></a>History</h2>
<p>The <a href="http://www.x.org/" rel="nofollow">X.org</a> display system now used in Linux is a descendant of a very old system called<a href="http://en.wikipedia.org/wiki/X_Window_System" rel="nofollow">X11</a>, or just "X". A key design aspect of X is that it is fundamentally a<em>client-server</em> architecture.</p>
<p>You may be used to thinking of interacting with web "servers" as remote machines, with your local desktop machine the "client". In X, the<em>X server</em> is a daemon (background service) program running on your desktop, i.e. on the computer with the monitor to which graphics should be displayed.<em>X clients</em> are programs that want to display graphics in windows: web browsers, terminal programs, video players, etc.</p>
<h2><a name="The_Old_Way"></a>The Old Way</h2>
<p>When you run an X client it implicitly checks the value of the DISPLAY environment variable, which has the format<tt>[</tt><em>server</em><tt>]</tt>:<em>d</em><tt>[</tt>.<em>s</em><tt>]</tt>; <em>server</em> can be the DNS name or IP address of the machine running the X server, which does not need to be the same as the machine running the client program. It can also be "localhost" or omitted, in the common case that both the X client and server are on the same machine. <em>d</em> and the optional <em>s</em> are integers that can be used to select which part of a multi-screen display should be targetted; they are usually both 0 unless you know you need something else.</p>
<p>So, it seems that if we want to run an X client program, like xterm, on one machine (let's call it<tt>client</tt>), and have it display on (possily another) machine <tt>server</tt>, then it should suffice to run these two commands on<tt>client</tt>:</p>
<pre class="prettyprint"><span class="pun">&gt;</span><span class="kwd">export</span><span class="pln"> DISPLAY</span><span class="pun">=</span><span class="pln">server</span><span class="pun">:</span><span class="lit">0.0</span><span class="pun">&gt;</span><span class="pln"> xterm </span><span class="pun">&amp;</span></pre>
<p>Once upon a time, that would have worked, almost. Even in the old days there was a security mechanism: on the server you would have had to run</p>
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> xhost </span><span class="pun">+</span><span class="pln">client</span></pre>
<p>to indicate that incoming X connections from <tt>client</tt> should be accepted. (Here we are using<tt>client</tt> and <tt>server</tt> as standins for the corresponding DNS names.)</p>
<p>Unfortunately, nowadays increased security means that the above steps alone usually do not work.</p>
<p>Also, networks are now often relatively complex; the above example assumes that both<tt>client</tt> and <tt>server</tt> can "see" each other on the network. In practice, it is now common to have "local subnets" like 192.168.1 which appear to machines outside the subnet as a single address corresponding to a router machine running<em>network address translation</em> (NAT, which used to be called "IP Masquerading", possibly a more informative name). Machines inside a local subnet can see out, but unless special arrangements are made, they cannot be seen from the outside.</p>
<p>Remote X display is usually still possible despite these challenges.</p>
<h2><a name="The_Modern_Way:_Connecting_from_Server_to_Client"></a>The Modern Way: Connecting from Server to Client</h2>
<p>The usual situation is that you are sitting at the machine running the X server (<tt>server</tt>, e.g. your PC), and you ssh from there to<tt>client</tt> (e.g. the HLP on your robot; note that we are using the server and client terms to refer to the "backwards" X connection; the ssh connection is actually from<tt>server</tt> to <tt>client</tt>.) <tt>client</tt> needs to be visible from <tt>server</tt> (but not vice-versa), and <tt>client</tt> also needs to be running an ssh server.</p>
<p>Ubuntu desktop editions to not come with an ssh server installed by default, but it can be easily added simply by running</p>
<pre class="prettyprint"><span class="pln">sudo apt</span><span class="pun">-</span><span class="kwd">get</span><span class="pln"> install openssh</span><span class="pun">-</span><span class="pln">server</span></pre>
<p>which both installs the ssh server and starts it, or does nothing if it's already installed and up to date. (If you're taking a course using OHMM and using one of the pre-configured Ubuntu installations, this step should have already been done for you.)</p>
<p>There are only two steps needed in this common <tt>server</tt>-to-<tt>client</tt> use case:</p>
<ol>
<li>Make sure <tt>X11Forwarding</tt> is set to <tt>yes</tt> in <tt>/etc/ssh/sshd_config</tt> on<tt>client</tt> (this requires root perms on <tt>client</tt> to change, of course; it should have been done for you if you are using a pre-configured Linux installation as part of a course).</li>
<li>On <tt>server</tt> run
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> ssh </span><span class="pun">-</span><span class="pln">X </span><span class="pun">-</span><span class="pln">C </span><span class="pun">[</span><span class="pln">user@</span><span class="pun">]</span><span class="pln">client</span></pre>
</li>
</ol>
<blockquote>which will log you in to <tt>client</tt> with <em>X forwarding</em> (and lossless compression) enabled. Then on<tt>client</tt> you can run
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> echo $DISPLAY</span></pre>
which should return a value like <tt>localhost:10.0</tt>. This may seem confusing; what is happening is that ssh has arranged a<em>tunnel</em> so that when you run an X client program like
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> xterm </span><span class="pun">&amp;</span></pre>
on <tt>client</tt>, it will connect to a special port on <tt>client</tt> that is tunneled back to the X server on<tt>server</tt>, all through the ssh connection.</blockquote>
<h2><a name="The_Modern_Way:_Connecting_from_Client_to_Server"></a>The Modern Way: Connecting from Client to Server</h2>
<p>It is less common, but possible, to do this all the other way around, i.e. to start on<tt>client</tt> (e.g. your HLP), establish an ssh tunnel to <tt>server</tt> (e.g. your PC), and then run X client programs on<tt>client</tt> that display on <tt>server</tt>. It is trickier to set this up. Here is a recipe:</p>
<ol>
<li>Make sure that <tt>server</tt> is visible over the network from <tt>client</tt> (i.e. you should be able to ping<tt>server</tt> from <tt>client</tt>). If <tt>server</tt> is actually a virtual machine, you will either have to configure it to use "bridged" networking (not "NAT"; this is an easy switch in VMWare settings, and does not even require a reboot), or you can install a secondary network interface device, like a USB wifi dongle, that you ask VMWare to connect directly to the virtual machine. The first option requires that the VM have privlidges to connect directly to the external network (which will not work on managed networks like at many educational institutions unless the sysadmins to allow the virtual machines directly onto the network). The second option enables the VM to potentially connect to the same wifi router to which<tt>client</tt> (e.g. your HLP) is connected, thus putting them both on the <em>same</em> local subnet.</li>
<li>Make sure that an ssh server is installed and running on <tt>server</tt> (see above).</li>
<li>In a moment we are going to be opening the X server port and we don't want just anyone to be able to connect. If you're running Ubuntu then you can try the simple firewall we provide in<tt>ohmm-sw/hlp/scripts/firewall</tt>, which plays nicely with the NetworkManager service; just run<tt>./install.sh</tt> there on your server (If you're taking a course using OHMM and using one of the pre-configured Ubuntu installations, this step should have already been done for you.).</li>
</ol>
<p>&nbsp;</p>
<ol>
<li>Log in graphically and run
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> xhost </span><span class="pun">+</span></pre>
</li>
</ol>
<blockquote>on server (this will be forgotten when you log out). This step and the last two have opened the X server TCP port 6000, which would allow any X client to connect, if it were not for the firewall. The firewall only allows<em>local</em> X clients (i.e. those that appear as if they originate from <tt>server</tt> itself), but still allows incoming ssh connections.</blockquote>
<ol>
<li>Finally, on <tt>client</tt> run
<pre class="prettyprint"><span class="pun">&gt;</span><span class="pln"> ssh </span><span class="pun">-</span><span class="pln">f </span><span class="pun">[</span><span class="pln">user@</span><span class="pun">]</span><span class="pln">server </span><span class="pun">-</span><span class="pln">L </span><span class="lit">6010</span><span class="pun">:</span><span class="pln">server</span><span class="pun">:</span><span class="lit">6000</span><span class="pun">-</span><span class="pln">N
</span><span class="pun">&gt;</span><span class="kwd">export</span><span class="pln"> DISPLAY</span><span class="pun">=</span><span class="pln">localhost</span><span class="pun">:</span><span class="lit">10.0</span><span class="pun">&gt;</span><span class="pln"> xterm </span><span class="pun">&amp;</span></pre>
</li>
</ol>
<p>The first command establishes an ssh tunnel from the X server port 6010 on <tt> client</tt> to that port on <tt>server</tt>; the second tells X client programs on<tt>client</tt> to connect to that local port (the convention is that the port number is 6000+<em>d</em> where<em>d</em> is the display number, here 10), which will be tunneled back to <tt>server</tt>.</p>
</div>
</div>
<p>&nbsp;</p>
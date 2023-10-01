<h1>Suricata Basics Lab</h1>


<h2>Description</h2>
Used Linux command line to explore signatures and logs with Suricata. I will update the main project directory with additional suricata projects as I progress.
<br />


<h2>Languages and Utilities Used</h2>

- <b>Bash</b> 
- <b>Suricata</b>

<h2>Environments Used </h2>

- <b>Linux</b>

<h2>Project walk-through:</h2>

<p align="center">
I started with 'pwd' to print the working directory and verify I am where I need to be. Then used 'ls -a' to list files in the directory. Then used 'cat custom.rules'. This returned a rule which can be observed to have an action, header, and rule options (alert, pass, drop, reject). The arrow indicates the direction of the traffic. 
    The msg: option provides the alert text. In this case, the alert will print out the text “GET on wire”, which specifies why the alert was triggered.
    The flow:established,to_server option determines that packets from the client to the server should be matched. (In this instance, a server is defined as the device responding to the initial SYN packet with a SYN-ACK packet.)
    The content:"GET" option tells Suricata to look for the word GET in the content of the http.method portion of the packet.
    The sid:12345 (signature ID) option is a unique numerical value that identifies the rule.
    The rev:3 option indicates the signature's revision which is used to identify the signature's version. Here, the revision version is 3: <br/>
<img src="https://i.imgur.com/osFtDO2.png" height="80%" width="80%" alt="First Suricata Lab"/>
<br />
<br />
Next I ran 'ls -l /var/log/suricata', which  returned 0 results. 'sudo suricata -r sample.pcap -S custom.rules -k none' This command starts the Suricata application and processes the sample.pcap file using the rules in the custom.rules file. 
    The -r sample.pcap option specifies an input file to mimic network traffic. In this case, the sample.pcap file.
    The -S custom.rules option instructs Suricata to use the rules defined in the custom.rules file.
    The -k none option instructs Suricata to disable all checksum checks. When running the previous command, 4 files are returned:  <br/>
<img src="https://i.imgur.com/3UPt1v2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
'cat /var/log/suricata/eve.json' returns a ton of log data: <br/>
<img src="https://i.imgur.com/Xq1RQKd.png" height="80%" width="80%" alt="First Suricata Lab"/>
<br />
<br />
So we shorten it with 'jq . /var/log/suricata/eve.json | less':  <br/>
<img src="https://i.imgur.com/tWTLgh8.png" height="80%" width="80%" alt="First Suricata Lab"/>
<br />
<br />
Finally I used 'jq -c "[.timestamp,.flow_id,.alert.signature,.proto,.dest_ip]" /var/log/suricata/eve.json'. Then refined it with 'jq "select(.flow_id==X)" /var/log/suricata/eve.json' (where the x is replaced with '2099990240065685'). This allowed me to grab basic data from the log:  <br/>
<img src="https://i.imgur.com/DKbCtfc.png" height="80%" width="80%" alt="First Suricata Lab"/>
<br />
<br />
<p/>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

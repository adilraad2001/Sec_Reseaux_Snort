# TP5_Sec_Reseaux

![logo-eidia](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/608744a9-d99b-4314-ab58-2828cee9a1a0)

Realisé Par : Adil Erraad

# Sommaire:

 1. **Introduction:**
 
    1. *Objectifs de ce TP :*
    
    2. *Outils logiciels :*
    
 2. **Network Topology:**
 
 3. **Conclusion:**


## 1. **Introduction:**
  1. *Objectifs de ce TP :*
    Snort is an open source network intrusion detection and prevention system that plays an important role in protecting computer networks. With its ability to analyze network traffic in real time, our goal in this application is to learn how to use it for intrusion detection and how to deal with it.

  2. *Outils logiciels :*
      machine 1: Ipcop(Firewall)-R :10.0.4.15   G:192.168.1.1   O:192.168.2.1
      machine 2: DMZ(ubuntu) : 192.168.2.10
      machine 3: Client :192.168.1.0/24 by (Ipcop)
      machine 4: Snort  :192.168.1.0/24 by (Ipcop)
      interface 1:NAT
      interface 2: Internal Network
      interface 3: Internal Network 2
  
## 2. **Network Topology:**

![2023-05-30 14_53_05-a drawio - draw io](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/1e3b583f-59d7-4142-9ef3-aa4fd22d1d5d)


As shown in the Topology we have to create three zone (GREEN:LOCAL  - Red: Internet  -ORANGE: DMZ)

And since we have already made these Configguration in the previous practical exercise, we will only need to add the intrusion detector (snort) in the local network and start setting it up with some modifications in the firewall(IPCOP).

To start the firewall settings, we must add a new law that allows the possibility of making a connection from the orange area to the green area By connecting to Setting panel from client machine to add this new rules : 

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/9e613a29-efa3-45df-b037-5cea4c068e4b)
![2023-05-30 15_15_42-Ubuntu  Running  - Oracle VM VirtualBox](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/711e3875-bc9b-4cc6-9a8a-03a8f8e08d8f)


u can do this by going to Firewall menu -> Firewall Rules -> Internal Traffic

Before:

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/58e035d6-b89e-4c0c-831b-c8ca622f1396)
![2023-05-30 15_19_38-Ubuntu  Running  - Oracle VM VirtualBox](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/5d774d5d-5c3f-42ae-8209-2a876416b1b7)


After :

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/0df136e5-1349-428d-94bd-cf0da92d434c)
![2023-05-30 15_20_24-Ubuntu  Running  - Oracle VM VirtualBox](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/482c1e49-3429-4bb1-9123-cf04b9a8d0ef)


And now lets start to install Snort and configure:

For install snort need to go into client machine and Run the command:

```bat

sudo apt update
sudo apt install snort

```

And while you installing snort see some configuration prompt:

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/b93cff5b-357a-46fd-b0ae-9ae2cf7f3017)
![2023-05-30 15_29_23-Ubuntu  Running  - Oracle VM VirtualBox](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/5fd9b4f0-d65e-4510-8a92-846e7db394d0)



this for choosing the interface where the snort will listening

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/331249a4-c4f8-4fae-b5db-889a8fc7b1ca)

![2023-05-30 15_30_50-Ubuntu  Running  - Oracle VM VirtualBox](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/37638b04-c3c1-484e-af3a-49ffeb419c97)

For choose the range of the local network in this interface

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/70c5e5c2-fd3d-4302-a39b-d4a48691c98a)
![2023-05-30 15_31_45-Ubuntu  Running  - Oracle VM VirtualBox](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/e6d4ab20-5e81-4bf8-b615-ad56904624c4)


For the promiscuous mode for the network adapter

And there is a little promt like sending email for the daily report of snort

And by this we finished the installation.

And start to the configuration But before his you need to knew some basic command

```bat

sudo snort -T -c /etc/snort/snort.conf                   // Test the Snort configuration to ensure it is valid
sudo snort -i <interface> -c /etc/snort/snort.conf      //For start snort with specific configuration file
snort -A console -i <interface>                        //start snort and show the alert in the console
snort -A alert -l <log_directory> -i <interface>      //Start snort Log Alert to filE

```
_snort.conf_: The Pricipale Configuration file
_ipvar:_  variables to specify your network's IP ranges
_alert, log, pass, or drop_ : rules can using in the configuration
_include $RULE_PATH/icmp.rules:_ To enable rules in the configuration file (in this example we enable icmp rules)

And now let's start with configuration of detction of icmp traffic:

first we need to identifier the range of network where detect

and after we need to enable icmp processeur and include the rules by write:

```bat

preprocessor icmp
include $RULE_PATH/icmp.rules

```

after this we need to go to the file of rules /etc/snort/rules/icmp.rule 
and try to write a rule for icmp detection 

```bat

alert icmp any any -> any any (msg:"Ping detected"; icode:0; itype:8; sid:1000001;)

```
_**alert**_: This keyword specifies that an alert should be generated when the condition of the rule is met. An alert typically results in a log entry and notification.
_**icmp**_: This specifies the protocol to match, in this case, ICMP.
_**any**_ any -> any any: These fields indicate the source and destination IP addresses and ports. Using any means the rule matches any IP address and port for both source and destination.
_**msg**_:"Ping Detected": This is the message associated with the alert. It provides a description or label for the alert generated by this rule.
_**icode:0**_: This field specifies the ICMP code to match. In this case, 0 represents an ICMP Echo Request.
_**itype:8**_: This field specifies the ICMP type to match. In this case, 8 represents an ICMP Echo Request.
sid:1000001: This is the unique identifier for the rule. It helps to identify and manage specific rules within Snort

Lets try this:

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/40c7c8ff-f397-48d4-a061-a618dd6d14f5)
![2023-05-30 21_33_29-Editing TP5_Sec_Reseaux_README md at main · adilraad2001_TP5_Sec_Reseaux · GitHu](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/11637208-ad2d-410b-b08a-b14a54d28dac)


As you can see we detect the icmp request:

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/29cbe195-d121-471a-a091-84721f627e1b)
![2023-05-30 21_34_31-DMZ  Running  - Oracle VM VirtualBox](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/8523374d-0408-4c68-b6a4-733133a45fdf)


it's the same thing between the different services:

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/bf3928de-ebf4-408d-98a9-b2f1d9749491)
![2023-05-30 21_22_50-Ubuntu 1  Running  - Oracle VM VirtualBox](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/5a56e703-b46a-4fb9-9b11-fbdb21c2a75c)



 3. **Conclusion:**
 
  In conclusion, Snort IDS (Intrusion Detection System) is a powerful tool for network security that helps monitor and detect potential threats and attacks.
  















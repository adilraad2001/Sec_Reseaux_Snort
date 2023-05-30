# TP5_Sec_Reseaux

![logo-eidia](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/608744a9-d99b-4314-ab58-2828cee9a1a0)

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

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/ff2226f1-8c51-42ab-bec0-be71677faf82)

As shown in the Topology we have to create three zone (GREEN:LOCAL  - Red: Internet  -ORANGE: DMZ)

And since we have already made these Configguration in the previous practical exercise, we will only need to add the intrusion detector (snort) in the local network and start setting it up with some modifications in the firewall(IPCOP).

To start the firewall settings, we must add a new law that allows the possibility of making a connection from the orange area to the green area By connecting to Setting panel from client machine to add this new rules : 

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/9e613a29-efa3-45df-b037-5cea4c068e4b)

u can do this by going to Firewall menu -> Firewall Rules -> Internal Traffic

Before:

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/58e035d6-b89e-4c0c-831b-c8ca622f1396)

After :

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/0df136e5-1349-428d-94bd-cf0da92d434c)

And now lets start to install Snort and configure:

For install snort need to go into client machine and Run the command:

```bat

sudo apt update
sudo apt install snort

```

And while you installing snort see some configuration prompt:

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/b93cff5b-357a-46fd-b0ae-9ae2cf7f3017)

this for choosing the interface where the snort will listening

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/331249a4-c4f8-4fae-b5db-889a8fc7b1ca)

For choose the range of the local network in this interface

![image](https://github.com/adilraad2001/TP5_Sec_Reseaux/assets/99618982/70c5e5c2-fd3d-4302-a39b-d4a48691c98a)

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










# -DDoS-attacks
Analysis of DDoS attacks in SDN Environment

1)Prerequisites

    1.Install Python
    2.Install mininet along with pox controller
    	mininet installation : ​ http://mininet.org/download/
    	pox controller : ​http://github.com/noxrepo/pox

2) Creating test environment
Copy the following files from src folder:
(a) Packet-Generation/traffic.py
(b) Packet-Generation/attack.py
to ​ mininet/custom
(a) POX-Detections/detectionUsingEntropy.py
(b) POX-Detections/detectionUsingPCA.py
(c) POX-Detections/l3_detectionEntropy.py
(d) POX-Detections/l3_detectionPCA.py
to ​ pox/pox/forwarding

3) Detection Using Sample Entropy
	1) Run the pox controller:
	$ cd pox
	$ python ./pox.py forwarding.​l3_detectionEntropy
	2) Create a mininet topology by entering the following command in 		another terminal:
	$ sudo mn --switch ovsk --topo tree,depth=2,fanout=8 --controller=remote,ip=127.0.0.1,port=6633 
	3) Open xterm for the following hosts:
	mininet>xterm h1 h2
	4) In the xterm window of h1, run the following commands to launch 		   traffic:
	$ cd ../mininet/custom
	$ python traffic.py –s2 –e 65
	5) Now the pox controller generates a list of values for entropy. 		The least value obtained is the threshold entropy for normal 		traffic. 
	6) Repeat step (4) on h1 and parallelly enter the following 		commands on h2 and h3 xterm windows to launch the attack:
	$ cd ../mininet/custom
	$ python attack.py 10.0.0.64
Observe the entropy values in the pox controller. The value 		decreases below the threshold value for normal traffic.Thus we can 	detect the attack.


4) Detection Using PCA
	1) Run the pox controller:
	$ cd pox
	$ python ./pox.py forwarding.​l3_detectionPCA
	2) Create a mininet topology by entering the following command in 		another terminal:
	$ sudo mn --switch ovsk --topo tree,depth=2,fanout=8 --controller=remote,ip=127.0.0.1,port=6633
	3) Open xterm for the following hosts:
	mininet>xterm h1 h2
	4) In the xterm window of h1, run the following commands to launch 		traffic:
	$ cd ../mininet/custom 
	$ python traffic.py –s2 –e 65
	5) Now the pox controller generates a list of values for deltaY, 		   which is the difference between the y-coordinates of a packet  		   and the point obtained by drawing a perpendicular from this  	    packet to the Principal Component Axis.
	6) Repeat step (4) on h1 and parallelly enter the following 		   commands on h2 and h3 xterm windows to launch the attack:
	   $ cd ../mininet/custom
           $ python attack.py 10.0.0.64
Observe the deltaY values in the pox controller. The values converge to the interval (-1, 1). Thus we can detect the attack. 


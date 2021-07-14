# Project 3
## Final Engagement: Attack, Defense, and Analysis of a Vulnerable Network and Capture the Flag
This project was completed as a duo with Sanjay Sharma.

**Scenario**

You are working as a Security Engineer for X-Corp, supporting the SOC infrastructure. The SOC analysts have noticed some discrepancies with alerting in the Kibana system and the manager has asked the Security Engineering team to investigate. 

To start, your team needs to confirm that newly created alerts are working. Once the alerts are verified to be working, you will monitor live traffic on the wire to detect and abnormalities that aren't reflected in the alerting system.

You will then report back all your finding to both the SOC manager and the Enginnering Manager with appropriate analysis.


### Configuring Kibana

We impleted 3 alerts using the path:
**Management** > **Watcher** > **Create Alert** > **Create Threshold Alert**

- **Excessive HTTP Errors**
  ```kql
  WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes
  ```
  
- **HTTP Request Size Monitor**
  ```kql
  WHEN sum() of http.request.bytes OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute
  ```

- **CPU Usage Monitor**
  ```kql
  WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes
  ```
  
### Attacking the Target

The Following Steps were followed to complete the attack.

1. Scan the network to identify the IP address of Target 1.
2. Document all exposed ports and services.
3. Enumerate the WordPress site.
4. Use SSH to gain a user shell.
5. Find the MySQL database password.
6. Use the credentials to log into mySQL and dump WordPress user password hashes.
7. Crack password hashes with `john`
8. Secure a user shell as the user whose password you cracked.
9. Escalate to `root`.

There were also 4 flags to be discovered during this attack. The attack can be summarized by viewing the Red Team: Summary of Operations.

1. Using the command `nmap 192.168.1.*` We were able to see the entire scope of the network. Target 1 had an IP address of 192.168.1.110 and the following open ports:
<details>
    <summary>2. Click here to see open ports and services used by Target 1</summary>
  <br>
  
  - Port 22: SSH
  
  - Port 80: HTTP  

  - Port 111: rpcbind

  - Port 159: netbios-ssn

  - Port 445: microsoft-ds

</details>
  
<br>

3. We used the dirb command to brute force a directory list of the website, using the command `dirb http://192.168.1.110` we were not aware of the directory for the wordPress site but this gave us access to hidden directories and it was then able to locate `http://192.168.1.110/wordpress`.
  Using google, we were then able to find a simple WordPress Security Scanner called **WPScan** to be able to enumerate the site.
  Using the command `wpscan --url http://192.168.1.110/wordpress/ --enumerate u` we able to see the results we were looking for, two users for the site were identified 'Steven' and 'Michael'.

4. Knowing that this was a vulnerable website we took a chance and started to guess password combos for the two users to gain access into the system via SSH. After discovering that michael was his own name as a password **(Bad Practice)** we able to gain access to the network using `ssh michael@192.168.1.105` and the password of 'michael'.

5. Looking for 

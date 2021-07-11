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

There were also 4 flags to discovered while finishing this attack. The attack can be summarized by viewing the Red Team: Summary of Operations.

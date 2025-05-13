# Secured and monitored web infrastructure

<p align="center">
  <img src="https://raw.githubusercontent.com/pablonudel/holbertonschool-system_engineering-devops/refs/heads/main/web_infrastructure_design/images/2-secured_and_monitored_web_infrastructure.png" width="50%"/>
</p>

## Specifics about this infrastructure

#### Why Add 3 Firewalls, 1 SSL Certificate, and 3 Monitoring Clients?
Implementing multiple firewalls at different layers provides a defense-in-depth security strategy.
- **Network Firewall:** Typically placed at the entry point of your network infrastructure, it controls traffic based on source and destination IP addresses, ports, and protocols. This acts as the first line of defense against broad internet-based attacks.
- **Host-Based Firewalls:** Installed on each individual server, these firewalls provide granular control over network traffic at the operating system level. They can restrict which applications can accept incoming connections and which outgoing connections are allowed. This adds a layer of security even if the network firewall is compromised or has overly permissive rules. Having firewalls on both servers is crucial for isolating them from each other in case one is compromised.

An SSL (Secure Sockets Layer) certificate enables HTTPS (HTTP Secure). This is essential for:
- **Encryption:** It encrypts the communication between the user's browser and the web server, protecting sensitive data (like login credentials, personal information, etc.) from being intercepted by third parties.
- **Authentication:** It verifies the identity of the website to users, assuring them that they are connecting to the legitimate www.foobar.com and not a malicious imposter. This builds trust and confidence.

The monitoring clients are crucial for gaining insights into the performance and the infrastructure's health:
- **Proactive Issue Detection:** They collect metrics that allow us to identify potential problems (e.g., high CPU usage, low memory, slow database queries) before they lead to outages or performance degradation.
- **Performance Optimization:** The collected data helps to understand how the application and infrastructure are performing under load, enabling us to identify bottlenecks and optimize resource allocation for better efficiency and user experience.
- **Troubleshooting:** When issues do arise, monitoring data provides valuable context and helps us pinpoint the root cause quickly, reducing downtime. Having clients on both servers and potentially the load balancer ensures comprehensive visibility across our key components.

#### What Are Firewalls For?

Firewalls are network security devices that control incoming and outgoing network traffic based on a set of predefined rules. They act as a barrier between a trusted internal network and an untrusted external network (like the internet). Their primary purposes include:
- **Preventing Unauthorized Access:** Firewalls block malicious traffic and attempts to access the servers or network resources by unauthorized individuals or systems.
- **Controlling Network Traffic:** They enforce policies that dictate which types of traffic are allowed or denied based on source and destination IP addresses, ports, and protocols.
- **Protecting Against Attacks:** Firewalls can help mitigate various types of network-based attacks, such as port scanning, denial-of-service (DoS) attacks, and exploitation of known vulnerabilities.
- **Enforcing Security Policies:** They allow organizations to implement and enforce their security policies regarding network connectivity.

#### Why Is the Traffic Served Over HTTPS?

Traffic is served over HTTPS for the following critical reasons:
- **Security (Encryption):** HTTPS encrypts the communication between the user's browser and the web server using protocols like TLS (Transport Layer Security). This prevents eavesdropping and ensures that sensitive data transmitted between the client and the server remains confidential.
- **Integrity:** HTTPS helps ensure the integrity of the data being transmitted. It prevents attackers from tampering with the data in transit. The browser can verify that the data received from the server has not been altered.
- **Authentication:** The SSL/TLS certificate used by HTTPS verifies the identity of the website. This helps users confirm that they are interacting with the legitimate website and not a phishing site.
- **SEO Benefits:** Search engines like Google favor websites that use HTTPS, potentially giving them a slight ranking boost.
- **User Trust:** The "HTTPS" padlock icon in the browser address bar signals to users that their connection is secure, building trust and encouraging them to interact with the website.

#### What Monitoring Is Used For?

Monitoring is used to continuously observe and track the performance, health, and availability of our IT infrastructure and applications. Its key purposes include:
- **Availability Monitoring:** Ensuring that our services and applications are up and running and accessible to users.
- **Performance Monitoring:** Tracking key performance indicators (KPIs) like CPU utilization, memory usage, network latency, disk I/O, and application response times to identify bottlenecks and performance issues.
- **Error Detection:** Identifying and alerting on errors, exceptions, and failures within our applications and infrastructure.
- **Capacity Planning:** Analyzing historical performance data to predict future resource needs and plan for scaling.
- **Security Monitoring:** Detecting suspicious activity and potential security breaches.
- **Troubleshooting and Diagnostics:** Providing data and context to help diagnose and resolve issues quickly when they occur.
- **Service Level Agreement (SLA) Compliance:** Tracking performance against defined SLAs to ensure that service quality is maintained.

#### How the Monitoring Tool Is Collecting Data

Monitoring tools like those integrated with Sumologic collect data through various methods, depending on the type of resource being monitored:

**Agents/Clients**

These are software programs installed on the servers or applications being monitored. They collect metrics specific to the operating system, hardware, applications (like Nginx, MySQL), and custom application logs. These agents then send the collected data to the central monitoring platform (e.g., Sumologic).

**Protocols**

Monitoring tools often leverage standard protocols to gather data. For example:
- **SNMP (Simple Network Management Protocol):** Used to collect information from network devices (like routers, switches) and sometimes servers.
- **Syslog:** A standard protocol for event logging, used by many devices and applications to send log messages to a central server for analysis.
- **JMX (Java Management Extensions):** Used to monitor Java-based applications.
- **APM (Application Performance Monitoring) Agents:** Specialized agents that instrument application code to track requests, transactions, and performance metrics within the application itself.

**APIs (Application Programming Interfaces)**

Many services and applications expose APIs that monitoring tools can use to pull performance data and metrics. For example, cloud providers (like AWS, Azure, GCP) offer APIs for monitoring their services.
- **Log File Analysis:** Monitoring tools can collect and analyze log files generated by operating systems, web servers, application servers, and databases to identify errors, warnings, and performance-related events.

#### How to Monitor Your Web Server QPS (Queries Per Second)

To monitor the web server's QPS (Queries Per Second) using your monitoring tool, we would typically follow these steps:

**Identify the Metric**

The key metric you need to track is the number of HTTP requests the Nginx web server is handling per second.

**Configure Data Collection**

- **Nginx Status Module:** Nginx has a built-in status module (usually accessed via a specific URL, e.g., /nginx_status) that provides various metrics, including the number of active connections, total requests handled, and requests per second. We would need to ensure this module is enabled in your Nginx configuration.
- **Monitoring Agent Configuration:** We would then configure our monitoring agent (running on the server where Nginx is installed) to collect data from this Nginx status endpoint. This might involve specifying the URL to scrape and the specific metrics to extract.
- **Log Analysis:** Alternatively, we could configure our monitoring tool to analyze our Nginx access logs. By counting the number of log entries within a one-second window, we can derive the QPS. Our monitoring tool likely has features to perform this kind of log aggregation and analysis.

**Visualize and Alert**

- **Dashboard Creation:** Once the data is being collected, we would create a dashboard in your monitoring tool to visualize the QPS over time. This allows us to see trends and identify any sudden drops or spikes in traffic.
- **Alerting:** We can set up alerts based on QPS thresholds. For example, we might want to be alerted if the QPS drops below a certain level (indicating a potential issue) or spikes above a certain level (indicating a potential overload).

**Analyze and Troubleshoot**

Use the QPS data in conjunction with other metrics (CPU usage, memory, latency, error rates) to understand the performance of the web server and troubleshoot any issues.

## Issues with this Infrastructure

**Why Terminating SSL at the Load Balancer Level Is an Issue (Potential Issue, Depends on Context)**

While terminating SSL at the load balancer is a common and often recommended practice, it can introduce a potential issue if the communication between the load balancer and the backend servers are not also encrypted.
- **Lack of End-to-End Encryption:** If the traffic from the load balancer to the web servers is over plain HTTP, the data is unencrypted within your internal network. If an attacker were to gain access to this internal network, they could potentially eavesdrop on sensitive data after it has been decrypted by the load balancer.
- **Mitigation:** To address this, we should implement TLS encryption between the load balancer and the backend servers as well (often referred to as "re-encryption"). This ensures end-to-end encryption of the data from the user's browser all the way to the application server.

**Why Having Only One MySQL Server Capable of Accepting Writes Is an Issue (Single Point of Failure for Writes)**

Even though we likely have the MySQL server on both of our application servers, if only one is configured to accept write operations (the primary/master), this introduces a single point of failure for write availability.
- **Write Downtime:** If the server hosting the primary MySQL instance fails, our application will be unable to perform any write operations (e.g., create new accounts, place orders, update data) until a replica is promoted to become the new primary. This failover process can take time and might involve manual intervention, leading to downtime and data inconsistencies if not handled correctly.
- **Mitigation:** To improve write availability, we would need to implement a more robust database clustering solution that supports automatic failover for write operations. This could involve technologies like MySQL Group Replication or other distributed database systems.

**Why Having Servers with All the Same Components (Database, Web Server, and Application Server) Might Be a Problem (Resource Contention and Scalability)**

While this setup is simpler to manage initially, it can lead to several issues as our application grows:
- **Resource Contention:** Each of these components (web server, application server, database) has different resource requirements (CPU, memory, I/O). Running them all on the same server can lead to resource contention, where one component might starve the others for resources, impacting overall performance. For example, a spike in database activity could slow down our web server and application.
- **Scalability Challenges:** Scaling individual components becomes difficult. If our web traffic increases significantly, we might need to scale out our web servers. However, because the database and application server are on the same machine, we would have to scale all three together, even if the other components don't require it. This can be inefficient and costly.
- **Security Isolation:** Running all components on the same server can increase the attack surface. If one component is compromised, an attacker might be able to gain access to the other components more easily. Isolating them on separate servers (or using containers) can improve security.
- **Deployment Complexity:** Deploying updates or changes to one component might require downtime for the entire server, affecting all services running on it. Separating the components allows for more granular and less disruptive deployments.
- **Mitigation:** As our application scales, it's generally recommended to decouple these components and run them on separate, appropriately sized servers or containers. This allows for independent scaling, better resource management, improved security, and more flexible deployments. For example, we could have dedicated servers or clusters for our web servers, application servers, and database.
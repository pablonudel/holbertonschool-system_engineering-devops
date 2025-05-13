# Distributed web infrastructure

<p align="center">
  <img src="https://raw.githubusercontent.com/pablonudel/holbertonschool-system_engineering-devops/refs/heads/main/web_infrastructure_design/images/1-distributed_web_infrastructure.png" width="50%"/>
</p>

## Specifics about this infrastructure
#### Why a Load Balancer (HAproxy) and at Least 2 Servers?

If we only have a single server, and lots of users come in at once, that server will quickly become overwhelmed. Users will experience slow service or might not even get served at all. With at least two servers, A load balancer (like HAproxy) intelligently directs incoming users to the available server. This setup provides several key benefits:

- **High Availability (HA):** If one of our servers fails for any reason (hardware issue, software glitch, etc.), the load balancer will automatically stop sending traffic to that server and direct it to the remaining healthy servers. This ensures that our website stays online and accessible to users, hence the "HA" in HAproxy.
- **Scalability:** As our website traffic grows, we can easily add more servers behind the load balancer to handle the increased load. The load balancer will distribute the traffic across all available servers, preventing any single server from becoming overloaded.
- **Improved Performance:** By distributing traffic across multiple servers, each server handles a smaller portion of the overall load. This can lead to faster response times and a better user experience.

#### Load Balancer Distribution Algorithm and How It Works

Common and effective distribution algorithms include:

- **Round Robin:** This is a simple and widely used algorithm. It distributes incoming requests to each server in a sequential order.
- **Least Connections:** This algorithm directs new requests to the server with the fewest active connections at that moment. This can be more efficient than round robin when servers have varying processing capacities or when requests have different processing times.
- **Least Response Time:** This algorithm sends requests to the server with the lowest average response time. It aims to send users to the servers that are responding the fastest.
- **Source IP Hash:** This algorithm uses the client's IP address to determine which server they are consistently directed to. This can be useful for maintaining session stickiness (ensuring a user's requests during a session are handled by the same server).

The load balancer works by acting as a single point of entry for all incoming traffic. It listens on a specific IP address and port. When a client (a user's browser) sends a request to our website, the request first reaches the load balancer. The load balancer then applies its configured distribution algorithm to select one of the backend servers and forwards the request to that server. The chosen server processes the request and sends the response back to the load balancer, which then relays the response back to the client.

#### Active-Active vs. Active-Passive Load Balancer Setup

A load balancer setup can be either **Active-Active** or **Active-Passive**:
- **Active-Active:** In this setup, we have multiple load balancer instances running simultaneously and actively handling traffic. All load balancers are live and distributing requests. If one load balancer fails, the others can continue to handle the entire load. This provides high redundancy and better performance as the load is spread across multiple load balancers.
- **Active-Passive:** In this setup, we have one primary (active) load balancer handling all the traffic, and one or more secondary (passive) load balancers standing by. The passive load balancers are only activated if the primary load balancer fails. While this provides redundancy, during normal operation, only one load balancer is utilized. Failover to the passive instance might take a short time, leading to a brief interruption in service.

#### Difference Between Primary and Replica Nodes for the Application

- **Primary Node:** The application typically connects to the primary database for both reading and writing data. The primary node is the source of truth and ensures data consistency for write operations.
- **Replica Nodes:** The application can be configured to connect to the replica nodes for read operations. This can significantly improve the application's performance and scalability by distributing the read load. However, applications need to be aware that there might be a slight delay (replication lag) between the data on the primary and the replicas. For critical read-after-write operations, the application might need to be directed to the primary to ensure it reads the most up-to-date data.

## Issues with this Infrastructure

#### Single Points of Failure (SPOF):
- **The Load Balancer (if only one instance):** If we only have a single HAproxy instance, it becomes a SPOF. If this server fails, your entire web application becomes inaccessible. Implementing a second load balancer in an Active-Active or Active-Passive configuration resolves this.
- **The Primary Database Server:** While we have replicas, if the primary database server fails and we don't have automated failover in place, write operations will be impossible, and we'll need to manually promote a replica to become the new primary. This downtime is a SPOF for write availability.

#### Security Issues:
- **No Firewall:** A firewall acts as a security guard for our servers, controlling incoming and outgoing network traffic based on predefined rules. Without a firewall, our servers are directly exposed to the internet and are vulnerable to various attacks, such as unauthorized access, denial-of-service attacks, and exploitation of software vulnerabilities.
- **No HTTPS:** HTTPS (HTTP Secure) encrypts the communication between the user's browser and our web server. Without HTTPS, all data transmitted, including sensitive information like login credentials, personal details, and payment information, is sent in plain text and can be intercepted by malicious actors. This is a significant security and privacy risk.

#### No Monitoring:

Without monitoring, we lack visibility into the health and performance of our infrastructure. We won't be alerted to potential problems like high CPU usage, low disk space, slow database queries, or application errors until they cause a significant outage or impact user experience. Proactive monitoring allows us to identify and address issues before they escalate.
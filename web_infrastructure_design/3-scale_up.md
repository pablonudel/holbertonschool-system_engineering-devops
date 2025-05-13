# Scale up

<p align="center">
  <img src="https://raw.githubusercontent.com/pablonudel/holbertonschool-system_engineering-devops/refs/heads/main/web_infrastructure_design/images/3-scale_up.png" width="50%"/>
</p>

## Specifics about this infrastructure

#### Why Add a Clustered Load Balancer?

- **High Availability for Load Balancing:** By clustering the two HAproxy instances, we achieve redundancy at the load balancer level itself. If one of the load balancers fails (due to hardware issues, network problems, etc.), the other load balancer in the cluster will automatically take over and continue to distribute traffic to our backend servers without any interruption in service for our users. This eliminates the load balancer as a SPOF.
- **Improved Reliability:** Clustering often involves mechanisms for state synchronization between the load balancers. This ensures that session information and other critical data are shared, so if a failover occurs, user sessions are maintained, and they don't experience a loss of continuity.
- **Increased Performance and Scalability (Potentially):** Depending on the clustering configuration, we might also see improved performance and scalability. For instance, both load balancers can actively distribute traffic in an Active-Active setup, effectively doubling the capacity to handle incoming requests.

#### Why Split Components onto Separate Servers?
Moving to three dedicated servers – one for the web server (Nginx), one for the application server, and one for the database (MySQL) – provides significant advantages over having all components on the same servers:
- **Improved Resource Utilization:** Each component has different resource requirements. By separating them, we can allocate resources (CPU, RAM, disk I/O) more efficiently to each service based on its specific needs. The web server, application server, and database can scale independently without impacting each other due to resource contention on the same machine.
- **Enhanced Scalability:** When traffic to our website increases, we can scale out our web server layer by adding more web server instances behind the load balancers without needing to scale the application or database servers unnecessarily. Similarly, if our application becomes more resource-intensive, we can scale the application server independently. If our data grows or query load increases, we can focus on scaling the database server. This granular scalability is much more cost-effective and efficient.
- **Increased Security:** Separating components can improve security by isolating them. If one server is compromised, it's less likely to provide a direct pathway to compromise other critical components like the database. We can also apply more specific security policies and hardening measures to each server based on its role.
- **Simplified Management and Deployment:** Managing and deploying updates to each component becomes easier and less risky. For example, we can update our application code on the application server without needing to take down the web server or database. This reduces downtime and simplifies maintenance.
- **Better Performance and Stability:** Separating components reduces the likelihood of one service impacting the performance or stability of another due to resource exhaustion or configuration conflicts. This leads to a more stable and responsive application.
- **Technology Specialization:** We can choose the most appropriate hardware and operating system configurations for each role. For example, the database server might benefit from fast, high-capacity storage, while the web server might need more network bandwidth.
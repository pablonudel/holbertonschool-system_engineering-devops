
# Simple Web Stack

This is the trajectory of the requests and the corresponding server-side processes if a user want to visit `www.foobar.com`:

- The user types `www.foobar.com` into the browser address bar.
- Their computer sends a request to a **DNS (Domain Name System) server** to find the IP address associated with `www.foobar.com`.
- The DNS server looks up the record for `www.foobar.com` that is configured this with a **CNAME record** (Canonical Name record) pointing `www` to `foobar.com`, which in turn has an A record pointing to your server's IP address: `8.8.8.8`.
- The user's browser sends an **HTTP (Hypertext Transfer Protocol)** request to the IP address `8.8.8.8` on the standard web port 80 (or 443 for HTTPS).

## The One-Server Web Infrastructure (LAMP Stack)

<p align="center">
  <img src="https://raw.githubusercontent.com/pablonudel/holbertonschool-system_engineering-devops/refs/heads/main/web_infrastructure_design/images/0-simple_web_stack.png" width="50%"/>
</p>

## Specifics about this infrastructure
#### What is a server:
A server is a physical or virtual computer dedicated to running software and providing services to other computers (clients). The single server here is responsible for hosting the website, handling requests, processing application logic, and storing data.  

#### What is the role of the domain name:
The domain name (`foobar.com`) provides a human-friendly and memorable address for our website instead of users having to remember the numerical IP address (`8.8.8.8`). The DNS system translates this domain name into the actual IP address that computers use to communicate.  

#### What type of DNS record `www` is in `www.foobar.com`:
`www` is configured as a **CNAME (Canonical Name) record** pointing to the base domain `foobar.com`. The `foobar.com` domain itself would have a record pointing to the server's IP address (`8.8.8.8`).

#### What is the role of the web server (Nginx):
The web server (Nginx) acts as the entry point for all incoming HTTP requests from users' browsers. Its primary responsibilities include:
- Receiving and processing HTTP requests.
- Serving static content like HTML files, CSS stylesheets, JavaScript files, images, etc., directly to the user's browser.
- Forwarding dynamic requests (e.g., requests for PHP scripts) to the application server for processing.
- Handling security aspects like SSL/TLS encryption (if configured for HTTPS).  

#### What is the role of the application server:
The application server is responsible for executing the dynamic code of your website. For example, in a website built with PHP, the application server (like PHP-FPM) will interpret the PHP scripts, interact with the database if needed, and generate dynamic HTML content. This generated content is then passed back to the web server, which sends it to the user's browser.  

#### What is the role of the database (MySQL):
The database (MySQL) is used to store and manage the persistent data of the website. This include all the data that needs to be stored and retrieved. The application server interacts with the database to fetch, store, update, and delete this data as required by the website's logic.  

#### What is the server using to communicate with the computer of the user requesting the website:
The server communicates with the user's computer using the TCP/IP (Transmission Control Protocol/Internet Protocol) suite. Specifically, for web traffic, it primarily uses HTTP (or HTTPS for secure connections) running over TCP. The server listens for incoming connections on specific ports (port 80 for HTTP and port 443 for HTTPS) and responds to the requests it receives.

## Issues with this Infrastructure

#### Single Point of Failure (SPOF):
If this single server fails (due to hardware issues, software crashes, network problems, etc.), the entire website becomes unavailable.

#### Downtime when maintenance needed:
Any maintenance task that requires restarting services on the server (such as deploying new code to the web server or application server, upgrading software, or performing system updates) will result in downtime for the website. Users will not be able to access it during these periods.  

#### Cannot scale if too much incoming traffic:
This infrastructure has limited scalability. If the website experiences a sudden surge in traffic, the single server might become overloaded. It might run out of resources (CPU, RAM, network bandwidth), leading to slow performance or even crashes.
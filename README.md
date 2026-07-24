## Porject Overview
Production-Ready High Availability Web Infrastructure with Nginx

Built a highly available web architecture by configuring Nginx as a reverse proxy and load balancer to distribute incoming traffic across three Apache application servers.

As website traffic grows, relying on a single web server can lead to slow response times and downtime. In this project, I implemented an Nginx Load Balancer that distributes client requests across three Apache web servers, creating a scalable and highly available web infrastructure.

During implementation, I encountered a 502 Bad Gateway issue caused by an incorrect backend port configuration. After identifying the actual Apache listening port and updating the Nginx upstream configuration, the application became accessible through the load balancer.

## Architecture

                     Client
                        │
                        ▼
            +---------------------+
            |  Nginx Load Balancer|
            |      (stlb01)       |
            +----------+----------+
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
 +-------------+ +-------------+ +-------------+
 |   stapp01   | |   stapp02   | |   stapp03   |
 | Apache HTTPD| | Apache HTTPD| | Apache HTTPD|
 +-------------+ +-------------+ +-------------+

## Tech Stacks

| Technology   | Purpose                       |
| ------------ | ----------------------------- |
| Linux        | Operating System              |
| Nginx        | Reverse Proxy & Load Balancer |
| Apache HTTPD | Backend Web Server            |
| SSH          | Remote Administration         |
| HTTP         | Web Protocol                  |

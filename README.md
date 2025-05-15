# Web Infrastructure Design - Holberton School

This repository contains the solutions to the four progressive exercises on web infrastructure design, illustrating from a simple single-server stack to a scalable, secured, and monitored multi-server architecture.

## Exercises

### 0. Simple Web Stack
**Goal**: Design a basic single-server web infrastructure hosting www.foobar.com.

**Architecture**:
1. Server with:

- Web server (Nginx)

- Application server

- Application files (code base)

- Database (MySQL)

- Domain foobar.com with www DNS A record pointing to server IP 8.8.8.8


**Key Concepts**:
- **Server**: Physical or virtual machine running services
- **Domain name**: Human-friendly website address resolving to IP
- **DNS record**: www is an A record linking www.foobar.com to 8.8.8.8
- **Web server (Nginx)**: Handles HTTP requests, serves static content, proxies dynamic requests to app server
- **Application server**: Executes backend business logic
- **Database (MySQL)**: Stores persistent data
- **Communication**: HTTP/HTTPS protocols over TCP/IP enable user-browser to server communication

**Limitations**:
- Single Point of Failure (SPOF)
- Downtime during maintenance or deployments
- No horizontal scaling to handle high traffic

---

### 1. Distributed Web Infrastructure
**Goal**: Design a multi-server infrastructure hosting www.foobar.com.

**Architecture**:
1. Load balancer (HAProxy)
2. Servers, each with:

- Web server (Nginx)

- Application server

- Application files

- Database (MySQL)


**Additions & Explanations**:
- **Load Balancer**: Distributes traffic across servers to improve availability
- **Distribution Algorithm**: Usually round-robin or least connections
- **Active-Active vs Active-Passive**:
  - Active-Active: All load balancers serve traffic simultaneously
  - Active-Passive: One active, one standby for failover
- **Database Cluster**: Primary-Replica setup where Primary handles writes, Replicas handle reads
- **Primary vs Replica Node**: Primary accepts writes; Replicas serve read queries to reduce load

**Limitations**:
- SPOF at load balancer if single node
- No firewall or HTTPS leads to security risks
- Lack of monitoring to detect failures or performance issues

---

### 2. Secured and Monitored Web Infrastructure
**Goal**: Enhance the 3-server infrastructure with security and monitoring.

**Additions**:
- 3 Firewalls protecting each server
- SSL Certificate for www.foobar.com enabling HTTPS
- 3 Monitoring clients collecting data (e.g., Sumo Logic)

**Explanations**:
- **Firewalls**: Control incoming/outgoing traffic to prevent unauthorized access
- **HTTPS**: Encrypts communication between users and servers
- **Monitoring**: Tracks server health, performance, and traffic metrics
- **Data Collection**: Monitoring clients collect logs and metrics sent to centralized service
- **QPS Monitoring**: Analyze Query Per Second (QPS) via logs or application metrics

**Remaining Issues**:
- SSL termination at load balancer complicates backend encryption
- Single MySQL node accepting writes is a SPOF
- Mixing database, web, and app servers on same machines can reduce isolation and scalability

---

### 3. Scale Up
**Goal**: Design a scalable infrastructure with component separation and load balancer clustering.

**Architecture**:
1 Load balancer cluster (HAProxy) for high availability
Dedicated servers for:

- Web server

- Application server

- Database server


**Reasons for Additions**:
- Load balancer cluster: Avoids SPOF on load balancing
- Component separation: Improves scalability and resource isolation
- Clustering: Ensures continuous availability during node failures

---

## Summary Table

| Exercise | Components Added | Key Benefits | Remaining Issues |
|----------|------------------|--------------|------------------|
| 0 | Single server | Simplicity, easy to setup | SPOF, no scaling, downtime |
| 1 | Load balancer + DB replication | Availability, load distribution | SPOF at LB, no security, no monitoring |
| 2 | Firewalls, SSL, monitoring | Security, encryption, observability | SSL termination issues, DB SPOF, mixed roles |
| 3 | LB clustering, component split | Scalability, high availability | Complexity, requires management |

---

## How to Run

Each design is conceptual and can be implemented using tools such as:

- **Web server**: Nginx
- **Load balancing**: HAProxy
- **Databases**: MySQL
- **SSL certificates**: Let's Encrypt
- **Firewall tools**: iptables, ufw
- **Monitoring tools**: Prometheus, Grafana, Sumo Logic
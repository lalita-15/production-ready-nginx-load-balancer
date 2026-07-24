# Troubleshooting Guide

This document describes the issues encountered while configuring the Nginx Load Balancer and the steps taken to resolve them.

---

# Issue 1: 502 Bad Gateway

## Error

While accessing the Load Balancer:

```bash
curl http://stlb01
```

Received:

```text
502 Bad Gateway
```

---

## Root Cause

Nginx was forwarding requests to the wrong backend port.

The upstream configuration was pointing to:

```nginx
server stapp01:8085;
server stapp02:8085;
server stapp03:8085;
```

However, Apache HTTPD was actually listening on a different port.

---

## Investigation

Verified the Apache listening port.

```bash
grep "^Listen" /etc/httpd/conf/httpd.conf
```

Output:

```text
Listen 6200
```

This confirmed that the backend servers were not listening on port **8085**.

---

## Resolution

Updated the upstream configuration.

```nginx
upstream backend {

    server stapp01:6200;
    server stapp02:6200;
    server stapp03:6200;

}
```

Validated configuration.

```bash
nginx -t
```

Restarted Nginx.

```bash
systemctl restart nginx
```

---

# Issue 2: Connection Refused

## Error

```bash
curl http://stapp01:8085
```

Output

```text
curl: (7) Failed to connect to stapp01 port 8085: Connection refused
```

---

## Root Cause

Apache was not listening on port **8085**.

---

## Investigation

Checked listening ports.

```bash
ss -tlnp
```

Verified Apache configuration.

```bash
grep "^Listen" /etc/httpd/conf/httpd.conf
```

---

## Resolution

Configured the Nginx upstream to use the correct Apache listening port.

---

# Issue 3: Validate Nginx Configuration

Before restarting Nginx, configuration syntax was validated.

```bash
nginx -t
```

Expected Output

```text
syntax is ok
test is successful
```

---

# Issue 4: Verify Backend Connectivity

Verified that all backend servers were reachable.

```bash
curl http://stapp01:6200
curl http://stapp02:6200
curl http://stapp03:6200
```

---

# Issue 5: Verify Load Balancer

Confirmed that the application was accessible through the Load Balancer.

```bash
curl http://stlb01
```

---

# Lessons Learned

- Always verify the backend application port before configuring a reverse proxy.
- Validate the Nginx configuration using `nginx -t` before restarting the service.
- A **502 Bad Gateway** usually indicates that Nginx cannot communicate with the backend application.
- Use `curl` to test backend connectivity before troubleshooting Nginx.
- Check Apache configuration and listening ports before assuming network issues.

---

# Troubleshooting Workflow

```text
Client Request
      │
      ▼
502 Bad Gateway
      │
      ▼
Verify Nginx Configuration
      │
      ▼
Check Backend Connectivity
      │
      ▼
Verify Apache Listening Port
      │
      ▼
Update Upstream Configuration
      │
      ▼
Validate Nginx
      │
      ▼
Restart Nginx
      │
      ▼
Website Accessible
```

---

# Outcome

- Successfully resolved **502 Bad Gateway**.
- Configured Nginx to use the correct backend port.
- Restored communication between the Load Balancer and all backend servers.
- Built a production-ready high availability web infrastructure using Nginx and Apache.
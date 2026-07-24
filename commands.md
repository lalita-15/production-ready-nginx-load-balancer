# Commands Used
Important Linux and Nginx commands used while configuring the High Availability Web Infrastructure.

---

## 1. Verify Nginx Installation

```bash
which nginx
```

**Purpose:** Check whether Nginx is installed on the Load Balancer server.

---

## 2. Install Nginx (If Required)

```bash
sudo yum install nginx -y
```

**Purpose:** Install the Nginx web server.

---

## 3. Verify Apache HTTP Server Status

```bash
systemctl status httpd
```

**Purpose:** Ensure Apache HTTPD service is running on all application servers.

---

## 4. Check Apache Listening Port

```bash
grep "^Listen" /etc/httpd/conf/httpd.conf
```

**Purpose:** Identify the port on which Apache is listening.

Example Output:

```text
Listen 6200
```

---

## 5. Verify Open Listening Ports

```bash
ss -tlnp
```

or

```bash
ss -tlnp | grep httpd
```

**Purpose:** Confirm that Apache is listening on the expected port.

---

## 6. Edit Nginx Configuration

```bash
vi /etc/nginx/nginx.conf
```

**Purpose:** Configure the upstream backend servers and reverse proxy.

---

## 7. Configure Backend Servers

```nginx
upstream backend {
    server stapp01:6200;
    server stapp02:6200;
    server stapp03:6200;
}
```

**Purpose:** Define all backend Apache servers.

---

## 8. Configure Reverse Proxy

```nginx
location / {
    proxy_pass http://backend;
}
```

**Purpose:** Forward incoming client requests to backend servers.

---

## 9. Validate Nginx Configuration

```bash
nginx -t
```

**Purpose:** Verify configuration syntax before restarting Nginx.

Expected Output:

```text
syntax is ok
test is successful
```

---

## 10. Restart Nginx

```bash
systemctl restart nginx
```

**Purpose:** Apply configuration changes.

---

## 11. Test Backend Connectivity

```bash
curl http://stapp01:6200
```

```bash
curl http://stapp02:6200
```

```bash
curl http://stapp03:6200
```

**Purpose:** Verify that the Load Balancer can reach all backend servers.

---

## 12. Test Load Balancer

```bash
curl http://stlb01:80
```

**Purpose:** Confirm that the application is accessible through the Nginx Load Balancer.


---

## 14. Verify Nginx Process

```bash
systemctl status nginx
```

**Purpose:** Confirm Nginx service is running successfully.

---

## Summary

In this project I used Linux administration and Nginx commands to:

- Install and verify Nginx
- Configure reverse proxy
- Configure upstream backend servers
- Validate Nginx configuration
- Restart services
- Verify Apache listening ports
- Troubleshoot a **502 Bad Gateway** error
- Successfully distribute traffic across three backend servers
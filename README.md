# SafeLine WAF + DVWA Web Application Security Lab

## Overview

This project demonstrates the deployment and testing of a Web Application Firewall (WAF) in a simulated attacker-versus-defender environment using:

* **SafeLine WAF** as the defensive reverse proxy
* **DVWA (Damn Vulnerable Web Application)** as the intentionally vulnerable web application
* **Kali Linux** as the attacking machine
* **Ubuntu Server** hosting DVWA and SafeLine
* **HTTPS/TLS** using self-signed certificates

The objective of this lab was to gain hands-on experience with:

* Web application security
* Reverse proxy and WAF deployment
* SQL Injection (SQLi) testing
* Attack detection and blocking
* Rate limiting and HTTP flood protection
* WAF access control and IP blocking
* HTTPS/TLS configuration
* Security logging and event analysis

---

# Lab Architecture

```text
Kali Linux (Attacker)
        ↓
https://dvwa.local
        ↓
SafeLine WAF (HTTPS Reverse Proxy)
        ↓
DVWA Application (Apache/PHP/MySQL)
        ↓
Ubuntu Server
```

---

# Technologies Used

| Technology    | Purpose                                      |
| ------------- | -------------------------------------------- |
| SafeLine WAF  | Web Application Firewall / Reverse Proxy     |
| DVWA          | Vulnerable web application for testing       |
| Kali Linux    | Attacker VM and penetration testing platform |
| Ubuntu Server | Backend hosting environment                  |
| Apache2       | Web server                                   |
| MySQL         | Database backend                             |
| PHP           | DVWA runtime                                 |
| OpenSSL       | TLS certificate generation                   |
| VirtualBox    | Virtualized lab environment                  |

---

# Environment Setup

## Ubuntu Server

The Ubuntu VM hosted:

* DVWA
* Apache2
* MySQL
* SafeLine WAF

DVWA was configured to run behind SafeLine using a reverse proxy architecture.

The Apache virtual host was configured to serve DVWA directly from:

```text
/var/www/html/DVWA
```

---

# HTTPS/TLS Configuration

A self-signed certificate was generated using OpenSSL:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/ssl/dvwa/dvwa.key \
-out /etc/ssl/dvwa/dvwa.crt
```

The certificate and key were uploaded into SafeLine WAF so the application could be served securely over HTTPS.

The application became accessible through:

```text
https://dvwa.local
```

---

# DNS Resolution

Local DNS resolution was configured through the `/etc/hosts` file on the Kali Linux VM:

```text
192.168.1.103 dvwa.local
```

This allowed the attacker machine to resolve the lab domain locally.

---

# SafeLine WAF Configuration

The DVWA application was onboarded into SafeLine using:

| Setting       | Value                                                  |
| ------------- | ------------------------------------------------------ |
| Domain        | dvwa.local                                             |
| Frontend Port | 443/HTTPS                                              |
| Backend URL   | [http://192.168.1.103:8080](http://192.168.1.103:8080) |
| Access Method | Reverse Proxy                                          |

The WAF was configured to:

* Inspect incoming HTTP/HTTPS traffic
* Detect malicious payloads
* Block SQL injection attempts
* Apply request rate limiting
* Enforce custom deny rules
* Provide centralized attack logging

---

# SQL Injection Testing

DVWA was configured with the security level set to:

```text
Low
```

The SQL Injection module was used to test malicious payloads such as:

```sql
1' OR '1'='1
```

and:

```sql
1' OR 1=1#
```

Testing was conducted both:

1. Directly against the backend application
2. Through the SafeLine WAF reverse proxy

---

# WAF Detection and Blocking

SafeLine WAF successfully:

* Detected SQL injection payloads
* Logged malicious requests
* Generated attack events
* Returned HTTP block responses

Attack activity was reviewed through the SafeLine management console under:

* Security Events
* Protection Logs
* Attack Monitoring

Captured logs included:

* Source IP address
* Request URI
* Detection rule triggered
* Timestamp
* Attack type
* Block status

---

# HTTP Flood Protection

SafeLine HTTP flood protections were configured by:

* Defining request-per-second thresholds
* Configuring temporary blocking durations
* Applying automated mitigation rules

This simulated basic DDoS and brute-force mitigation strategies.

---

# Access Control and IP Blocking

Custom deny rules were created within SafeLine to:

* Block requests originating from the Kali Linux attacker VM
* Validate policy enforcement functionality
* Simulate network-layer traffic filtering

Blocked responses were verified successfully.

---

# Lessons Learned

This project provided hands-on experience with:

* Reverse proxy security architecture
* WAF deployment and policy management
* HTTPS/TLS implementation
* SQL injection attack behavior
* Security event analysis
* Layered web application defenses
* Attack surface reduction
* Troubleshooting Apache, PHP, MySQL, and proxy configurations

The lab also reinforced the importance of:

* Proper backend application isolation
* Secure traffic inspection
* Authentication and access controls
* Security monitoring and alert visibility

---

# Key Skills Demonstrated

* Web Application Security
* WAF Administration
* Reverse Proxy Configuration
* SQL Injection Testing
* Kali Linux
* Apache2 Administration
* MySQL Administration
* HTTPS/TLS Configuration
* Linux System Administration
* Security Monitoring and Logging
* Attack Detection and Mitigation

---

# Screenshots

> Add screenshots to the repository here.

Suggested screenshots:

1. SafeLine dashboard
2. SafeLine application configuration
3. DVWA login page over HTTPS
4. Successful SQL injection attempts against backend
5. Blocked SQL injection attempts through WAF
6. SafeLine attack logs
7. HTTP flood/rate limiting configuration
8. IP blocking policy

---

# Disclaimer

DVWA is intentionally vulnerable and was deployed strictly inside an isolated virtual lab environment for educational and defensive security testing purposes.

This project was conducted in a legal and controlled environment only.

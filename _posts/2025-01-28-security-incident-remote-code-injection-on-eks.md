---
layout: post
title:  "How We Secured a Kubernetes Cluster After a Remote Code Injection Incident"
date:   2025-01-28 05:48:12 +0500
categories: eks security cloudflare imds
author: Ali Mehdi
---

# How We Secured a Kubernetes Cluster After a Remote Code Injection Incident  

**A few days ago, we faced a remote code injection attack on a Kubernetes cluster. This post is about how we discovered the vulnerabilities, the steps we took to secure the setup (like implementing IMDSv2, removing privileged containers, and enforcing HTTPS), and the lessons we learned along the way. It’s a story of fixing mistakes, improving security, and growing stronger.**

---

## The Incident  

One of our clients was hit with a **remote code injection attack** on their Kubernetes cluster. The vulnerability stemmed from a `file_get_contents()` function in the code, which lacked proper checks for ensuring files were remote and used HTTPS. This allowed the attacker to exploit the function and attempt to access local files.  

### Security Gaps Identified  

1. **IMDSv1 enabled on nodes**: The attacker accessed instance metadata by replacing the URL with `http://169.254.169.254/latest/meta-data/`, exposing access IDs and secrets.  
2. **Containers running as root**: This created a major security loophole.  
3. **Exposed environment files**: The `.env` file revealed sensitive data.  
4. **Cloudflare traffic SSL set to "Flexible"**: Traffic between Cloudflare and the load balancer wasn’t fully encrypted.  

---

## What Worked in Our Favor  

Despite the gaps, some proactive measures helped us respond effectively:  

1. **AWS GuardDuty**: Alerted us to suspicious activity, enabling a quick response.  
2. **Logs stored on S3 with Athena**: Made it easy to query and trace the issue.  
3. **ECR container scanning**: Helped us identify vulnerabilities in our container images.  
4. **SonarQube integration**: Enabled developers to quickly patch code vulnerabilities.  
5. **Least privilege IAM policies**: Prevented the attacker from deleting or modifying data, even after gaining access.  

---

## The Fixes We Implemented  

After identifying the vulnerabilities, we implemented the following measures to **secure the environment**:  

1. **Non-root containers**: Updated containers to run as non-root users and removed escalated privileges.  
2. **IMDSv2 implementation**: Migrated nodes to IMDSv2, which uses session tokens and protects metadata from SSRF attacks. [Learn more about IMDSv2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html).  
3. **IAM Roles for Service Accounts (IRSA)**: Enabled granular access control for pods.  
4. **HSTS on Cloudflare**: Enforced HTTPS connections at the browser level.  
5. **Disabled `file_get_contents()`**: Replaced it with `cURL` and enforced HTTPS for fetching remote files.  
6. **Removed `.env` files**: Moved sensitive configuration to environment variables loaded at runtime.  
7. **Full encryption on AWS load balancer**: Ensured end-to-end encryption by integrating certificates on the load balancer and restricting traffic to Cloudflare only.  

---

## Key Takeaways  

1. **A chain is only as strong as its weakest link.** Continuous monitoring and proactive updates are essential to prevent vulnerabilities.  
2. **IMDSv2 is non-negotiable** in modern cloud environments. Its token-based approach significantly improves metadata security and is critical for protecting against SSRF or similar attacks.  
3. **Security isn’t just about infrastructure.** Code quality matters just as much. Even a minor oversight can expose the entire system.  
4. **Defense in depth works.** Layered tools like GuardDuty, ECR scanning, and IAM least privilege help contain potential damage.  
5. **You learn from mistakes and failures. Never give up.** Every challenge is an opportunity to improve and become more resilient.  

---

### Tags  

**#Kubernetes #DevOps #CloudSecurity #AWS #CyberSecurity #GuardDuty #ECR #Cloudflare #IMDSv2 #DevSecOps #SSRF #SecurityBestPractices #InfrastructureAsCode #Resilience #NeverGiveUp #LessonsLearned**  
# AWS Cloud Architecture: EFS, S3 Security, IAM & Web Hosting ☁️🔒

**Date:** 2026-07-03

**Category:** Cloud Computing / AWS
**Tags:** #AWS #EFS #S3 #IAM #Linux #Networking

Today I leveled up my AWS Cloud Architecture skills by focusing on enterprise networking, secure storage policies, and live web server deployment. 

## 1. Advanced Networking: EFS (Elastic File System) 
**The Goal:** Connect multiple independent web servers to a single, shared networked hard drive.

*   **The Concept (Stateless Architecture):** In enterprise environments, you never want your web servers storing user uploads (like profile pictures) on their own local hard drives (EBS). If the server crashes, the data is lost. Instead, you mount an **Amazon EFS** drive. EFS allows dozens of EC2 instances to read and write to the exact same file system simultaneously.
*   **Implementation:** Spun up two Amazon Linux instances (`web-server-01` and `web-server-02`) and mounted the EFS drive to a directory on both.
*   **Industry Pitfall (The NFS Hang):** Encountered a `mount.nfs4: No route to host` error when trying to connect to the EFS IP address. 
    *   *Why it happens:* AWS operates on a "Zero Trust" network model. 
    *   *The Fix:* EFS connections will permanently "hang" unless the Virtual Private Cloud (VPC) **Security Group** inbound rules explicitly allow **NFS traffic (Port 2049)** between the EC2 instances and the EFS network interface. 

## 2. Cloud Storage & JSON Security Policies (Amazon S3) 
**The Goal:** Create secure, infinitely scalable cloud storage and explicitly make specific files accessible over the public internet for web hosting.

*   **Bucket Creation & Uploads:** Created an S3 bucket (`demo-web-assets-bucket`) and uploaded a mix of web files (`index.html`) and source code (`structure.c`).
*   **The Security Principle:** By default, Amazon S3 absolutely blocks all public access to prevent data leaks. 
*   **Writing the Bucket Policy:** To use S3 to host website assets (like images or HTML), you must write a custom JSON Bucket Policy. This policy explicitly grants the `s3:GetObject` permission to the public (`*`).

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicRead",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::demo-web-assets-bucket/*"
        }
    ]
}
```

## 3. Identity and Access Management (IAM) 
**The Goal:** Secure the AWS environment by adhering to the Principle of Least Privilege, managing users and server permissions without hardcoding passwords.

- Granular Users: Created a dedicated IAM user (`app-developer`) instead of using the all-powerful Root AWS account.
- EC2 IAM Roles (The Best Practice): Attached a specific IAM Role (`ec2-s3-readonly-role`) directly to an EC2 instance (`i-0abcd1234efgh5678`).
    - The Industry Standard: Historically, developers would hardcode AWS access keys into their application code to let the server talk to S3 or DynamoDB. This is a massive security   vulnerability (if the code is pushed to GitHub, hackers steal the keys).
    - **The Solution:** By attaching an IAM Role, AWS automatically rotates secure, temporary credentials in the background. The server can talk to AWS services natively, and there are zero passwords stored in your code!

## 4. Linux Web Server Deployment (Apache) 
**The Goal:** Provision an empty Amazon Linux VM into a live, functioning web server strictly via the command-line interface (CLI).

    - Note on Amazon Linux: Amazon Linux is heavily based on Red Hat Enterprise Linux (RHEL), meaning it uses `yum` (or `dnf`) for package management, unlike Ubuntu which uses `apt`

The Deployment Steps:

1. Installation: Connect via SSH and install the Apache daemon (`httpd`).
```bash
$ sudo yum install httpd -y
```

2. Service Management: Start the web server immediately, and critically, enable it. (Enabling creates a symlink in `systemd` so that if the server crashes and reboots, Apache will automatically start itself back up).
```bash
$ sudo systemctl start httpd
$ sudo systemctl enable httpd
```

3. Web Configuration: Navigate to the default Apache directory (defined by the FHS) and create the main webpage.
```bash
$ sudo vim /var/www/html/index.html
```

4. Graceful Reload: Apply the changes without dropping active user connections.
```bash
$ sudo systemctl reload httpd
```

## 5. Real-World Debugging: Windows SSH Keys 
**The Error:** `WARNING: UNPROTECTED PRIVATE KEY FILE!`

- The Architecture Context: The SSH protocol is incredibly strict about cryptographic security. When connecting to an EC2 instance using a `.pem` file, that file acts as the absolute master key to your server.

- Why Windows Triggers This: On Linux, you simply run `chmod 400 production-key.pem` to restrict read access to only yourself. However, when you download a key onto a Windows machine, the NTFS file permissions are often inherited from the Downloads folder, making it too "open" (meaning other user accounts on the PC could theoretically copy your key). SSH intentionally aborts the connection to protect the server.

- **The Windows Fix:** You must manually replicate Linux's `400` permission in the Windows GUI. Right-click the `.pem` file -> `Properties` -> `Security` -> `Advanced` -> Disable inheritance-> Remove every user and group except for your specific active Windows profile. The file must be completely locked down!
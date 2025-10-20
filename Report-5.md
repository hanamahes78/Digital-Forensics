# Forensic Analysis of Linux Web Server

## Introduction
As a forensic investigator, the objective of this case is to analyze a compromised Linux web server to determine how the attacker gained access to the system, which modifications were made, and what persistence mechanisms were implemented (for example, backdoors). The investigation aims to identify the root cause of the compromise, enumerate malicious artifacts, and produce actionable remediation guidance.

## Tools Used
- `Autopsy` used as the main forensic analysis tool for examining file systems and generating reports.
- `FTK Imager` used to mount and explore the forensic disk image.
- `Linux forensic utilities` (strings, grep, last, cat) used for log examination and verification.

## Digital Evidence Description
The evidence consists of a forensic disk image of a compromised Linux web server with a total size of approximately 1.1 GB. The image contains the operating system files, web application directories, authentication logs, and user data. The server hosts a web application that was exploited during the attack.

## Forensic Process
1. **Evidence Acquisition**<br>
The forensic image was downloaded from the source https://archive.org/download/case1-webserver and verified for integrity prior to analysis.

2. **Image Mounting**<br>
The downloaded forensic image was opened in FTK Imager to enable examination of its contents.<br>
<img width="600" alt="Process-2a" src="">
<img width="600" alt="Process-2b" src="">

3. **Directory Inspection**<br>
The system directory was located within the root Logical Volume (LVM), particularly `/var/www/html`, `/etc`, `/home`, and `/var/log`, were reviewed to identify web applications and relevant files.<br>
<img width="600" alt="Process-3a" src="">
<img width="600" alt="Process-3b" src="">

4. **Login Log Analysis**<br>
To begin the analysis, the log files located in the `/var/log` directory were examined. The relevant log files for login investigation include `wtmp`, `btmp`, `auth.log`, and `lastlog`, as these files record user authentication events.

   The `last` command was used to display the user login history.<br>
   <img width="600" alt="Process-4a" src="">
   <img width="600" alt="Process-4b" src=""><br>
   <img width="600" alt="Process-4c" src=""><br>
   <img width="600" alt="Process-4d" src="">

   The `btmp` file records only failed login attempts. The `wtmp` file records all login sessions and is the default log accessed by the last command.

   From these logs, numerous failed login attempts were observed originating from the IP address `192.168.210.131`. Additionally, the user "**mail**" was found to have successfully logged in from the same IP address four times.
   
   To validate these findings, the `auth.log` file was analyzed. This log provides more detailed information regarding authentication attempts, the use of sudo, and other login-related events.<br>
   <img width="600" alt="Process-4e" src=""><br>

   Searching within `auth.log` for the IP address and username "**mail**" revealed multiple suspicious activities. Numerous failed login attempts targeting the root account were recorded, indicating a brute-force attack. Approximately one hour after the brute-force attempts, a new user named "**php**" was created and added to the sudo group, while the user "**mail**" was assigned a login shell, a password, and also added to the sudo group.
   
   These findings indicate that the attacker eventually discovered an alternative method to gain access to the system after the brute-force attack failed.
   
   The `lastlog` file, which displays the most recent login for each user and the originating host, was also examined. Although the file appeared to be corrupted, partial information was successfully retrieved using the `strings` command. The same IP address `192.168.210.131` found in `wtmp`, `btmp`, and `auth.log` was also present here, further confirm the intrusion source.<br>
   <img width="600" alt="Process-4f" src=""><br>

5. **User and Group Analysis**<br>
To collect information about system users and their associated groups, three files in the `/etc` directory were examined:
   - `passwd` contains a list of user accounts, home directories, login shells, and UID/GID information.
   - `shadow` stores hashed passwords for all users.
   - `group` lists system groups and their respective members.

   Two suspicious users were identified, "**mail**" and "**php**". From the `passwd` file, it was observed that both accounts used **bash** as their login shell and each had a corresponding home directory.
   
   Using the `grep` command on the `shadow` and `group` files confirmed that both users had password entries and were members of the **sudo** group, granting them administrative privileges.<br>
   <img width="600" alt="Process-5a" src=""><br>
   <img width="600" alt="Process-5b" src=""><br>

   Further examination of user directories revealed that the home directory of "**php**" contained only default files created during user account initialization. In contrast, the "**mail**" user directory contained a `.bash_history` file, which records all commands executed in the bash shell.<br>
   <img width="600" alt="Process-5c" src=""><br>
   <img width="600" alt="Process-5d" src=""><br>

   Analysis of the `.bash_history` file showed that the "**mail**" user frequently executed the command `sudo su -` to elevate privileges to root.<br>
   <img width="600" alt="Process-5e" src=""><br>

   Examination of the root user’s `.bash_history` confirmed that the `lastlog` file had been modified and referenced two additional suspicious files requiring further investigation:
   - `update.php`
   - A file with a `.c` extension bearing a suspicious name.

## Findings
1. **How Attacker Gain Access to System**<br>
Log analysis indicates that the attacker did not gain initial access via successful brute-force authentication. Given that the compromised host was a web server, the investigation focused on the deployed web application and its possible vulnerabilities. Inspection of `/var/www/html` revealed a Drupal installation.<br>
<img width="600" alt="Finding-1a" src=""><br>
<img width="600" alt="Finding-1b" src=""><br>
   The Drupal version was identified in `./jabc/includes/bootstrap.inc` as **Drupal 7.26**, which is known to be vulnerable to **CVE-2014-3704**.<br>
   <img width="600" alt="Finding-1c" src=""><br>
   This vulnerability can be exploited via specially crafted `POST` requests.

   Apache access logs were filtered for POST requests in the relevant time window.<br>
   <img width="600" alt="Finding-1d" src=""><br>
   <img width="600" alt="Finding-1e" src=""><br>
   Three suspicious POST requests originating from `192.168.210.131` were identified. <br>
   <img width="600" alt="Finding-1f" src=""><br>
   <img width="600" alt="Finding-1g" src=""><br>
   Further analysis of uploaded content and webroot files revealed a web shell script. The web shell provided remote command execution capability and was used to establish a reverse connection to the attacker host (192.168.210.131).

   In conclusion initial access was achieved by exploiting a remote code execution/SQL injection vulnerability in Drupal 7.26, which allowed the attacker to upload and execute a web shell and gain remote control of the server.

2. **Privileges the Attacker Obtain**<br>
After deploying the web shell, the attacker executed commands that resulted in privilege escalation. The attacker created or modified local accounts (notably `mail` and `php`) and added them to the sudo group. Command history and log entries indicate that the attacker used the web shell to execute commands that conferred root privileges (for example, via `sudo su -` and direct modifications to system account/group files).

   The attacker obtained root-level privileges by leveraging remote code execution through the web shell and by creating privileged accounts and/or executing privilege-escalation commands.

3. **Modifications Made to System**<br>
Observed system modifications include:
   - **Installation of backdoors/web shells** in the webroot to provide continued remote access.
   - **Creation and modification of user accounts**, specifically `mail` and `php`, and their addition to the `sudo` group.
   - **Tampering with log files** (`lastlog`), evidence of log modification or deletion intended to obscure attacker activity.
   - **Presence of suspicious files** such as `update.php` and a C source file (`*.c`) referenced in command history.
   - **Potential cron job alterations** to schedule persistence mechanisms.

4. **Persistence Mechanisms Used**<br>
The attacker implemented multiple persistence mechanisms to maintain access:
   - **Web shell** placed in web directories.
   - **Backdoor binaries or scripts** (identified among suspicious files).
   - **Cron job modifications** or additions that execute malicious code on a schedule or at reboot.
   - **SSH configuration changes** and/or addition of authorized keys (suspected) to permit direct SSH access.
   - **Unauthorized privileged user accounts** (members of `sudo`) to allow interactive or scripted escalation.

5. **System Recovered**<br>
The system can be remediated, but given the confirmed root compromise, the safest approach is to rebuild the host from a trusted image and restore services from known-good backups. If remediation in place is chosen, recommended actions are:<br>
a. Identify and remove all web shells, backdoors, and malicious binaries.<br>
b. Remove unauthorized user accounts and revoke any added sudo privileges.<br>
c. Restore or preserve forensic copies of modified or deleted logs for evidentiary purposes, not overwrite existing evidence.<br>
d. Patch the exploited web application (upgrade Drupal to a secure version) and apply all relevant OS and application security updates.<br>
e. Review and restore secure SSH and cron configurations, rotate all credentials and keys.<br>
f. Conduct a comprehensive integrity and malware scan, and monitor the environment for residual C2 activity before returning the host to production.

6. **Artifacts to Extract and Analyze**<br>
Extract and preserve the following artifacts for detailed analysis and reporting:
   - Web shell scripts found in `/var/www/html` and any other suspicious PHP/CGI files.
   - Suspicious binaries and source code files (identified `.c` file).
   - `/etc/passwd`, `/etc/shadow`, and `/etc/group` to document unauthorized account creation.
   - User home directories, especially `/home/mail/.bash_history` and root’s `.bash_history`.
   - Cron tables and files under `/etc/cron.*`.
   - SSH configuration and authorized_keys files (`/etc/ssh/sshd_config`, `/root/.ssh/authorized_keys`, `/home/*/.ssh/`).
   - Web server logs showing exploit `POST` requests and subsequent activity.
   - Authentication logs (`/var/log/auth.log`, `wtmp`, `btmp`, `lastlog`) including evidence of tampering.
   - File metadata and hashes (timestamps, SHA-256) for all suspicious files to create IOCs.
   - Any network artifacts showing reverse shells or C2 communication.

7. **Timeline Construction with log2timeline Plaso**<br>
Create a chronological timeline by ingesting relevant logs and file system metadata into log2timeline. Correlate web server access events, POST request timestamps, account creation events, sudo usage, file creation or modification times, and cron edits to reconstruct the attack lifecycle.<br>
<img width="600" alt="Finding-7a" src=""><br>
<img width="600" alt="Finding-7b" src=""><br>
<img width="600" alt="Finding-7c" src="">

8. **Reporting with Autopsy**<br>
Generate a formal forensic report via Autopsy (`Tools → Generate Report`) that includes objectives, methodology, extracted artifacts and their hashes, event timeline, IOCs, impact assessment, and remediation recommendations.

   [File Report](https://drive.google.com/drive/folders/1ZYnpp7PjfPqeNI_ECxjK-6YJBQJsYAKE)

## Conclusion
The forensic investigation confirmed that the attacker gained access to the Linux web server by exploiting a known vulnerability in **Drupal 7.26 (CVE-2014-3704)**, subsequently deploying a **web shell** to maintain remote control. The attacker **escalated privileges to root**, created unauthorized users, and established multiple persistence mechanisms, including modified cron jobs and SSH configurations.

The system can be remediated by:
- Removing all backdoors, web shells, and unauthorized users.
- Restoring modified configuration and log files from trusted backups.
- Patching the exploited vulnerability and applying the latest security updates.
- Conducting a full malware scan and rebuilding the system if root compromise is confirmed.
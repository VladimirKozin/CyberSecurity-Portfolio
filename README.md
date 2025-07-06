## Network Port & Firewall Audit for a Personal Workstation

Project Goal

This project outlines the methodology and steps taken to perform a basic security audit of network ports and firewall configurations on a personal workstation (covering both macOS and Windows environments). The objective is to identify potentially open entry points and assess the effectiveness of host-based and network-based protective measures. This demonstrates practical skills in network reconnaissance and defensive security.

Tools Utilized

lsof (List Open Files) - Command-line utility for macOS/Linux for displaying open files and network connections.

netstat - Command-line utility for displaying network connections, routing tables, interface statistics, etc. (available on both Windows and Linux/macOS, with platform-specific flags).

Online Port Scanners (e.g., canyouseeme.org, portchecker.co) - Used for externally validating port accessibility from the internet.

Operating System's Built-in Firewall (e.g., macOS Firewall, Windows Defender Firewall) - For analyzing host-based firewall configurations.

Audit Methodology & Steps

Identifying Locally Listening Ports

The initial step involves identifying which applications on the workstation are opening ports and actively listening for incoming connections. This provides an understanding of the host's local attack surface.

For macOS / Linux (using Terminal):

Execute the following command to list active listening TCP/UDP ports along with the processes using them:

sudo lsof -i -P -n | grep LISTEN

Rationale: sudo is used to gain the necessary privileges to view all system processes and their associated PIDs. lsof -i -P -n displays network files in numerical format, and grep LISTEN filters for actively listening ports.

Expected Output Analysis:
The output provides details such as COMMAND, PID, and NAME (protocol, IP address, and port). Key insights include:
Ports listening on 127.0.0.1 (localhost) are typically safe as they are only accessible from the local machine.
Ports listening on 0.0.0.0 or * (all interfaces) indicate potential external accessibility, which requires careful review of firewall rules.
Identification of processes (e.g., rapportd, ControlCe for macOS system services; steam_osx for gaming clients) using these ports helps determine their legitimacy and necessity.

For Windows (using Command Prompt):

Execute the following command in an administrator-level Command Prompt to list listening TCP/UDP ports and their associated PIDs:

netstat -ano | find "LISTEN"

Rationale: -a shows all connections, -n shows numerical addresses, and -o shows the PID. find "LISTEN" filters for listening states.

Expected Output Analysis:
The output displays Proto, Local Address (IP:Port), Foreign Address, State (LISTEN), and PID. Similar to macOS/Linux, attention should be paid to:
Local Address: 0.0.0.0 indicates listening on all interfaces, while 127.0.0.1 indicates local-only access.
PID: Used to identify the corresponding process in Task Manager (tasklist /svc /FI "PID eq [PID]") to understand which application is using the port.

External Port Accessibility Verification (Internet Perspective)

This step assesses whether any ports on the workstation are visible and accessible from the public internet, thereby evaluating the effectiveness of the router's firewall and any port forwarding configurations.

Tool: Online Port Scanning Services

Services like canyouseeme.org or portchecker.co can be used.

Steps:

Navigate to the chosen online port scanner (e.g., https://canyouseeme.org/).

The service will typically auto-detect your public IP address.

Enter a specific port number to check (e.g., common ports like 80, 443, 21, 22, 23, 3389, or application-specific ports like 27036 for Steam).

Initiate the port check.

Expected Outcomes & Analysis:
"Success": Indicates the port is open and accessible from the internet. This warrants investigation to ensure it is intentional, securely configured, and necessary.
"Error" / "Connection timed out": Indicates the port is closed or filtered by a firewall (router or host-based), effectively blocking external access. This is the desired outcome for most personal workstation ports.

System Firewall Configuration Analysis

Reviewing the operating system's built-in firewall settings is critical for host-level protection, controlling which applications can accept incoming connections.

For macOS (macOS Firewall):

Navigate to "System Settings" (or "System Preferences" on older versions).

Go to the "Network" section.

Select "Firewall" and ensure it is enabled.

Click "Options..." to review detailed rules.

Key Configuration Points to Check:
"Block all incoming connections": Should typically be disabled for normal usage, as it severely restricts network functionality.
"Automatically allow built-in software to receive incoming connections": Should generally be enabled to allow macOS system services to function securely.
"Automatically allow downloaded signed software to receive incoming connections": Should generally be enabled to permit trusted, signed applications to operate correctly.
Review specific application rules to ensure only necessary apps are explicitly allowed to accept incoming connections.

For Windows (Windows Defender Firewall):

Open "Control Panel" or search for "Windows Defender Firewall" in the Start Menu.

Ensure "Windows Defender Firewall is On".

Click on "Allow an app or feature through Windows Defender Firewall" for detailed rules.

Key Configuration Points to Check:
Review the list of "Allowed apps and features" to ensure only legitimate and necessary applications have permissions for private or public networks.
Understand "Inbound Rules" and "Outbound Rules" for advanced control. By default, Windows Firewall is configured to block most unsolicited inbound connections.

Key Findings & Lessons Learned

Understanding the Attack Surface: Regularly auditing open ports helps to map the potential entry points into a system, which is fundamental for effective security.

Layered Defense: Effective security relies on a multi-layered approach, combining network-level firewalls (routers) with host-based firewalls (OS-specific) to filter unwanted traffic.

Principle of Least Privilege: Only open ports and allow network access for services that are absolutely essential. Unnecessary open ports represent unnecessary risk.

Continuous Monitoring: The network landscape of a workstation can change with new software installations or updates. Periodic checks are crucial for maintaining security posture.

Legality and Ethics: All scanning activities must be performed on systems for which explicit permission has been granted (e.g., personal machines, lab environments).

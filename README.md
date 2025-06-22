# Scanning Linux: Authenticated vs Unauthenticated

## Part A: Lab Setup and Scan Execution

### 1. Creating the Linux VM in Azure

- **Operating System**: Ubuntu 22.04 LTS
- **Steps**:
  1. Go to the **Azure Portal** and navigate to **Virtual Machines > Create**.
  2. Select **Ubuntu 22.04 LTS** as the image .
  3. Configure the **inbound port rules** to allow:
     - **SSH (TCP/22)** for remote access.
     - **ICMP** (ping) by modifying the Network Security Group (NSG).
  4. Complete the setup and deploy the VM.
  5. Note the **public IP address** for remote access.

### 2. Connectivity Testing

- **Ping Test**:
  - From your local terminal, run:
    ```powershell
    ping <public-ip>
    ```
  - Confirm that the VM responds to ICMP requests.

- **SSH Access**:
  - Use the following command to log in:
    ```powershell
    ssh VMname@<public-ip>
    ```
  - Ensure successful login to verify SSH access.

### 3. Tenable Scan Setup

- **Tool Used**: Tenable Vulnerability Management
- **Scan Types**:
  - **Authenticated Scan**:
    - SSH credentials provided.
    - Full access to system internals (packages, configurations, services).
  - **Unauthenticated Scan**:
    - No credentials provided.
    - Limited to external visibility (open ports, banners, OS fingerprinting).
- **Target**: The Azure-hosted Ubuntu VM.

- ** Refernce to the Tennable scan result "
-  [Authenticated Scan PDF](https://github.com/SruthinagaK/azure-linux-tenable-scan-auth-vs-unauth-lab/blob/main/linux-scan_authenticated_Scan.pdf)
-  [Unauthenticated Scan PDF](https://github.com/SruthinagaK/azure-linux-tenable-scan-auth-vs-unauth-lab/blob/main/linux-scan_Unauthenticated_Scan.pdf)

- ## Part A: Vulnerability Identification and Comparison


## Vulnerability Comparison Using NIST RMF


| Plugin ID | Name | Severity | CVSS v3 Score | CVSS Vector | CWE ID | CWE Name | Patch Available |
|-----------|------|----------|---------------|-------------|--------|----------|-----------------|
| 240200 | PAM vulnerability (USN-7580-1) | High | 7.8 | CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H | CWE-22 | Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal') | Yes |
| 240162 | Requests vulnerabilities (USN-7568-1) | Medium | 6.5 | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N | CWE-200 | Exposure of Sensitive Information to an Unauthorized Actor | Yes |
| 240202 | UDisks vulnerability (USN-7578-1) | Medium | 7.8 | CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H | CWE-269 | Improper Privilege Management | Yes |
| 240195 | libblockdev vulnerability (USN-7577-1) | Medium | 7.8 | CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H | CWE-269 | Improper Privilege Management | Yes |
| 240097 | Python vulnerabilities (USN-7570-1) | Low | 5.3 | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N | CWE-20 | Improper Input Validation | Yes |
| 10114 | ICMP Timestamp Request Remote Date Disclosure | Low | 0.0 | CVSS:2.0/AV:N/AC:L/Au:N/C:N/I:N/A:N | CWE-200 | Exposure of Sensitive Information to an Unauthorized Actor | Yes |



### Vulnerability 1: PAM Privilege Escalation (USN-7580-1)
| **Field** | **Details** |
|----------|-------------|
| **Description** | A vulnerability in the `pam_namespace` module of PAM (Pluggable Authentication Modules) allows local attackers to exploit user-controlled paths to escalate privileges to root. This occurs due to improper handling of namespace configurations. |
| **CVSS v3 Base Score** | 7.8 |
| **CVSS v3 Rating** | High |
| **Vector** | `CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H` |
| **Attack Complexity** | Low |
| **Privileges Required** | Low |
| **User Interaction** | None |
| **Scope** | Unchanged |
| **Confidentiality Impact** | High |
| **Integrity Impact** | High |
| **Availability Impact** | High |
| **Date Published** | June 18, 2025 |
| **CWE ID and Name** | CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal') |
| **Exploit Exists?** | Yes, local privilege escalation confirmed |
| **Patch Available?** | Yes, fixed in `libpam-modules` for Ubuntu 22.04+, update required and reboot recommended[^3^][^5^][^8^] |

### Vulnerability 2: Python Requests Library Information Disclosure (USN-7568-1)
| **Field** | **Details** |
|----------|-------------|
| **Description** | Multiple vulnerabilities in the Python `requests` library allow remote attackers to leak sensitive information via improperly handled HTTP headers and URL parsing. |
| **CVSS v3 Base Score** | 6.5 |
| **CVSS v3 Rating** | Medium |
| **Vector** | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N` |
| **Attack Complexity** | Low |
| **Privileges Required** | None |
| **User Interaction** | None |
| **Scope** | Unchanged |
| **Confidentiality Impact** | Low |
| **Integrity Impact** | None |
| **Availability Impact** | None |
| **Date Published** | June 16, 2025 |
| **CWE ID and Name** | CWE-200: Exposure of Sensitive Information to an Unauthorized Actor |
| **Exploit Exists?** | Not publicly confirmed, but theoretically exploitable |
| **Patch Available?** | Yes, available via Ubuntu Pro updates[^11^] |

### Vulnerability 3: UDisks Privilege Escalation (USN-7578-1)
| **Field** | **Details** |
|----------|-------------|
| **Description** | A local privilege escalation vulnerability in UDisks allows attackers with console access to gain root privileges by exploiting improper handling of mount options during filesystem resizing. |
| **CVSS v3 Base Score** | 7.8 |
| **CVSS v3 Rating** | High |
| **Vector** | `CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H` |
| **Attack Complexity** | Low |
| **Privileges Required** | Low |
| **User Interaction** | None |
| **Scope** | Unchanged |
| **Confidentiality Impact** | High |
| **Integrity Impact** | High |
| **Availability Impact** | High |
| **Date Published** | June 18, 2025 |
| **CWE ID and Name** | CWE-269: Improper Privilege Management |
| **Exploit Exists?** | Not confirmed, but feasible in local environments |
| **Patch Available?** | Yes, fixed in `udisks2` package updates[^19^][^20^] |

## Part B: Risk Analysis Using NIST RMF

### Risk Impact Table (Based on NIST SP 800-30 Tables H-2 and H-3)
| **Vulnerability** | **Confidentiality Impact** | **Integrity Impact** | **Availability Impact** | **Overall Risk Level** | **Potential Organizational Impact** |
|-------------------|----------------------------|-----------------------|--------------------------|-------------------------|-------------------------------------|
| **PAM Privilege Escalation (USN-7580-1)** | High | High | High | **High** | Root access could lead to full system compromise, data theft, and service disruption. |
| **Requests Info Disclosure (USN-7568-1)** | Low | None | None | **Low** | May expose sensitive data in transit, affecting user trust and compliance. |
| **UDisks Privilege Escalation (USN-7578-1)** | High | High | High | **High** | Local attackers could gain root access, manipulate filesystems, and disrupt services. |


| **Vulnerability** | **Categorize** | **Select Controls** | **Implement** | **Assess** | **Authorize** | **Monitor** |
|-------------------|----------------|---------------------|---------------|------------|---------------|-------------|
| **PAM Privilege Escalation (USN-7580-1)** | High impact on Confidentiality, Integrity, and Availability | AC-6 (Least Privilege), SI-2 (Flaw Remediation), AU-2 (Audit Events) | Check if PAM configurations enforce namespace restrictions | Vulnerability scan shows control failure (exploit possible) | May require reauthorization or conditional ATO | Should be tracked in patch management and SIEM tools |
| **Python Requests Info Disclosure (USN-7568-1)** | Low impact on Confidentiality, None on Integrity and Availability | SC-8 (Transmission Confidentiality), SI-2 (Flaw Remediation) | Ensure secure HTTP headers and URL parsing in requests library | Vulnerability scan shows potential information leakage | Low risk, likely does not affect ATO | Monitor HTTP traffic and update requests library regularly |
| **UDisks Privilege Escalation (USN-7578-1)** | High impact on Confidentiality, Integrity, and Availability | AC-6 (Least Privilege), SI-2 (Flaw Remediation), AU-2 (Audit Events) | Check if UDisks configurations enforce proper mount options | Vulnerability scan shows control failure (exploit possible) | May require reauthorization or conditional ATO | Should be tracked in patch management and SIEM tools |


### Risk Management Hierarchy Tiers
**Tier 1 - Organization Level**  
- **Impact**: High-risk vulnerabilities like PAM and UDisks could compromise the entire infrastructure if exploited across multiple systems. This affects governance, compliance, and strategic objectives.

**Tier 2 - Mission/Business Process Level**  
- **Impact**: These vulnerabilities could disrupt critical business processes, especially if systems are used in production environments or handle sensitive data.

**Tier 3 - Information System Level**  
- **Impact**: Direct compromise of the affected Linux systems, leading to unauthorized access, data manipulation, and potential downtime.

## Part C: Continuous Monitoring Steps (NIST RMF Quick Start Guide)

1. **Define Monitoring Strategy**: Establish a comprehensive monitoring strategy aligned with organizational goals and risk tolerance.
2. **Identify Monitoring Requirements**: Determine specific monitoring requirements based on system characteristics and threat landscape.
3. **Select Monitoring Tools**: Choose appropriate tools and technologies for continuous monitoring.
4. **Develop Monitoring Policies**: Create policies and procedures to guide monitoring activities.
5. **Implement Monitoring Controls**: Deploy monitoring controls across the system and network.
6. **Collect Monitoring Data**: Gather data from various sources, including logs, alerts, and sensors.
7. **Analyze Monitoring Data**: Analyze collected data to identify potential security incidents and vulnerabilities.
8. **Respond to Findings**: Take appropriate actions to address identified issues and mitigate risks.
9. **Report Monitoring Results**: Communicate monitoring results to stakeholders and decision-makers.
10. **Review and Update Monitoring Strategy**: Regularly review and update the monitoring strategy to ensure its effectiveness.
11. **Integrate Monitoring with Risk Management**: Integrate continuous monitoring into the overall risk management framework.





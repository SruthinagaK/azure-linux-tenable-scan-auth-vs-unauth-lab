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



# workshop-AppConnectEnterprise-2026
ACE Enablement

## Remote Desktop Access

To access the workshop environment, use Remote Desktop Protocol (RDP) with one of the following servers:

### Server 1
- **Host**: `na4.services.cloud.techzone.ibm.com`
- **Port**: `22966`
- **Username**: `Administrator`
- **Password**: `IBMDem0s`

**Connection String**: `na4.services.cloud.techzone.ibm.com:22966`

### Server 2
- **Host**: `na4.services.cloud.techzone.ibm.com`
- **Port**: `21410`
- **Username**: `Administrator`
- **Password**: `IBMDem0s`

**Connection String**: `na4.services.cloud.techzone.ibm.com:21410`

### How to Connect

**Windows:**
1. Open **Remote Desktop Connection** (mstsc.exe)
2. Enter the connection string (e.g., `na4.services.cloud.techzone.ibm.com:22966`)
3. Click **Connect**
4. Enter credentials when prompted:
   - Username: `Administrator`
   - Password: `IBMDem0s`

**macOS:**
1. Open **Microsoft Remote Desktop** app
2. Click **Add PC**
3. Enter the host and port
4. Add user account with the provided credentials
5. Connect to the PC

**Linux:**
1. Use **Remmina** or **rdesktop**
2. Example with rdesktop:
   ```bash
   rdesktop na4.services.cloud.techzone.ibm.com:22966 -u Administrator -p IBMDem0s
   ```

---

## Workshop Tutorials

This repository contains hands-on tutorials for IBM App Connect Enterprise:

1. **[Create REST API](1.%20Create%20REST%20API/)** - Learn to create REST APIs in ACE
2. **[Transforming Data](2.%20Transforming%20/)** - Data transformation techniques
3. **[Connect to Database (DB2)](3.%20Connect%20to%20Database%20(DB2)/)** - Database integration with DB2
4. **[CI/CD with GitLab](4.CI-CD-Gitlab/)** - Version control and GitLab integration
5. **[SOAP Services](5.%20SOAP/)** - Working with SOAP web services

---

## Prerequisites

- IBM App Connect Enterprise Toolkit 13.0.6.0
- Git installed
- Access to workshop RDP environment
- Basic understanding of integration concepts

---

## Getting Started

1. Connect to the RDP environment using the credentials above
2. Open IBM App Connect Enterprise Toolkit
3. Follow the tutorials in order
4. Each tutorial folder contains detailed step-by-step instructions

---

## Support

For issues or questions during the workshop, please contact your workshop facilitator.

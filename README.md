# Multi-Tenant Automation with Ansible Automation Platform

This repository provides an Infrastructure as Code (IaC) solution for configuring multi-tenant environments in Ansible Automation Platform (AAP). It automates tenant provisioning, role-based access control (RBAC), and resource segregation while maintaining a common job structure across tenants.

## Features
- **Automated Tenant Deployment**: Create tenants, assign resources, and configure access using playbooks.
- **RBAC Implementation**: Ensures each tenant has isolated resources with predefined access permissions.
- **Common Job Structure**: Standardized automation jobs are available across tenants.
- **Credential Management**: Securely assigns cloud credentials per tenant.
- **Infrastructure as Code**: Uses Git-based workflows to manage configurations.

## Prerequisites
- Ansible Automation Platform (AAP) installed and configured.
- Access to **Automation Hub** for required collections.
- A **Controller Credential** configured to allow API access.
- Internet access to pull required collections from **console.redhat.com**.

## Setup Instructions

### 1. Configure the Ansible Automation Platform
#### a) Create a New Project
1. Navigate to **Projects** in AAP.
2. Click **Create New Project**.
3. Use **HTTPS Git URL** of this repo.
4. Set the project name as `Multi-Tenant`.
5. Assign the **Default Organization**.
6. Click **Save** and synchronize the project.

#### b) Setup Required Credentials
1. Ensure a **Controller Credential** exists in AAP.
2. Create an **Automation Hub Credential**:
   - Retrieve the token from [console.redhat.com](https://console.redhat.com/).
   - Navigate to **Credentials** in AAP.
   - Select type **Automation Hub** and paste the token.
   - Set `Galaxy Server URL` to `console.redhat.com`.

#### c) Sync the Project
- Navigate to **Projects**.
- Click **Sync** to fetch required collections from Automation Hub.

### 3. Deploy a Tenant
1. Go to **Templates** and create a new **Job Template**:
   - Name: `Multi-Tenant Demo`
   - Project: `Multi-Tenant`
   - Playbook: `demo.yaml`
   - Inventory: `Demo Inventory`
   - Credential: `Controller Credential`
2. Save and launch the job to provision a new tenant.

### 4. Verify Tenant Deployment
#### a) Check Tenant Resources
- Navigate to **Inventories**:
  - Each tenant should have a dedicated inventory.
  - If a tenant has multiple cloud accounts, multiple sources should be visible.
- Navigate to **Credentials**:
  - Each tenant has access only to their specific cloud credentials.
- Navigate to **Users & Teams**:
  - A team named `<TenantName> Users` is created.
  - Users can be added to this team manually or via SAML/LDAP.

#### b) Validate User Access
- Log in as a tenant user (e.g., `T1_U1`).
- Confirm access to:
  - **Common automation jobs** (App Restart, Data Refresh, Enable Debug).
  - **Only tenant-specific credentials**.
  - **Only tenant-specific inventory**.

### 5. Customization
- Modify files in`common_vars/` to define additional global settings.
- Add global jobs in `jobs.yml`.
- Extend RBAC rules in `roles.yml` to customize team structures.
- Add new credential types for multi-cloud support.

## Troubleshooting
- **Project Sync Fails**: Ensure Automation Hub credentials are correctly configured.
- **Job Execution Issues**: Verify the correct credentials and inventories are assigned.
- **RBAC Not Working as Expected**: Check user-team assignments and playbook permissions.

## Conclusion
This repository provides an automated way to set up a multi-tenant environment in Ansible Automation Platform. It ensures tenant isolation, maintains shared automation jobs, and enables Infrastructure as Code for automation management.

For further details or contributions, refer to the repository documentation or open an issue.


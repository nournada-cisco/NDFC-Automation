# NDFC Automation  
Automated provisioning and management of **Cisco NDFC (Nexus Dashboard Fabric Controller)** using **Ansible**. This repository includes playbooks for **fabric deployment, interface management, vPC setup, network provisioning, and policy configuration**—streamlining NDFC operations with **infrastructure-as-code**.

## 📌 Features  
- **Automated VXLAN EVPN Fabric Deployment**  
- **vPC Peering & Interface Configuration**  
- **Network & VRF Management**  
- **Ansible Tags for Modular Execution**  
- **Integration with Cisco NDFC APIs**  

---

## 📂 Repository Structure  

```bash
NDFC_Automation/
│── group_vars/               # Fabric variables (NDFC settings, VRFs, networks, etc.)
│── roles/                    # Modular Ansible roles
│   ├── create_fabric/        # Creates the NDFC fabric
│   ├── add_inventory/        # Adds devices to the fabric
│   ├── setup_vpc/            # Configures vPC peer links
│   ├── manage_interfaces/    # Manages host-facing and vPC interfaces
│   ├── manage_overlay/       # Manages VRFs and Networks
│   ├── manage_interface_groups/ # Manages interface groups and network associations
│── hosts.stage.yml           # Defines Ansible connection to NDFC
│── build_fabric.yml          # Main playbook for fabric deployment
│── README.md                 # Documentation
```

---
## 🚀 Installation & Setup
Ensure **Ansible** is installed on your system before running the playbooks.

**Step 1: Install Ansible**
```bash
pip install ansible==9.5.1
```

Verify installation:

```bash
ansible --version
```

**Step 2: Install JMESPath (Required for Ansible JSON Queries)**
```bash
pip install jmespath==1.0.1
```
**Step 3: Install the Cisco NDFC Ansible Collection**
```bash
ansible-galaxy collection install cisco.dcnm -p ./collections
```
Verify installation:
```bash
ansible-galaxy collection list | grep -A 5 "ndfclab/ansible"
```
You should see:
```bash
# /home/cisco/Documents/ndfclab/ansible/collections/ansible_collections
Collection                               Version
---------------------------------------- -------
cisco.dcnm                               3.6.0
```
---
## 🛠️ Configuration
**1️⃣ Edit NDFC Connection Information**

Modify ```hosts.stage.yml``` with your NDFC IP, username, and password.
```bash
ndfc:
  children:
    stage:
      hosts:
        10.105.11.145:  # NDFC IP address
          ansible_connection: ansible.netcommon.httpapi
          ansible_httpapi_use_ssl: true
          ansible_httpapi_validate_certs: false
          ansible_python_interpreter: auto_silent
          ansible_network_os: cisco.dcnm.dcnm
          ansible_user: admin
          ansible_password: Cisco@123
```
**2️⃣ Customize Variables (Fabric, VRFs, Networks, Interfaces)**

Modify the necessary files inside ```group_vars/``` to match your fabric topology.

* ```group_vars/stage/fabric.yml``` → General fabric settings
* ```group_vars/stage/interfaces.yml``` → Interface configurations
* ```group_vars/stage/vrfs.yml``` → VRF and network assignments
---

## 🚀 Running the Playbook
Run the **main playbook** to build the fabric:




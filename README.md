# 🚀 NDFC Automation using Ansible
Automated provisioning and management of **Cisco NDFC (Nexus Dashboard Fabric Controller)** using **Ansible**. This repository includes playbooks for **fabric deployment, interface management, vPC setup, network provisioning, and policy configuration**—streamlining NDFC operations with **infrastructure-as-code**.

## 📌 Features

✅ Automated VXLAN EVPN Fabric deployment  
✅ vPC Peering & Interface Configuration  
✅ Network & VRF Management  
✅ Uses the Cisco Ansible NDFC Collection  
✅ Ansible Tags for granular task execution  

## 📂 Repository Structure

```
NDFC_Automation/
│── group_vars/               # Stores fabric variables (NDFC settings, VRFs, networks, etc.)
│── roles/                    # Ansible roles for modular automation
│   ├── create_fabric/        # Creates the NDFC fabric
│   ├── add_inventory/        # Adds devices to the fabric
│   ├── setup_vpc/            # Configures vPC peer links
│   ├── manage_interfaces/    # Manages host-facing and vPC interfaces
│   ├── manage_overlay/       # Manages VRFs and Networks
│   ├── manage_interface_groups/ # Manages interface groups and network associations
│── hosts.stage.yml           # Defines how Ansible connects to NDFC
│── build_fabric.yml          # The main playbook for fabric deployment
│── README.md                 # Documentation
```

---

## 🚀 Installation & Setup

Ensure **Ansible** is installed on your system before running the playbooks.

### Step 1: Install Ansible

```
pip install ansible==9.5.1
```

Verify installation:

```
ansible --version
```

### Step 2: Install JMESPath (Required for Ansible JSON Queries)

```
pip install jmespath==1.0.1
```

### Step 3: Install the Cisco NDFC Ansible Collection

```
ansible-galaxy collection install cisco.dcnm -p ./collections
```

Verify installation:

```
ansible-galaxy collection list | grep -A 5 "ndfclab/ansible"
```

You should see:

```
# /home/cisco/Documents/ndfclab/ansible/collections/ansible_collections
Collection                               Version
---------------------------------------- -------
cisco.dcnm                               3.6.0
```

---

## 🛠️ Configuration

### 1️⃣ Edit NDFC Connection Information  
Modify `hosts.stage.yml` with your NDFC IP, username, and password.

```yaml
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
          ansible_user: admin # NDFC username
          ansible_password: ****** # NDFC password
```

### 2️⃣ Customize Variables (Fabric, VRFs, Networks, Interfaces)  
Modify the necessary files inside `group_vars/` to match your fabric topology.

- `group_vars/stage/fabric.yml` → General fabric settings  
- `group_vars/stage/interfaces.yml` → Interface configurations  
- `group_vars/stage/vrfs.yml` → VRF and network assignments  

---

## 🚀 Running the Playbook

Run the main playbook to build the fabric:

```
ansible-playbook -i hosts.stage.yml build_fabric.yml --tags cf_vxlan
```

---

## 🎯 Using Ansible Tags

This playbook uses **Ansible Tags** to allow you to select specific tasks to run. Below are the available tags and their purposes:

| **Tag**               | **Description**                                      |
|----------------------|--------------------------------------------------|
| `cf_vxlan`          | Creates the VXLAN EVPN Fabric                     |
| `ai_fabric`         | Adds devices to the fabric                        |
| `vpc_all`           | Configures vPC peer links                         |
| `mi_hosts`          | Manages host-facing interfaces                    |
| `mi_vpc`            | Manages vPC interfaces                            |
| `mo_vrfs_nets`      | Configures VRFs and Networks                      |
| `create_groups`     | Creates interface groups                          |
| `associate_networks`| Associates networks with interface groups         |
| `deploy`           | Deploys the fabric and saves configuration        |

---

## 🔹 Example: Running Specific Tasks

### Run only vPC setup:

```
ansible-playbook -i hosts.stage.yml build_fabric.yml --tags vpc_all
```

### Deploy configurations:

```
ansible-playbook -i hosts.stage.yml build_fabric.yml --tags deploy
```

---

## 📌 Notes

✔️ Uncomment the roles you want to run in `build_fabric.yml`.  
✔️ Use Ansible Tags to run specific sections of the playbook.  
✔️ Modify `group_vars` to match your network topology before execution.  

---

## 📖 Documentation

For more details on the Cisco NDFC Ansible collection, visit:  
🔗 [Cisco NDFC Ansible Collection Docs](https://galaxy.ansible.com/cisco/dcnm)

---

## 📜 License

This project is licensed under the **MIT License**.

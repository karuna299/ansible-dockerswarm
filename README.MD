# Ansible Task: Docker Swarm, MySQL 8, and Redis

## Task Requirement

* Install **Docker Swarm**
* Install **MySQL version 8**
* Install **Redis**
* Use **Ansible**
* Make the solution **modular and flexible**
* Support **Debian** and **RedHat** systems

---

## Environment

* **Control Node**: Local machine (Ubuntu)
* **Managed Node**: AWS EC2 (Ubuntu)
* **Provisioning**: Terraform
* **Configuration Management**: Ansible

> Note: Ubuntu belongs to the **Debian OS family**, so Debian-specific Ansible tasks apply to Ubuntu while maintaining RedHat compatibility.

---

## Approach

* Terraform is used to provision the EC2 instance.
* Ansible is used to automate installation and configuration.
* The local machine acts as the **control node**.
* The EC2 instance acts as the **managed node**.
* Ansible connects to EC2 over **SSH (port 22)**.

---

## Ansible Design (Modularity)

The implementation uses **Ansible roles** to keep the setup modular and reusable.

Each major component is separated into its own role:

* Docker installation
* Docker Swarm initialization
* MySQL installation
* Redis installation

This avoids a single large playbook and makes the code easier to maintain and reuse.

---

## Project Structure

```text
ansible-docker/
├── inventory/
│   └── hosts.ini
├── group_vars/
│   └── all.yml
├── roles/
│   ├── docker/
│   │   └── tasks/main.yml
│   ├── swarm/
│   │   └── tasks/main.yml
│   ├── mysql/
│   │   └── tasks/main.yml
│   └── redis/
│       └── tasks/main.yml
└── playbook.yml
```

---

## OS Compatibility Logic

* **Debian family (Ubuntu/Debian)**:

  * Uses `apt`
* **RedHat family**:

  * Uses `yum`

OS-specific execution is controlled using Ansible `when` conditions based on `ansible_os_family`.

---

## Execution

Run the playbook from the control node:

```bash
ansible-playbook -i inventory/hosts.ini playbook.yml --private-key <key.pem>
```

---

## What the Playbook Does

1. Installs Docker on the host
2. Initializes Docker Swarm
3. Installs **MySQL 8** on the host OS
4. Installs Redis on the host OS
5. Starts and enables all required services

---

## Verification on EC2

### Docker & Swarm

```bash
docker --version
sudo docker info | grep -i swarm
```

Expected:

```
Swarm: active
```

---

### MySQL 8

```bash
mysql --version
sudo systemctl status mysql
```

Expected:

```
mysql  Ver 8.0.x
```

---

### Redis

```bash
redis-cli ping
sudo systemctl status redis-server
```

Expected:

```
PONG
```

---

## Database Seeding (Host MySQL)

MySQL uses **socket-based authentication** by default on Ubuntu.

### Access MySQL (no password)

```bash
sudo mysql
```

### Create database

```sql
CREATE DATABASE myapp_db;
```

---

## Outcome

* Docker Swarm installed and initialized
* MySQL **8** installed on host
* Redis installed on host
* Modular Ansible roles implemented
* Debian and RedHat compatibility achieved
* Task requirements fully satisfied



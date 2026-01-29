# Ansible Task: Docker Swarm with MySQL and Redis

## Task Objective

* Install **Docker Swarm**
* Deploy **MySQL 8** and **Redis**
* Use **Ansible**
* Keep the solution **modular and flexible**
* Ensure compatibility with **Debian** and **RedHat** systems

---

## Environment Used

* **Control Node**: Local machine (Ubuntu)
* **Managed Node**: AWS EC2 (Ubuntu)
* **Provisioning Tool**: Terraform
* **Configuration Tool**: Ansible

> Note: Ubuntu belongs to the **Debian OS family**, so Debian-based tasks apply to Ubuntu while still keeping RedHat compatibility.

---

## Ansible Design Approach

* Used **Ansible roles** for modularity:

  * `docker` – Docker installation
  * `swarm` – Docker Swarm initialization
  * `mysql` – MySQL service deployment
  * `redis` – Redis service deployment
* Used **OS family–based conditions**:

  * Debian family (Ubuntu/Debian)
  * RedHat family
* Centralized variables using `group_vars`

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

## Docker Installation Logic

* **Debian family (Ubuntu/Debian)** → `docker.io`
* **RedHat family** → `docker`

This ensures the playbook can be reused across different Linux distributions.

---

## Execution

Run the playbook from the control node:

```bash
ansible-playbook -i inventory/hosts.ini playbook.yml --private-key <key.pem>
```

---

## Verification on EC2

### Check Docker Swarm

```bash
sudo docker info | grep -i swarm
```

Expected output:

```
Swarm: active
```

---

### Check running services

```bash
sudo docker service ls
```

Expected services:

* mysql
* redis

---

### Check containers created by Swarm

```bash
sudo docker ps
```

Containers are automatically created and managed by Docker Swarm services.

---

## Outcome

* Docker installed successfully
* Docker Swarm initialized
* MySQL and Redis running as **Swarm services**
* Playbook works on **Ubuntu** and supports **Debian and RedHat**
* Task requirements satisfied using modular Ansible roles

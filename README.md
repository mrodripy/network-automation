# ansible-network-backups


Automatización simple para realizar **backups de configuración** en dispositivos de red (Cisco, Juniper, Arista) usando **Ansible**.


## 🚀 Características
- Conexión vía `network_cli` o `ssh`.
- Toma de configuraciones (`show running-config`, `show configuration`, etc.).
- Guarda backups con timestamp.
- Inventario modular por entornos.


## 📦 Requisitos
- Ansible ≥ 2.10
- Python ≥ 3.8
- Paquetes → ver `requirements.txt`


## 📁 Estructura
Ver árbol de directorios en el repositorio.


## ▶️ Ejecución
```bash
ansible-playbook -i inventories/lab/hosts.yml playbooks/backup-config.yml

# Proyecto: ansible-network-backups (Esqueleto listo para GitHub)

A continuación tienes la **estructura base completa** del proyecto + contenido mínimo de cada archivo. Puedes copiar/pegar tal cual en tu GitHub.

---

## 📁 Estructura del proyecto

```
ansible-network-backups/
├── README.md
├── inventories/
│   └── lab/
│       ├── hosts.yml
│       └── group_vars/
│           └── all.yml
├── playbooks/
│   └── backup-config.yml
├── roles/
│   └── backup/
│       ├── tasks/
│       │   └── main.yml
│       └── files/
├── requirements.txt
└── ansible.cfg
```

---

## 📝 README.md

```markdown
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
```

## 🧪 Pruebas en entorno virtual
Puedes usar **EVE-NG, GNS3 o CSR1000v** para simular dispositivos.

## 📚 Ejemplo de salida
```
backups/
├── R1_2025-01-01_12-00.cfg
├── R2_2025-01-01_12-01.cfg
```

## 📄 Licencia
MIT
```

---

## 📁 inventories/lab/hosts.yml

```yaml
all:
  children:
    routers:
      hosts:
        R1:
          ansible_host: 192.168.100.11
          ansible_network_os: cisco.ios.ios
        R2:
          ansible_host: 192.168.100.12
          ansible_network_os: cisco.ios.ios
```

---

## 📁 inventories/lab/group_vars/all.yml
```yaml
ansible_connection: network_cli
ansible_user: admin
ansible_password: admin123
backup_path: "./backups"
```

---

## ▶️ playbooks/backup-config.yml
```yaml
---
- name: Backup de configuraciones de red
  hosts: routers
  gather_facts: no
  roles:
    - backup
```

---

## 📁 roles/backup/tasks/main.yml
```yaml
---
- name: Ejecutar comando para obtener configuración
  ios_command:
    commands:
      - show running-config
  register: config_output

- name: Crear carpeta de backups si no existe
  ansible.builtin.file:
    path: "{{ backup_path }}"
    state: directory

- name: Guardar configuración con timestamp
  ansible.builtin.copy:
    content: "{{ config_output.stdout[0] }}"
    dest: "{{ backup_path }}/{{ inventory_hostname }}_{{ ansible_date_time.date }}_{{ ansible_date_time.time }}.cfg"
```

---

## 📄 requirements.txt
```text
ansible
```

---

## ⚙️ ansible.cfg
```ini
[defaults]
host_key_checking = False
retry_files_enabled = False
stdout_callback = yaml
pipelining = True
```

---

Si quieres, también puedo generar:
- pipeline de GitHub Actions (CI)
- script Python equivalente con Netmiko (para otro repo)
- versión del proyecto usando Nornir
- versión más avanzada con backups en S3 o almacenamiento remoto


# Ansible. Windows 

## Windows PowerShell
```
Enable-WSManCredSSP -Role Server -Force
```

## Ansible Vars
```
ansible_host: 10.10.10.2
ansible_user: domain\admin
ansible_password: "{{ ansible_password }}"
ansible_connection: winrm
ansible_winrm_transport: credssp
ansible_winrm_server_cert_validation: ignore
```

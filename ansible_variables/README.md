# Ansible Variables
> **Ansible Variables** là những giá trị định nghĩa để dùng lại trong playbook, thay vì hardcode. Variables có thể được định nghĩa ở nhiều nơi: inventory, playbook hoặc file riêng. 
> Nên tách ra thành file riêng thay vì định nghĩa trong inventory.

## 1. Định nghĩa trong Inventory
```yaml
all:
  children:
    vagrant:
      hosts:
        vagrant1:
          ansible_port: 2222
        vagrant2:
          ansible_port: 2200
        vagrant3:
          ansible_port: 2201
      vars:
        ansible_host: 127.0.0.1
        ansible_user: vagrant
        ansible_private_key_file: /home/minhquang/.vagrant.d/insecure_private_key
```
Các biến `ansible_host`, `ansible_user` và `ansible_private_key_file` dùng chung cho các server trong group `vagrant`.
## 2. Định nghĩa trong Playbook
```yaml
- name: Use variables in playbook
  hosts: webservers
  vars:
    app_version: 1.0
    server_name: webserver01
  tasks:
    - name: Print variables
      debug:
        msg: "App version is {{ app_version }}, server is {{ server_name }}"
```
### 3. Định nghĩa trong file riêng
Tạo folder `group_vars` cùng cấp với file `inventory.yaml`. Ví dụ trong `inventory.yaml` có group `webservers` và `dbservers`, thì trong `group_vars/` sẽ có `webservers.yaml` và `dbservers.yaml`.
```yaml
# group_vars/webservers.yaml
http_port: 80
max_clients: 200
```
Ansible sẽ tự động load file này cho group `webservers`.
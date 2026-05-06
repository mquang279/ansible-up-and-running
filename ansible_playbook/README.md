# Ansible Playbook
## 1. Định nghĩa
> **Ansible Playbook** là file YAML chứa danh sách các task mà Ansible sẽ chạy trên server. Mô tả trạng thái mong muốn của hệ thống và để Ansible thực thi theo thứ tự xác định. **Playbook** bao gồm các *play (kế hoạch)*, mỗi play chứa các *task (nhiệm vụ)*.

```
Playbook (list)
 ├── Play (dictionary)
 │    ├── hosts
 │    ├── vars
 │    ├── tasks (list)
 │    │     ├── Task (dictionary)
 │    │     └── Task
 │    └── handlers
 └── Play
```

```yaml
- name: Tên của play
  hosts: webservers
  tasks:
    - name: Tên task 1
      module_name: arguments
    - name: Tên task 2
      module_name: arguments
```
- `-name`: Tên đặt cho play
- `hosts`: Group server từ inventory
- `tasks`: Danh sách task, mỗi task dùng 1 module (`package`, `copy`, `file`, `service`, `template`,...)

## 2. Plays
**Plays** được định nghĩa bằng dictionary trong `YAML`. **Plays** sẽ chứa những variables sau:
- `name`: Tên của play
- `become`: Biến boolean, chỉ định có dùng `sudo` hay không.
- `vars`: Danh sách các biến và giá trị.
- `tasks`: Danh sách các task mà play sẽ thực thi.
# 3. Tasks
```yaml
- name: Ensure nginx is installed
  package:
    name: nginx
    update_cache: true
```
Mỗi task sẽ có `name` để mô tả hành động mà nó thực hiện. Tiếp theo là module mà nó sẽ thực hiện, ở ví dụ trên là `package` có nghĩa là cài đặt package.
Một số module thường được sử dụng là:
- `package`: Cài hoặc xoá packages sử dụng package manager của host.
- `copy`: Copy file từ host chạy Ansible sang server.
- `file`: Set thuộc tính của file.
- `service`: Start, stop hoặc restart một service.
- `template`: Sinh file từ template và copy vào server.

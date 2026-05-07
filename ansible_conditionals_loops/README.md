# Ansible Conditionals Loops
> **Ansible Conditionals Loops** là cách thêm logic điều khiển vào playbook:
> - `when` khi task có điều kiện.
> - `loop` dùng để lặp lại task nhiều lần.

## 1. Conditionals với `when`
Điều kiện dựa trên biến hoặc facts (thông tin server mà Ansible thu thập). Ví dụ cài `nginx` chỉ trên từng distribution:
```yaml
- name: Install Nginx with conditionals
  hosts: webservers
  become: yes
  tasks:
    - name: Install Nginx on Ubuntu
      apt:
        name: nginx
        state: present
      when: ansible_os_family == "Debian"
    - name: Install Nginx on CentOS
      yum:
        name: nginx
        state: present
      when: ansible_os_family == "RedHat"
```
Nhưng trong trường hợp này nên dùng module `package` thay vì chỉ định rõ package manager của từng distribution (chắc vậy).

Để xem danh sách các facts:
```bash
ansible <group_name> -i <inventory_path> -m setup
```

## 2. Loops
Ví dụ tạo 5 user:
```yaml
- name: Create multiple users
  hosts: webservers
  become: yes
  tasks:
    - name: Create users
      user:
        name: "{{ item }}"
        state: present
        shell: /bin/bash
        create_home: yes
      loop:
        - user1
        - user2
        - user3
        - user4
        - user5
```
- `loop:` : Danh sách giá trị để lặp
- `{{ item }}`: Giá trị hiện tại trong vòng lặp
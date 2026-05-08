# Ansible Error Handling
Nếu task fail (VD: không cài được package) thì Ansible sẽ báo lỗi. **Ansible Error Handling** là cách xử lý lỗi trong playbook để playbook không dùng khi gặp vấn đề và dùng để kiểm soát lỗi. 

Các cách xử lý lỗi của Ansible là:
- `ignore_errors`: Bỏ qua lỗi, tiếp tục chạy task khác
```yaml
- name: Install Nginx
  apt:
    name: nginx
    state: present
  become: yes
  ignore_errors: yes

- name: Debug after ignoring error
  debug:
    msg: "We ignored the error and continued!"
```

- `failed_when`: Tự định nghĩa khi nào task được coi là fail
```yaml
- name: Check disk usage
  hosts: webservers
  tasks:
    - name: Get disk usage
      shell: df -h / | tail -1 | awk '{print $5}' | cut -d'%' -f1
      register: disk_usage
    - name: Fail if disk usage is over 90%
      debug:
        msg: "Disk usage is {{ disk_usage.stdout }}% - too high!"
      failed_when: disk_usage.stdout | int > 90
```
- `register` và `when`: Kiểm tra kết quả task, chạy task khác nếu muốn. Ví dụ: Cài Nginx, nếu fail thì gửi thông báo.
```yaml
- name: Try to install Nginx
  apt:
    name: nginx
    state: present
  become: yes
  register: nginx_install
  ignore_errors: yes

- name: Notify if Nginx install failed
  debug:
    msg: "Failed to install Nginx on {{ ansible_hostname }}. Check the server!"
  when: nginx_install.failed
```


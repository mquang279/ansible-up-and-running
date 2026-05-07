# Ansible Role
> **Role** là cơ chế đóng gói để tổ chức, tái sử dụng và phân tách logic cấu hình theo từng đơn vị chức năng (ví dụ: nginx, db,...) thay vì viết tất cả playbook vào 1 file `YAML`.
> Nên dùng khi nhiều playbook/project dùng chung Role, task dài, nhiều service.

## 1. Cách sử dụng
**Role** có cấu trúc thư mục cố định, giúp Ansible tự nhận diện. Folder `roles/` sẽ nằm cùng cấp với các file playbook.
```
roles/<role_name>/{tasks, vars, templates, handlers}
```
- `tasks/`: Chứa file định nghĩa task (bắt buộc)
- `vars/`: Chứa biến (tuỳ chọn)
- `templates/`: Chứa file template (tuỳ chọn)
- `handlers/`: Chứa handler để restart service (tuỳ chọn)


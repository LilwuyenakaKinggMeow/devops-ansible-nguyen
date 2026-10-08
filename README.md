\# DevOps - Ansible



\## 1. Giới thiệu



Bài thực hành Ansible tự động cấu hình và triển khai 2 web server Ubuntu chạy trên Docker.



Mục tiêu:



\* Xây dựng Ansible Control Node.

\* Quản lý nhiều server thông qua Inventory.

\* Sử dụng Ansible Role.

\* Tự động cài đặt và cấu hình Nginx.

\* Deploy website bằng Jinja2 Template.

\* Sử dụng Handler.

\* Kiểm tra tính Idempotency.



\## 2. Công nghệ sử dụng



\* Ansible

\* Docker

\* Ubuntu 24.04

\* Nginx

\* OpenSSH

\* Jinja2

\* PowerShell



\## 3. Kiến trúc



```text

&#x20;                Ansible Control Node

&#x20;                        |

&#x20;                 devops-network

&#x20;                   /         \\

&#x20;                  /           \\

&#x20;               web01         web02

&#x20;                 |             |

&#x20;               Nginx         Nginx

&#x20;                :80           :80

```



\## 4. Cấu trúc project



```text

ansible-web/

├── Dockerfile

├── Dockerfile.web

├── ansible.cfg

├── inventory

├── site.yml

├── README.md

├── group\_vars/

│   └── webservers.yml

└── roles/

&#x20;   └── web/

&#x20;       ├── handlers/

&#x20;       │   └── main.yml

&#x20;       ├── tasks/

&#x20;       │   └── main.yml

&#x20;       └── templates/

&#x20;           └── index.html.j2

```



\## 5. Inventory



Hai web server được quản lý:



```text

web01

web02

```



Ansible Control Node kết nối tới các server thông qua SSH.



\## 6. Ansible Role



Role `web` thực hiện các công việc:



1\. Update APT cache.

2\. Cài đặt Nginx.

3\. Enable và start Nginx.

4\. Deploy website.

5\. Restart Nginx khi website thay đổi.



\### Tasks



File:



```text

roles/web/tasks/main.yml

```



định nghĩa các task để cấu hình web server.



\### Handler



File:



```text

roles/web/handlers/main.yml

```



sử dụng handler để restart Nginx khi template website thay đổi.



\### Jinja2 Template



File:



```text

roles/web/templates/index.html.j2

```



Website sử dụng các Ansible facts để hiển thị:



\* Hostname

\* IP Address

\* Thông tin server được quản lý bởi Ansible



\## 7. Chạy Ansible



Kiểm tra kết nối tới các server:



```bash

ansible all -i /ansible/inventory -m ping

```



Chạy playbook:



```bash

ansible-playbook -i /ansible/inventory /ansible/site.yml

```



\## 8. Kiểm tra website



Web server 01:



```text

http://localhost:8081

```



Web server 02:



```text

http://localhost:8082

```



Mỗi server hiển thị hostname và IP tương ứng.



\## 9. Kiểm tra Idempotency



Sau khi cấu hình hoàn tất, chạy playbook lần thứ hai:



```bash

ansible-playbook -i /ansible/inventory /

```




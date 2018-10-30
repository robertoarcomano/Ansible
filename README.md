# Ansible Example Use

## 0. Variables
```
# HOST=rb2
HOST=ubuntu_ssh1
HOSTSPATH="-i ./hosts"
PARAMS="$HOST $HOSTSPATH"
```

## 1. Launch ping command on $HOST
```ansible $HOST -m ping```

## 2. Launch a command, uptime
```
ansible $HOST -m command -a uptime
ansible $PARAMS -a uptime
```

## 3. Launch a command, read syslog
```ansible $PARAMS -a "tail /var/log/syslog"```

## 4. Launch a command, check user
```ansible $PARAMS -a "id"```

## 5. Install nginx
```ansible $PARAMS -m apt -a "name=nginx update_cache=yes" -vvvv```

## 6. Start nginx
```ansible $PARAMS -m service -a "name=nginx state=started" -vvvv```

## 7. Group Execution
```ansible $HOSTSPATH webservers -m ping```

## 8. Playbooks
```ansible-playbook $HOSTSPATH playbook1.yml```

## playbook1.yml
```
- name: Configure webserver with nginx
  hosts: webservers
  become: True
  vars:
    message: "Hello world, this is a custom message from an Ansible playbook var"
  tasks:
    - name: install nginx
      apt: name=nginx update_cache=yes
      notify: restart nginx

    - name: copy nginx config file
      copy: src=files/nginx.conf dest=/etc/nginx/sites-available/default
      notify: restart nginx

    - name: enable configuration
      file: >
        dest=/etc/nginx/sites-enabled/default
        src=/etc/nginx/sites-available/default
        state=link
      notify: restart nginx

    - name: copy index.html
      template: src=templates/index.html.j2 dest=/usr/share/nginx/html/index.html mode=0644
      notify: restart nginx

  handlers:
    - name: restart nginx
      service: name=nginx state=restarted
```

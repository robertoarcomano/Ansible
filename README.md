# Ansible Example Use

## 0. Variables
```
# HOST=rb2
# HOST=ubuntu_ssh1
HOSTSPATH="-i ./hosts"
# PARAMS="$HOST $HOSTSPATH"
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

## 8. Playbooks nginx
```
date > log.txt
ansible-playbook $HOSTSPATH create_image.yml 1>>log.txt 2>&1
ansible-playbook $HOSTSPATH create_containers.yml 1>>log.txt 2>&1
ansible-playbook $HOSTSPATH configure_nginx.yml 1>>log.txt 2>&1
ansible-playbook $HOSTSPATH test_nginx.yml 1>>log.txt 2>&1
ansible-playbook $HOSTSPATH remove_containers.yml 1>>log.txt 2>&1
ansible-playbook $HOSTSPATH remove_image.yml 1>>log.txt 2>&1
date >> log.txt
```

## Playbooks Files
#### <a href=create_image.yml>create_image.yml</a>
#### <a href=create_containers.yml>create_containers.yml</a>
#### <a href=configure_nginx.yml>configure_nginx.yml</a>
#### <a href=test_nginx.yml>test_nginx.yml</a>
#### <a href=remove_containers.yml>remove_containers.yml</a>
#### <a href=remove_image.yml>remove_image.yml</a>

## Log File
#### <a href=log.txt>log.txt</a>

# ansible_collection
> Выполнил все подготовительные работы
### Задание 1-2
> Создал новый модуль my_module.py, добавил в него содержимое шаблона
***
### Задание 3
> Добавил создание файла в новый модуль
```
(venv) falconow@falconow:~/lesson/ansible_collection/ansible$ cat ./lib/ansible/modules/my_module.py 
#!/usr/bin/python

# Copyright: (c) 2018, Terry Jones <terry.jones@example.org>
# GNU General Public License v3.0+ (see COPYING or https://www.gnu.org/licenses/gpl-3.0.txt)
from __future__ import (absolute_import, division, print_function)
__metaclass__ = type


DOCUMENTATION = r'''
---
module: my_test

short_description: This is my test module

# If this is part of a collection, you need to use semantic versioning,
# i.e. the version is of the form "2.5.0" and not "2.4".
version_added: "1.0.0"

description: This is my longer description explaining my test module.

options:
    name:
        description: This is the message to send to the test module.
        required: true
        type: str
    new:
        description:
            - Control to demo if the result of this module is changed or not.
            - Parameter description can be a list as well.
        required: false
        type: bool
# Specify this value according to your collection
# in format of namespace.collection.doc_fragment_name
extends_documentation_fragment:
    - my_namespace.my_collection.my_doc_fragment_name

author:
    - Your Name (@yourGitHubHandle)
'''

EXAMPLES = r'''
# Pass in a message
- name: Test with a message
  my_namespace.my_collection.my_test:
    name: hello world

# pass in a message and have changed true
- name: Test with a message and changed output
  my_namespace.my_collection.my_test:
    name: hello world
    new: true

# fail the module
- name: Test failure of the module
  my_namespace.my_collection.my_test:
    name: fail me
'''

RETURN = r'''
# These are examples of possible return values, and in general should use other names for return values.
original_message:
    description: The original name param that was passed in.
    type: str
    returned: always
    sample: 'hello world'
message:
    description: The output message that the test module generates.
    type: str
    returned: always
    sample: 'goodbye'
'''

from ansible.module_utils.basic import AnsibleModule
import os

def run_module():
    # define available arguments/parameters a user can pass to the module
    module_args = dict(
        path=dict(type='str', required=True),
        name=dict(type='str', required=True),
        content=dict(type='str', required=True)
    )
  

    # seed the result dict in the object
    # we primarily care about changed and state
    # changed is if this module effectively modified the target
    # state will include any data that you want your module to pass back
    # for consumption, for example, in a subsequent task
    result = dict(
        changed=False,
        original_message='',
        message=''
    )

    # the AnsibleModule object will be our abstraction working with Ansible
    # this includes instantiation, a couple of common attr would be the
    # args/params passed to the execution, as well as if the module
    # supports check mode
    module = AnsibleModule(
        argument_spec=module_args,
        supports_check_mode=True
    )

    # if the user is working with this module in only check mode we do not
    # want to make any changes to the environment, just return the current
    # state with no modifications
    path=module.params["path"]
    name=module.params["name"]
    content=module.params["content"]
    if module.check_mode:
      if not (os.path.exists(path)):
        result['changed']=True

      if (os.path.exists(path_file)):
        with open(path_file) as file:
          read_text = file.read()
        if (read_text==content):
          result['changed']=False
        else:
          result['changed']=True
      else:
        result['changed']=True
          
      module.exit_json(**result)

    # manipulate or modify the state as needed (this is going to be the
    # part where your module will do what it needs to do)
    result['original_message'] = "Createfile"
    result['message'] = 'Createfile'

    # use whatever logic you need to determine whether or not this module
    # made any modifications to your target
    
    # Путь к файлу
    path_file = os.path.abspath(path)+'/'+name
    #Проверяем существует ли директория
    if not (os.path.exists(path)):
      os.makedirs(path)
      result['changed']=True
    
    #Проверяем наличие файла и его содержимое
    if (os.path.exists(path_file)):
      with open(path_file) as file:
        read_text = file.read()
      if (read_text==content):
        result['changed']=False
      else:
        with open(os.path.abspath(path)+'/'+name,'w') as file:
          file.write(content)
          result['changed']=True
    else:
      with open(os.path.abspath(path)+'/'+name,'w') as file:
        file.write(content)
        result['changed']=True
    
    # during the execution of the module, if there is an exception or a
    # conditional state that effectively causes a failure, run
    # AnsibleModule.fail_json() to pass in the message and the result
    if module.params['name'] == 'fail me':
        module.fail_json(msg='You requested this to fail', **result)

    # in the event of a successful module execution, you will want to
    # simple AnsibleModule.exit_json(), passing the key/value results
    module.exit_json(**result)


def main():
    run_module()


if __name__ == '__main__':
    main()
```
***
### Задание 4
> Создал json
```
{
  "ANSIBLE_MODULE_ARGS":{
    "path":"/tmp/netology",
    "name":"script.py",
    "content":"#This is test script netology"
  }
}
```
> Директория до выполнения 
```
(venv) falconow@falconow:~/lesson/ansible_collection/ansible$ tree /tmp/
/tmp/
├── config-err-Hy3B7b
├── mc-falconow
├── snap.snap-store [error opening dir]
├── ssh-NVR276pQ1DdM
│   └── agent.1547
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-colord.service-mMWfbg [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-ModemManager.service-8iDPAg [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-switcheroo-control.service-bpFmWf [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-systemd-logind.service-xlH6wj [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-systemd-resolved.service-EaERdg [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-systemd-timesyncd.service-S2Zepi [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-upower.service-doqkVi [error opening dir]
├── Temp-b7eb9399-375f-4b3d-8dc4-d96a5156a18f
├── tracker-extract-files.1000
└── vscode-typescript1000

13 directories, 2 files
```

> Запускаем
```
(venv) falconow@falconow:~/lesson/ansible_collection/ansible$ python3 -m ansible.modules.my_module payload.json 

{"changed": true, "original_message": "Createfile", "message": "Createfile", "invocation": {"module_args": {"path": "/tmp/netology", "name": "script.py", "content": "#This is test script netology"}}}
```

> Проверяем
```
(venv) falconow@falconow:~/lesson/ansible_collection/ansible$ tree /tmp/
/tmp/
├── config-err-Hy3B7b
├── mc-falconow
├── netology
│   └── script.py
├── snap.snap-store [error opening dir]
├── ssh-NVR276pQ1DdM
│   └── agent.1547
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-colord.service-mMWfbg [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-ModemManager.service-8iDPAg [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-switcheroo-control.service-bpFmWf [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-systemd-logind.service-xlH6wj [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-systemd-resolved.service-EaERdg [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-systemd-timesyncd.service-S2Zepi [error opening dir]
├── systemd-private-49d8619f88f34cf8a40da74443e45f3b-upower.service-doqkVi [error opening dir]
├── Temp-b7eb9399-375f-4b3d-8dc4-d96a5156a18f
├── tracker-extract-files.1000
└── vscode-typescript1000

14 directories, 3 files
```
***
### Задание 5-7
> Playbook
```
---
- name: Module create file
  hosts: localhost
  tasks:
    - name: Create file
      my_module:
        path: "/tmp/netology_test"
        name: "script.py"
        content: "This is script created Module"
```
```
(venv) falconow@falconow:~/lesson/ansible_collection/ansible$ ansible-playbook site.yml 
[WARNING]: You are running the development version of Ansible. You should only run Ansible from "devel" if you are modifying the Ansible engine, or
trying out features under development. This is a rapidly changing source of code and can become unstable at any point.
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [Module create file] ****************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************
ok: [localhost]

TASK [Create file] ***********************************************************************************************************************************
changed: [localhost]

PLAY RECAP *******************************************************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
```
(venv) falconow@falconow:~/lesson/ansible_collection/ansible$ ansible-playbook site.yml 
[WARNING]: You are running the development version of Ansible. You should only run Ansible from "devel" if you are modifying the Ansible engine, or
trying out features under development. This is a rapidly changing source of code and can become unstable at any point.
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [Module create file] ****************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************
ok: [localhost]

TASK [Create file] ***********************************************************************************************************************************
ok: [localhost]

PLAY RECAP *******************************************************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

***

### Задание 8
```
falconow@falconow:~/lesson$ ansible-galaxy collection init falconow.test_collection
- Collection falconow.test_collection was created successfully
```
***
### Задание 9
```
falconow@falconow:~/lesson/falconow$ tree
.
└── test_collection
    ├── docs
    ├── galaxy.yml
    ├── plugins
    │   ├── modules
    │   │   └── my_module.py
    │   └── README.md
    ├── README.md
    └── roles

5 directories, 4 files
```
***
### Задание 10-12
```
(venv) falconow@falconow:~/lesson/ansible_collection/ansible$ ansible-playbook site.yml 
[WARNING]: You are running the development version of Ansible. You should only run Ansible from "devel" if you are modifying the Ansible engine, or
trying out features under development. This is a rapidly changing source of code and can become unstable at any point.
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'
[WARNING]: Found variable using reserved name: name

PLAY [Module create file] ****************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************
ok: [localhost]

TASK [test_role : Create file] ***********************************************************************************************************************
changed: [localhost]

PLAY RECAP *******************************************************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

(venv) falconow@falconow:~/lesson/ansible_collection/ansible$ 
(venv) falconow@falconow:~/lesson/ansible_collection/ansible$ ansible-playbook site.yml 
[WARNING]: You are running the development version of Ansible. You should only run Ansible from "devel" if you are modifying the Ansible engine, or
trying out features under development. This is a rapidly changing source of code and can become unstable at any point.
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'
[WARNING]: Found variable using reserved name: name

PLAY [Module create file] ****************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************
ok: [localhost]

TASK [test_role : Create file] ***********************************************************************************************************************
ok: [localhost]

PLAY RECAP *******************************************************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

(venv) falconow@falconow:~/lesson/ansible_collection/ansible$ 
```
> Заполнил документацию, выложил в репозиторий
***

### Задание 13
```
falconow@falconow:~/lesson/falconow/test_collection$ ansible-galaxy collection build
[WARNING]: You are running the development version of Ansible. You should only run Ansible from "devel" if you are modifying the Ansible
engine, or trying out features under development. This is a rapidly changing source of code and can become unstable at any point.
Created collection for falconow.test_collection at /home/falconow/lesson/falconow/test_collection/falconow-test_collection-1.0.0.tar.gz
```
***

### Задание 14
```
falconow@falconow:~/lesson$ mkdir test 
falconow@falconow:~/lesson$ cp ./ansible_collection/ansible/site.yml ./test
cp ./falconow/test_collection/falconow-test_collection-1.0.0.tar.gz ./test
```
```
falconow@falconow:~/lesson$ cd ./test/
falconow@falconow:~/lesson/test$ tree
.
├── falconow-test_collection-1.0.0.tar.gz
└── site.yml

0 directories, 2 files
falconow@falconow:~/lesson/test$ 
```
### Задание 15-16
```
falconow@falconow:~/lesson/test$ ansible-galaxy collection install falconow-test_collection-1.0.0.tar.gz 
[WARNING]: You are running the development version of Ansible. You should only run Ansible from "devel" if you are modifying the Ansible
engine, or trying out features under development. This is a rapidly changing source of code and can become unstable at any point.
Starting galaxy collection install process
Process install dependency map
Starting collection install process
Installing 'falconow.test_collection:1.0.0' to '/home/falconow/.ansible/collections/ansible_collections/falconow/test_collection'
falconow.test_collection:1.0.0 was installed successfully
```

```
falconow@falconow:~/lesson/test$ ansible-playbook site.yml 
[WARNING]: You are running the development version of Ansible. You should only run Ansible from "devel" if you are modifying the Ansible
engine, or trying out features under development. This is a rapidly changing source of code and can become unstable at any point.
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'
[WARNING]: Found variable using reserved name: name

PLAY [Module create file] *******************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [localhost]

TASK [falconow.test_collection.test_role : Create file] *************************************************************************************
changed: [localhost]

PLAY RECAP **********************************************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

falconow@falconow:~/lesson/test$ 
falconow@falconow:~/lesson/test$ ansible-playbook site.yml 
[WARNING]: You are running the development version of Ansible. You should only run Ansible from "devel" if you are modifying the Ansible
engine, or trying out features under development. This is a rapidly changing source of code and can become unstable at any point.
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'
[WARNING]: Found variable using reserved name: name

PLAY [Module create file] *******************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [localhost]

TASK [falconow.test_collection.test_role : Create file] *************************************************************************************
ok: [localhost]

PLAY RECAP **********************************************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

falconow@falconow:~/lesson/test$ 
```
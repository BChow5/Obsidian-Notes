## Ansible loops with `loop`[](#ansible-loops-with-loop)

> Ansible offers the `loop`, `with_<lookup>`, and `until` keywords to execute a task multiple times. Examples of commonly-used loops include changing ownership on several files and/or directories with the [file module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html#file-module), creating multiple users with the [user module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html#user-module), and repeating a polling step until a certain result is reached.
> 
> - [Ansible Loops docs](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html)

Ansible will automatically replace the variable `item` with the current value in a loops iteration. So in the example below the first iteration `item=user-name1` and the second iteration `item=user-name2`.

```
- name: Add several users to the wheel group
  ansible.builtin.user:
    name: "{{ item }}"
    state: present
    groups: "wheel"
  loop:
     - user-name1
     - user-name2
```

### iterating over a list of hashes[](#iterating-over-a-list-of-hashes)

```
- name: Add several users
  ansible.builtin.user:
    name: "{{ item.name }}"
    state: present
    groups: "{{ item.groups }}"
  loop:
    - { name: 'testuser1', groups: 'wheel' }
    - { name: 'testuser2', groups: 'root' }
```

### Installing packages with a loop[](#installing-packages-with-a-loop)

Some Ansible modules accept a list as a parameter value, package managers for example. If this is the case it is often preferable to pass a list as a parameter than to use a loop to perform a task.

Example from the [Ansible docs](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html)

```
- name: Optimal yum
  ansible.builtin.yum:
    name: "{{ list_of_packages }}"
    state: present

- name: Non-optimal yum, slower and may cause issues with interdependencies
  ansible.builtin.yum:
    name: "{{ item }}"
    state: present
  loop: "{{ list_of_packages }}"
```

Shell scripting example with package manager to help illustrate the above.

```
# create an array of packages
packages=("vim" "ripgrep" "tmux")

# use a loop to install packages
# calls the "dnf install" command for every package
for package in "${packages[@]}"; do
  dnf install $package
done

# Install all packages with a single "dnf install" command
dnf install "${packages[@]}"
```

## Ansible Conditionals with `when`[](#ansible-conditionals-with-when)

> In a playbook, you may want to execute different tasks or have different goals, depending on the value of a fact (data about the remote system), a variable, or the result of a previous task. You may want the value of some variables to depend on the value of other variables. Or you may want to create additional groups of hosts based on whether the hosts match other criteria. You can do all of these things with conditionals.
> 
> - [Ansible Conditionals docs](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_conditionals.html)

```
- name: Install ngnix
  hosts: all
  become: true
  tasks:
  - name: Get the distribution
    set_fact: 
      distribution: "{{ ansible_facts['distribution'] }}" 
  - name: install nginx in Fedora
    dnf:
      name=nginx
    when: distribution == "Fedora"
  - name: install nginx in Debian
    apt:
      name: ngnix
    when: distribution == "Debian"
```

### combining loops and conditionals[](#combining-loops-and-conditionals)

```
tasks:
    - name: Run with items greater than 5
      ansible.builtin.command: echo {{ item }}
      loop: [ 0, 2, 4, 6, 8, 10 ]
      when: item > 5
```

## Ansible handlers for working with services[](#ansible-handlers-for-working-with-services)

> Sometimes you want a task to run only when a change is made on a machine. For example, you may want to restart a service if a task updates the configuration of that service, but not if the configuration is unchanged. Ansible uses handlers to address this use case. Handlers are tasks that only run when notified.
> 
> - [Ansible Handlers docs](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_handlers.html)

The primary use case of Handlers is in working with services. For example you may have a playbook that performs the following tasks:

- updates documents served by nginx
- changes your nginx configuration
- reloads the nginx service

If the second time you run this playbook you only need to update a document that is served by nginx you don't need to reload the nginx service, because the configuration hasn't changed. You can do this by using the notify keyword and handlers.

In the example below the list item in `notify:` must correspond to a handler name in the handlers section

```
tasks:
- name: Template configuration file
  ansible.builtin.template:
    src: template.j2
    dest: /etc/foo.conf
  notify:
    - Reload nginx
    - Restart memcached

handlers:
  - name: Restart memcached
    ansible.builtin.service:
      name: memcached
      state: restarted

  - name: Reload nginx
    ansible.builtin.service:
      name: nginx
      state: reloaded
```

### grouping handlers with `listen`[](#grouping-handlers-with-listen)

You can group handlers using the `listen` keyword.

In the example below the name in `notify` must mach to the string in `listen`

```
tasks:
  - name: Restart everything
    command: echo "this task will restart the web services"
    notify: "restart web services"

handlers:
  - name: Restart memcached
    service:
      name: memcached
      state: restarted
    listen: "restart web services"

  - name: Restart apache
    service:
      name: apache
      state: restarted
    listen: "restart web services"
```

## Ansible roles[](#ansible-roles)

> Roles let you automatically load related vars, files, tasks, handlers, and other Ansible artifacts based on a known file structure. After you group your content into roles, you can easily reuse them and share them with other users.
> 
> - [Ansible Roles docs](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)

### Where should roles go in your project[](#where-should-roles-go-in-your-project)

Ansible roles have a defined directory structure. Ansible will look for roles in the following locations:

- in collections, if you are using them
- ==in a directory called `roles/`, relative to the playbook file (This is the solution that we are going to use)==
- in the configured [roles_path](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#default-roles-path). The default search path is `~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles`.
- in the directory where the playbook file is located.

### Organizing the roles directory[](#organizing-the-roles-directory)

Inside of the roles directory individual directories are created for each role, for example a database role

```
ansible.cfg
roles/
	database/
```

Inside of each role, ie the database role directory ansible uses any of seven standard directories. If you don't need to include a directory you don't have to.

- Tasks The _tasks_ directory has a _main.yml_ file that serves as an entry point for the actions a role does.
    
- Files Holds files and scripts to be uploaded to hosts.
    
- Templates Holds Jinja2 template files to be uploaded to hosts.
    
- Handlers The _handlers_ directory has a _main.yml_ file that has the actions that respond to change notifications.
    
- Vars Variables that shouldn’t generally be overridden.
    
- Defaults Default variables that can be overridden.
    
- Meta Information about the role.
    

So the database example might look like this:

```
ansible.cfg
roles/
	database/
		tasks/
			main.yml
		vars/
			main.yml
```

**Resources:**

[Ansible Docs](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)

### Creating Ansible roles[](#creating-ansible-roles)

Because roles are just directories and files, you don't need any special tools for creating roles on your local machine.

You can also use existing roles, hosted on Ansible Galaxy. Because of this there is the `ansible-galaxy` command.

Today we are just going to focus on creating our own roles, and using them from local ansible configuration files. But the `ansible-galaxy` command can still help with this.

Try running the command below in the root of an ansible project. If the roles directory doesn't exist it will create it for you.

```
ansible-galaxy init --init-path roles db-role
```

This command will create the scaffolding for a 'db-role' inside of the roles directory. Which will create something like this:

```
.
└── roles
    └── db-role
        ├── defaults
        │   └── main.yml
        ├── files
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── README.md
        ├── tasks
        │   └── main.yml
        ├── templates
        ├── tests
        │   ├── inventory
        │   └── test.yml
        └── vars
            └── main.yml

11 directories, 8 files
```

**Resources:**

- [Ansible Docs](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html)
- [Ansible Galaxy](https://galaxy.ansible.com/ui/)

### Using a role in an Ansible playbook[](#using-a-role-in-an-ansible-playbook)

If your project includes a 'web.yml' playbook, and roles directory that contains a 'db-role' role, you can use that role from your playbook like so:

```
# web.yml
---
- name: Roles for the database servers
  hosts: database
  become: true
  roles:
    - db-role
```
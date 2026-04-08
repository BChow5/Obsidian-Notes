Flow control allows you to create more compact, reusable, and "intelligent" playbooks by managing how and when tasks execute based on conditions, errors, or repetitions.

---

### 1. Loops

Loops allow a single task to execute multiple times with different values, making playbooks more maintainable.

- **Keyword:** `loop`
    
- **Default Variable:** During execution, Ansible places the current iteration's value in a variable named `item`.
    
- **Data Types:** You can loop over lists, lists of hashes (dictionaries), or dictionaries.
    

#### Common Loop Patterns:

- **Simple List:**
    
    YAML
    
    ```
    loop: [ 'java-17-openjdk-devel', 'maven' ]
    # Access via {{ item }}
    ```
    
- **List of Hashes:** Used for multiple parameters (e.g., source and destination).
    
    YAML
    
    ```
    loop:
      - { src: 'a.conf', dest: '/etc/a.conf' }
    # Access via {{ item.src }} and {{ item.dest }}
    ```
    
- **Dictionaries:** Must use the `dict2items` filter to transform a dictionary into a loopable list.
    

#### Loop Control (`loop_control`):

- **`loop_var`:** Renames `item` to a custom name (essential for nested loops to avoid variable overwriting).
    
- **`pause`:** Sets a delay (in seconds) between each iteration.
    
- **`index_var`:** Tracks the current index (0, 1, 2...) of the loop in a specified variable.
    
- **`extended: true`:** Provides extra magic variables like `ansible_loop.first`, `ansible_loop.last`, and `ansible_loop.length`.
    

---

### 2. Registering Variables in Loops

When you use `register` with a `loop`, the output structure changes:

- The registered variable becomes a **dictionary** containing a key named **`results`**.
    
- `results` is a list where each element contains the output of one iteration.
    

---

### 3. Special Variables & Inventory Queries

- **`ansible_play_batch`:** List of hostnames currently being managed in the active batch.
    
- **`groups['group_name']`:** Accesses all hosts within a specific inventory group.
    
- **`query('inventory_hostnames', 'pattern')`:** A lookup plugin to get a list of hosts matching a specific pattern (e.g., `'all:!dbservers'` for all hosts except database servers).
    

---

### 4. Retrying Tasks (`until`)

Used to handle "temporal glitches" (like a busy package manager or network blip).

- **`until`:** The condition that must be met to stop retrying.
    
- **`retries`:** Maximum number of attempts (default is 3).
    
- **`delay`:** Seconds to wait between attempts (default is 5).
    

> **Warning:** Only use this for **idempotent** modules to avoid leaving the system in an unstable state.

---

### 5. Conditionals (`when`)

The `when` directive determines if a task should run or be skipped. It uses **Jinja2** expressions.

- **Based on Facts:** `when: ansible_facts['distribution'] == "Fedora"`
    
- **Based on Registered Variables:** Useful for "check-then-act" logic.
    
    - _Note:_ Use `ignore_errors: yes` on the first task so the playbook doesn't crash if the check "fails" (e.g., a file is missing).
        
- **Based on Variables:** `when: not special_mode`
    

#### Conditionals in Reusable Content:

- **`import_tasks`:** The condition is pre-processed and applied to **every task** inside the imported file.
    
- **`include_tasks`:** The condition is applied only to the **include task itself**.
    
- **Roles:** Can be conditionally applied: `- { role: my_role, when: is_db_server }`.
    

---

### 6. Jinja2 Operators Quick Reference

|**Type**|**Operators**|**Examples**|
|---|---|---|
|**Comparison**|`==`, `!=`, `>`, `<`, `>=`, `<=`|`age >= 18`|
|**Logic**|`and`, `or`, `not`, `( )`|`is_fedora and is_version_8`|
|**Tests**|`is defined`, `is failed`, `is success`|`my_var is defined`|
|**Containment**|`in`|`ansible_distribution in ['RedHat', 'CentOS']`|
### 1. Handlers

Handlers are special tasks that only run if "notified" by another task. They are typically used for service management (restarting/reloading).

- **Trigger Mechanism:** A task uses the `notify` directive. The handler only runs if the task reports a **`changed`** status. If no change occurs, the handler is skipped.
    
- **Execution Timing:** Handlers do not run immediately. They run **once** at the very end of the play, after all other tasks have finished.
    
- **De-duplication:** If multiple tasks notify the same handler, it still only runs **once**.
    
- **Order:** Handlers execute in the order they are listed in the `handlers:` section, not the order they are notified.
    

#### Grouping Handlers (`listen`):

Instead of notifying multiple handlers by name, you can use the `listen` keyword to create a topic.

- **Example:** Multiple handlers "listen" to `"restart web services"`. When a task notifies that string, all associated handlers trigger.
    

#### Flushing Handlers:

If you need handlers to run _immediately_ (e.g., restart a database before running a migration task), use the meta task:

YAML

```
- name: Flush handlers now
  ansible.builtin.meta: flush_handlers
```

---

### 2. Error Management

By default, if a task fails on a host, Ansible stops executing all further tasks for _that specific host_.

#### Ignoring Errors:

- **`ignore_errors: true`**: Ansible continues to the next task even if the current one fails.
    
- **`ignore_unreachable: true`**: Continues even if the host is down/unreachable.
    

#### Handlers and Failure:

If a play fails mid-way, triggered handlers will **not** run for that host.

- **Fix:** Use `--force-handlers` (CLI) or `force_handlers: true` (Playbook) to ensure handlers run even on failed hosts.
    

#### Customizing Failure and Change:

- **`failed_when`**: Define your own criteria for failure (e.g., fail if "error" appears in the `stdout` of a command, even if the exit code was 0).
    
- **`changed_when`**: Define when a task should report "changed" (e.g., a script that always returns 0 but you know it modified a file).
    

#### Aborting the Entire Play:

- **`any_errors_fatal: true`**: If _any_ host fails, stop the entire play for _all_ hosts immediately.
    
- **`max_fail_percentage`**: Stop the play only if a certain percentage of hosts (e.g., 50%) fail.
    

---

### 3. Blocks

Blocks logically group tasks together. They are useful for two main reasons:

#### A. Inheritance

Directives applied at the `block` level (like `when`, `become`, or `ignore_errors`) are inherited by every task inside that block.

YAML

```
block:
  - name: Task 1
  - name: Task 2
when: ansible_facts['os_family'] == "RedHat"
```

#### B. Error Handling (Try/Catch/Finally)

Blocks allow for sophisticated error recovery using `rescue` and `always`:

|**Block Directive**|**Purpose**|
|---|---|
|**`block`**|Defines the main tasks to attempt.|
|**`rescue`**|Tasks that run **only if** a task in the `block` fails (like a `catch`).|
|**`always`**|Tasks that run regardless of success or failure (like `finally`).|

> **Best Practice:** Avoid using variables in handler names. If a variable is missing at runtime, the entire play will fail.


## Ansible Roles (Chapter 8)

Ansible Roles are the standard way to bundle automation content. They allow you to move from writing "one-off" playbooks to creating reusable, parameterizable, and shareable components.

---

### 1. What is an Ansible Role?

A role is a structured directory that bundles tasks, variables, files, templates, and handlers.

- **Abstraction:** Users don't need to know _how_ Java is installed; they just call the `java` role.
    
- **Reusability:** The same role can be used across different projects and departments.
    
- **Portability:** Roles can be shared via Git, Ansible Galaxy, or web servers.
    

---

### 2. Consuming Roles (Ansible Galaxy)

**Ansible Galaxy** is the public hub for finding community-developed roles (e.g., `geerlingguy.java`).

- **Installation:** Use the CLI to download roles to your control node:
    
    `ansible-galaxy install username.role_name`
    
- **The `requirements.yml` file:** A best practice for managing multiple dependencies. It lists the roles needed for a project and their sources (Galaxy, Git, or URL).
    
    - _Command to install from file:_ `ansible-galaxy role install -r roles/requirements.yml`
        

---

### 3. Using Roles in a Playbook

There are two primary ways to invoke a role:

#### A. The `roles` Section (Classic)

Roles listed here run **before** any tasks defined in the play.

YAML

```
- hosts: all
  roles:
    - role: geerlingguy.java
      vars:
        java_packages: ["java-1.8.0-openjdk"]
```

#### B. The `include_role` Module (Flexible)

Allows you to call a role as a task, meaning it can run in the middle of other tasks.

YAML

```
tasks:
  - name: Install Java
    ansible.builtin.include_role:
      name: geerlingguy.java
```

---

### 4. Developing a Role

Use `ansible-galaxy role init role_name` to create the standard folder structure:

|**Directory**|**Purpose**|
|---|---|
|**`tasks/`**|The main list of tasks (starts at `main.yml`).|
|**`defaults/`**|**Low-priority** default variables for the role.|
|**`vars/`**|**High-priority** variables for the role.|
|**`handlers/`**|Handlers used by the role.|
|**`files/`**|Static files to be deployed.|
|**`templates/`**|Jinja2 templates.|
|**`meta/`**|Metadata (author, dependencies, platform support).|

---

### 5. Role Features & Best Practices

- **Parameterization:** Use variables (like `show_file` in the example) so users can customize the role's behavior without editing the role's code.
    
- **Handlers in Roles:** Handlers defined in a role are scoped to that role but can be notified by any task within it.
    
- **Single Responsibility:** A role should do one thing well (e.g., install Nginx, not Nginx + MySQL + PHP).
    
- **Dependencies:** If Role B requires Role A, define this in `meta/main.yml`. Ansible will automatically run Role A first.
    

---

### 6. Sharing Roles

The most common professional method is a **Git repository**. This allows for:

- **Versioning:** Use specific branches or tags in your `requirements.yml`.
    
- **Security:** Keep proprietary automation in private repositories.
    
- **Collaboration:** Multiple developers can contribute to the same role.
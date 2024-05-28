# Building an Inventory

Inventories organize managed nodes in centralized files that provide Ansible with system information and network locations. Using an inventory file, Ansible can manage a large number of hosts with a single command.

To complete the following steps, you will need the IP address or fully qualified domain name (FQDN) of at least one host system. For demonstration purposes, the host could be running locally in a container or a virtual machine. You must also ensure that your public SSH key is added to the `authorized_keys` file on each host.

Continue getting started with Ansible and build an inventory as follows:

1. Create a file named `inventory.ini` in the `ansible_quickstart` directory that you created in the [preceding step](get_started_ansible.html#get-started-ansible).

2. Add a new `[myhosts]` group to the `inventory.ini` file and specify the IP address or fully qualified domain name (FQDN) of each host system.

    ```ini
    [myhosts]
    192.0.2.50
    192.0.2.51
    192.0.2.52
    ```

3. Verify your inventory.

    ```sh
    ansible-inventory -i inventory.ini --list
    ```

4. Ping the `myhosts` group in your inventory.

    ```sh
    ansible myhosts -m ping -i inventory.ini
    ```

    > **Note**: Pass the `-u` option with the `ansible` command if the username is different on the control node and the managed node(s).

    ```text
    192.0.2.50 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
        },
        "changed": false,
        "ping": "pong"
    }
    192.0.2.51 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
        },
        "changed": false,
        "ping": "pong"
    }
    192.0.2.52 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
        },
        "changed": false,
        "ping": "pong"
    }
    ```

Congratulations, you have successfully built an inventory. Continue getting started with Ansible by [creating a playbook](get_started_playbook.html#get-started-playbook).

# Inventories in INI or YAML Format

You can create inventories in either `INI` files or in `YAML`. In most cases, such as the example in the preceding steps, `INI` files are straightforward and easy to read for a small number of managed nodes.

Creating an inventory in `YAML` format becomes a sensible option as the number of managed nodes increases. For example, the following is an equivalent of the `inventory.ini` that declares unique names for managed nodes and uses the `ansible_host` field:

```yaml
myhosts:
  hosts:
    my_host_01:
      ansible_host: 192.0.2.50
    my_host_02:
      ansible_host: 192.0.2.51
    my_host_03:
      ansible_host: 192.0.2.52
```

# Tips for Building Inventories

- Ensure that group names are meaningful and unique. Group names are also case sensitive.
- Avoid spaces, hyphens, and preceding numbers (use `floor_19`, not `19th_floor`) in group names.
- Group hosts in your inventory logically according to their **What**, **Where**, and **When**.

  **What**: Group hosts according to the topology, for example: db, web, leaf, spine.

  **Where**: Group hosts by geographic location, for example: datacenter, region, floor, building.

  **When**: Group hosts by stage, for example: development, test, staging, production.

## Use Metagroups

Create a metagroup that organizes multiple groups in your inventory with the following syntax:

```yaml
metagroupname:
  children:
```

The following inventory illustrates a basic structure for a data center. This example inventory contains a `network` metagroup that includes all network devices and a `datacenter` metagroup that includes the `network` group and all webservers.

```yaml
leafs:
  hosts:
    leaf01:
      ansible_host: 192.0.2.100
    leaf02:
      ansible_host: 192.0.2.110

spines:
  hosts:
    spine01:
      ansible_host: 192.0.2.120
    spine02:
      ansible_host: 192.0.2.130

network:
  children:
    leafs:
    spines:

webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
    webserver02:
      ansible_host: 192.0.2.150

datacenter:
  children:
    network:
    webservers:
```

## Create Variables

Variables set values for managed nodes, such as the IP address, FQDN, operating system, and SSH user, so you do not need to pass them when running Ansible commands.

Variables can apply to specific hosts.

```yaml
webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
      http_port: 80
    webserver02:
      ansible_host: 192.0.2.150
      http_port: 443
```

Variables can also apply to all hosts in a group.

```yaml
webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
      http_port: 80
    webserver02:
      ansible_host: 192.0.2.150
      http_port: 443
  vars:
    ansible_user: my_server_user
```

# Creating a Playbook

Playbooks are automation blueprints, in YAML format, that Ansible uses to deploy and configure managed nodes.

## Playbook

A list of plays that define the order in which Ansible performs operations, from top to bottom, to achieve an overall goal.

## Play

An ordered list of tasks that maps to managed nodes in an inventory.

## Task

A reference to a single module that defines the operations that Ansible performs.

## Module

A unit of code or binary that Ansible runs on managed nodes. Ansible modules are grouped in collections with a Fully Qualified Collection Name (FQCN) for each module.

## Steps to Create a Playbook

Complete the following steps to create a playbook that pings your hosts and prints a “Hello world” message:

1. Create a file named `playbook.yaml` in your `ansible_quickstart` directory, that you created earlier, with the following content:

    ```yaml
    - name: My first play
      hosts: myhosts
      tasks:
       - name: Ping my hosts
         ansible.builtin.ping:

       - name: Print message
         ansible.builtin.debug:
          msg: Hello world
    ```

2. Run your playbook.

    ```sh
    ansible-playbook -i inventory.ini playbook.yaml
    ```

3. Ansible returns the following output:

    ```
    PLAY [My first play] ****************************************************************************

    TASK [Gathering Facts] **************************************************************************
    ok: [192.0.2.50]
    ok: [192.0.2.51]
    ok: [192.0.2.52]

    TASK [Ping my hosts] ****************************************************************************
    ok: [192.0.2.50]
    ok: [192.0.2.51]
   

```
    ok: [192.0.2.52]

    TASK [Print message] ****************************************************************************
    ok: [192.0.2.50] => {
        "msg": "Hello world"
    }
    ok: [192.0.2.51] => {
        "msg": "Hello world"
    }
    ok: [192.0.2.52] => {
        "msg": "Hello world"
    }

    PLAY RECAP **************************************************************************************
    192.0.2.50: ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    192.0.2.51: ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    192.0.2.52: ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ```

## Understanding the Output

- **Names**: The names that you give the play and each task. You should always use descriptive names that make it easy to verify and troubleshoot playbooks.
- **Gathering Facts**: The “Gathering Facts” task runs implicitly. By default, Ansible gathers information about your inventory that it can use in the playbook.
- **Status**: The status of each task. Each task has a status of `ok` which means it ran successfully.
- **Play Recap**: The play recap summarizes results of all tasks in the playbook per host. In this example, there are three tasks so `ok=3` indicates that each task ran successfully.

Congratulations, you have started using Ansible!
```

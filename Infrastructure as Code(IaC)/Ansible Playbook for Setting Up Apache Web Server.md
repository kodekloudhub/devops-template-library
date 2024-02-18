## Ansible Playbook for Setting Up Apache Web Server

This playbook provides a simple yet effective method for configuring an Apache web server on Ubuntu-based systems using Ansible. It's designed for ease of use and quick deployment.

### Playbook Content

Copy the following playbook content into a file named `setup-apache.yml`. Remember to replace `your_web_servers` with your target server group or the individual server where you intend to install Apache.

```yaml
- name: Setup Apache Web Server
  hosts: your_web_servers  # Replace with your server group or individual server
  become: yes
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
        update_cache: yes

    - name: Start Apache and enable on boot
      systemd:
        name: apache2
        enabled: yes
        state: started

    - name: Deploy a basic index.html
      copy:
        content: "<html><body><h1>Hello from Ansible</h1></body></html>"
        dest: /var/www/html/index.html
```

### How to Use

To use this playbook for setting up Apache on your servers, follow these steps:

1. **Prepare Your Inventory**: Ensure your Ansible inventory is correctly set up with the target servers listed under `[your_web_servers]` group or defined appropriately in your inventory file.

2. **Run the Playbook**: Execute the playbook against your servers by running the following command:

    ```bash
    ansible-playbook -i path/to/your/inventory setup-apache.yml
    ```

    Replace `path/to/your/inventory` with the actual path to your Ansible inventory file.

3. **Verify Installation**: After the playbook execution completes, verify that Apache has been successfully installed and is running on your target servers. You can do this by accessing the server's IP address or domain name in a web browser, which should display the "Hello from Ansible" message.

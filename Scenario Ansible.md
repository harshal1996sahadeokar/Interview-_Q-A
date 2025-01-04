Here’s an advanced-level version of the scenario-based Ansible interview questions, complete with answers:


---

Scenario 1: Infrastructure Provisioning

1. Question: How would you provision infrastructure components like VMs, networks, and storage using Ansible?

Answer: Use cloud-specific Ansible modules like ec2, gce, or azure_rm. For instance, to create an AWS EC2 instance, you can use:

- name: Provision EC2 instance
  ec2:
    key_name: my_key
    instance_type: t2.micro
    image: ami-0abcdef1234567890
    region: us-east-1
    count: 2
  register: ec2
- debug:
    var: ec2.instances

This creates two EC2 instances and captures their details in the ec2 variable.



2. Question: How do you ensure idempotency while provisioning resources?

Answer: Use state attributes like present or absent. For example, state: present ensures that the resource is created only if it doesn’t already exist.





---

Scenario 2: Rolling Updates

1. Question: How would you implement a rolling update across servers?

Answer: Use the serial keyword in the playbook to limit the number of servers updated at a time.

- name: Rolling update
  hosts: web_servers
  serial: 2
  tasks:
    - name: Update application
      yum:
        name: my_app
        state: latest

This updates 2 servers at a time before moving to the next batch.



2. Question: How do you handle rollbacks in rolling updates?

Answer: Define a separate rollback task or playbook that reverts the update. Use handlers or conditional logic to trigger the rollback if needed.





---

Scenario 3: Security Compliance Checks

1. Question: How can Ansible enforce security policies?

Answer: Use Ansible playbooks with tasks like checking open ports, disabling unused services, or ensuring SELinux is enabled. Example:

- name: Check SELinux status
  shell: getenforce
  register: selinux_status
- fail:
    msg: "SELinux is not enforced!"
  when: selinux_status.stdout != "Enforcing"



2. Question: How would you perform regular compliance checks?

Answer: Use a combination of playbooks and scheduling tools like cron or AWX to run compliance checks periodically.





---

Scenario 4: Application Deployment

1. Question: How do you handle application dependencies during deployment?

Answer: Use the include_tasks module to handle dependencies. Example:

- name: Deploy dependencies
  include_tasks: dependencies.yml
- name: Deploy application
  include_tasks: deploy_app.yml

This ensures that dependencies are installed before the main application is deployed.



2. Question: How do you validate a deployment?

Answer: Use health-check tasks, such as verifying application endpoints or services. Example:

- name: Verify application
  uri:
    url: "http://{{ inventory_hostname }}/health"
    return_code: 200





---

Scenario 5: Disaster Recovery

1. Question: How would you back up critical data using Ansible?

Answer: Use the synchronize module for backups. Example:

- name: Backup data
  synchronize:
    src: /var/www/html/
    dest: /backup/html/
    archive: true



2. Question: How do you test your disaster recovery playbooks?

Answer: Regularly run DR drills using staging environments to simulate failures and validate recovery processes.





---

Scenario 6: Auto-scaling

1. Question: How would you integrate Ansible with cloud auto-scaling features?

Answer: Use dynamic inventory scripts to fetch auto-scaled instances. Example with AWS:

ansible-playbook deploy.yml -i ec2.py

Here, ec2.py is a dynamic inventory script.



2. Question: How do you ensure configuration consistency during scaling?

Answer: Use playbooks with tags to apply specific configurations only to newly added instances.





---

Scenario 7: Configuration Drift Detection

1. Question: How can Ansible detect and correct configuration drift?

Answer: Use the assert module or write tasks to compare actual vs. expected configurations. Example:

- name: Check configuration file
  stat:
    path: /etc/app/config.yml
  register: config_stat
- fail:
    msg: "Configuration file missing!"
  when: not config_stat.stat.exists



2. Question: How would you schedule drift detection tasks?

Answer: Use cron or AWX to schedule periodic checks.





---

Scenario 8: Multi-tier Application Deployment

1. Question: How do you manage interdependencies between tiers?

Answer: Use serial and role dependencies. Example:

- hosts: db_servers
  roles:
    - db_setup

- hosts: app_servers
  roles:
    - app_setup
  when: db_servers | success



2. Question: How do you handle scaling in multi-tier applications?

Answer: Use dynamic inventory to fetch updated host lists for each tier.





---

Scenario 9: Zero Downtime Deployment

1. Question: How do you achieve zero-downtime deployment with Ansible?

Answer: Implement blue-green or canary deployments. Example:

- name: Deploy to green environment
  hosts: green_servers
  tasks:
    - name: Deploy application
      copy:
        src: app.tar.gz
        dest: /opt/app/

Switch traffic to the green environment after deployment.



2. Question: How do you handle database migrations during such deployments?

Answer: Use conditional tasks to ensure migrations run only once and in a safe order.





---

Scenario 10: Compliance Reporting

1. Question: How do you generate compliance reports using Ansible?

Answer: Use debug or json_query modules to gather and format compliance data. Example:

- name: Collect package info
  yum:
    list: installed
  register: installed_packages
- debug:
    var: installed_packages



2. Question: How would you integrate compliance data with external tools?
.
Answer: Send compliance data to tools like Splunk or ELK using Ansible modules like uri or copy.





---

These answers demonstrate advanced usage of Ansible in real-world scenarios. Let me know if you'd like further clarifications or additional examples!


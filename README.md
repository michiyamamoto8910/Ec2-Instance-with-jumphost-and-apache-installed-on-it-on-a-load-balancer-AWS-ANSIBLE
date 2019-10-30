

Role Name
=========

This is a single play playbook, not using role-based playbook.

Requirements
------------

Virtual machine that runs on Ubuntu or any other linux-based OS with ansible installed on it, preferably latest version. Change the access keys in the variables as well to run, then, supply your own .pem and configure it in the ansible.cfg file. Don't forget to change the permission of .pem file to read only (chmod 400)

Example of editing in ansible.cfg (etc/ansible/ansible.cfg):<br>
[defaults]
host_key_checking = False <br>
private_key_file = /home/ansible-key.pem


Role Variables
--------------

Everything is in the variables.yml file which I included in the main.yml (include_vars)

Example Playbook
----------------
 #CREATE LOAD BALANCER <br>
    - name: create load balancer <br>
      ec2_elb_lb: <br>
        name: "load-balancer" <br>
        instance_ids: "{{ item }}" <br>
        aws_access_key: "{{ aws_access_key }}" <br>
        aws_secret_key: "{{aws_secret_key }}" <br>
        region: "{{ region }}" <br>
        security_group_names: "{{ instance_name_lb }}" <br>
        state: present <br>
        health_check:<br>
            ping_protocol: http # options are http, https, ssl, tcp<br>
            ping_port: 80<br>
            response_timeout: 10<br>
            ping_path: "/index.html"  # seconds<br>
            interval: 30 # seconds<br>
            unhealthy_threshold: 2<br>
            healthy_threshold: 2<br>
        subnets:<br>
          - "{{ websubnet.subnet.id }}"<br>
          - "{{ websubnetforlb.subnet.id }}"<br>
        purge_subnets: true<br>
        listeners:<br>
          - protocol: http<br>
            load_balancer_port: 80<br>
            instance_port: 80<br>
      with_items:<br>
          - "{{ private_ec2_for_lb.instance_ids }}"<br>
          - "{{ private_ec2.instance_ids }}"<br>
          
Author Information
------------------

Michihiro Yamamoto - Technical Consultant at DXC 
myamamoto4@dxc.com

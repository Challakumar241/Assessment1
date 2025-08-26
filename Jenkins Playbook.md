---
- name: Install Jenkins on Ubuntu 20.04
  hosts: jenkins_servers
  become: yes
  tasks:
    - name: Update apt repository cache
      apt:
        update_cache: yes

    - name: Install Java 17
      apt:
        name: openjdk-17-jdk
        state: present

    - name: Install Maven
      apt:
        name: maven
        state: present

    - name: Add Jenkins GPG key
      ansible.builtin.shell: |
        curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | \
        tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
      args:
        executable: /bin/bash

    - name: Add Jenkins repository
      copy:
        dest: /etc/apt/sources.list.d/jenkins.list
        content: |
          deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/

    - name: Update apt repository cache after adding Jenkins repo
      apt:
        update_cache: yes

    - name: Install Jenkins
      apt:
        name: jenkins
        state: present

    - name: Ensure Jenkins service is started and enabled
      service:
        name: jenkins
        state: started
        enabled: yes

    - name: Display Jenkins initial admin password
      command: cat /var/lib/jenkins/secrets/initialAdminPassword
      register: jenkins_password
      changed_when: false

    - name: Show Jenkins initial admin password
      debug:
        msg: "Jenkins Initial Admin Password: {{ jenkins_password.stdout }}"

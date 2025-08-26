---
- name: Assignment 2 - Question 4
  hosts: ansiblegroup
  become: yes

  tasks:
    - name: Update package cache
      apt:
        update_cache: true

    - name: Install Maven
      apt:
        name: maven
        state: present

    - name: Clone the Git repository
      ansible.builtin.git:
        repo: 'https://github.com/Challakumar241/addressbook-cicd-project'
        dest: /home/ubuntu/addressbook

    - name: Run Maven tests
      ansible.builtin.command:
        cmd: mvn test
        chdir: /home/ubuntu/addressbook/

    - name: Compile the project with Maven
      ansible.builtin.command:
        cmd: mvn clean compile
        chdir: /home/ubuntu/addressbook/

    - name: Package the project with Maven
      ansible.builtin.command:
        cmd: mvn package
        chdir: /home/ubuntu/addressbook/

    - name: Download Apache Tomcat
      ansible.builtin.get_url:
        url: https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.108/bin/apache-tomcat-9.0.108.zip
        dest: /home/ubuntu/

    - name: Unzip Apache Tomcat package
      ansible.builtin.unarchive:
        src: /home/ubuntu/apache-tomcat-9.0.108.zip
        dest: /home/ubuntu/
        remote_src: yes

    - name: Find all Tomcat shell scripts
      ansible.builtin.find:
        paths: /home/ubuntu/apache-tomcat-9.0.108/bin
        patterns: "*.sh"
        recurse: no
      register: sh_scripts

    - name: Make Tomcat shell scripts executable
      ansible.builtin.file:
        path: "{{ item.path }}"
        mode: '0755'
        state: file
      loop: "{{ sh_scripts.files }}"

    - name: Start Tomcat using startup.sh
      ansible.builtin.command:
        cmd: bash /home/ubuntu/apache-tomcat-9.0.108/bin/startup.sh

    - name: Deploy addressbook.war to Tomcat
      ansible.builtin.command:
        cmd: cp /home/ubuntu/addressbook/target/addressbook.war /home/ubuntu/apache-tomcat-9.0.108/webapps/
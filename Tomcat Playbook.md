---
- name: install tomcat at node
  hosts: [ansible-kumar-demo]
  become: yes

  tasks:
    - name: update package
      apt:
        update_cache: true

    - name: get tomcat from url
      ansible.builtin.get_url:
        url: https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.108/bin/apache-tomcat-9.0.108.zip
        dest: /home/ubuntu

    - name: get install unzip package
      apt:
        name: unzip
        state: present

    - name: unzip package
      ansible.builtin.unarchive:
        src: /home/ubuntu/apache-tomcat-9.0.108.zip
        dest: /home/ubuntu/
        remote_src: yes

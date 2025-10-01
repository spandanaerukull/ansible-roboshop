
#  catalogue Playbook Explanation
name: configure catalogue component
hosts: catalogue
become: yes


name → A descriptive label for the playbook.

hosts: catalogue → Run this playbook only on the hosts grouped under catalogue in your Ansible inventory.

become: yes → Run tasks with sudo/root privileges, since installing packages, creating users, and managing system files needs elevated access.

1️⃣ Disable default Node.js module
- name: disable default nodejs
  ansible.builtin.command: dnf module disable nodejs -y


You’re disabling the default Node.js module that comes with the OS.

Reason: By default, RHEL/CentOS may provide an older Node.js version. You need to turn it off before enabling the version you want.

Why command module? → Ansible has no module for dnf module disable, so we must use command.

2️⃣ Enable Node.js 20 module
- name: enable nodejs:20
  ansible.builtin.command: dnf module enable nodejs:20 -y


Now you enable the Node.js 20 stream.

This makes Node.js v20 available for installation.

Again, command is used because dnf module does not support module enable.

3️⃣ Install Node.js
- name: install nodejs
  ansible.builtin.dnf:
    name: nodejs
    state: present


# Installs Node.js package.

Why dnf module? → Ansible has a dedicated dnf module for package management, which is idempotent (safe to run multiple times).

4️⃣ Create roboshop system user
- name: create roboshop system user
  ansible.builtin.user:
    name: roboshop
    shell: /sbin/nologin
    system: true
    home: /app


# Creates a system user named roboshop.

shell: /sbin/nologin → This user cannot log in interactively (good for service accounts).

system: true → Marks this as a system user (UID < 1000).

home: /app → Sets /app as the home directory.

5️⃣ Create application directory
- name: create application directory
  ansible.builtin.file:
    path: /app
    state: directory


Ensures /app directory exists.

Why file module? → It can manage directories, permissions, and ownership.

6️⃣ Download catalogue code
- name: download catalogue code...
  ansible.builtin.get_url:
    url: "https://roboshop-artifacts.s3.amazonaws.com/catalogue-v3.zip"
    dest: /tmp/catalogue.zip


Downloads the catalogue service code (zipped) from an S3 bucket.

Why get_url module? → Best for downloading files from HTTP/HTTPS URLs.

7️⃣ Extract catalogue code
- name: extract catalogue code...
  ansible.builtin.unarchive:
    src: /tmp/catalogue.zip
    dest: /app


Extracts the catalogue code into /app.

Why unarchive module? → Automatically handles .zip, .tar, etc. It’s idempotent (won’t extract if already extracted properly).

⚡ Summary

command → used when no Ansible module exists for a job (like dnf module disable/enable).

dnf → for package installation.

user → for creating system users.

file → for creating directories.

get_url → for downloading code/artifacts.

unarchive → for extracting code.

# shell: /sbin/nologin → This user cannot log in interactively (good for service accounts). ididnt understand this line
Normally, a Linux user has a login shell

When you create a user, by default they get a shell like /bin/bash or /bin/sh.

That means they can log in interactively (via SSH or terminal) and type commands.

Example:

useradd testuser -s /bin/bash
su - testuser   # this works, user gets a shell

🔹 /sbin/nologin shell

If you set the shell to /sbin/nologin, then the user cannot log in.

If they try to log in (e.g., su - roboshop), the system will refuse and just display:
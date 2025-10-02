# important ppoints in ansible 
if we use ansible modules we get idempotency other wise we don't get the idempotency for other command or shell modules 

# to check the exit status of the code in ansible we can use register
So the truth is:
# register = captures the whole result of the task.
# Inside that result, rc = exit status.
# example:
- name: Run ls command
  ansible.builtin.command: ls /tmp
  register: result

# Now result will contain:
{
  "changed": true,
  "cmd": ["ls", "/tmp"],
  "rc": 0,                
  "stdout": "file1\nfile2",
  "stderr": ""
}

result.rc → exit status

result.stdout → normal output

result.stderr → error output

# Meaning of rc

rc: 0 → Command ran successfully (exit code 0).

rc: 1 or any non-zero → Command failed.

# replace & inline
  replace = replace only the word or number
  inline = inline means it will replace whole line and weget more controle 

  # why we are using the command for disable and enabling the 
  You are using the ansible.builtin.command module for disabling and enabling the nginx module because Ansible does not have a built-in module to directly manage DNF/YUM module streams (like dnf module enable/disable). The dnf module only installs or removes packages, not module streams.

So, you use the command module to run the shell commands:

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
  replace = replace only the word
  inline = inline means it will replace whole line and weget more controle 
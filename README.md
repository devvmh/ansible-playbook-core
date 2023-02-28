# ansible-playbook-core - core branch

Core ansible playbook for setting up a new server. This is basically just for me.

I am trying to re-use well-written Ansible playbooks where I can; I've started with the debops project since they have a pretty comprehensive set of modules that work well together and are extensible. More roles from debops are available at https://docs.debops.org/en/master/ansible/roles/index.html.

I assume you just spun up a VPS and want to *pull* your configuration from the web, rather than the default Ansible assumption of setting up a server from a controller somewhere.

### List of playbooks in this repo

- core (basic setup): https://github.com/devvmh/ansible-playbook-core/tree/core
- mail-server: https://github.com/devvmh/ansible-playbook-core/tree/mail-server

### Usage:

First, log on as root to a new Debian 11 host. Then install two core packages to get started:

    apt install ansible git

Use `ansible --version` to confirm you are on ansible 2.9+. Now you can pull the playbooks from debops + this repository, which should configure pretty much everything:

    ansible-galaxy collection install debops.debops
    export HOSTNAME=myhostname.mytld.com
    ansible-pull --url https://github.com/devvmh/ansible-playbook-core.git --checkout core \
      --extra-vars "netbase__hostname=${HOSTNAME} sshd__permit_root_login=yes sshd__password_authentication=yes"

Now set the password and you're done:

    passwd devin

Now pull the other playbooks as desired:

    ansible-pull --url https://github.com/devvmh/ansible-playbook-core.git --checkout mail-server

Once you have run this and tested ssh access, you can omit the extra vars; hostname doesn't need to be changed again, and you can drop ssh access to only passwordless access to a non-privileged account.

### Notes for Ubuntu

I tried with Ubuntu 20.04 LTS and this worked. Only wrinkle is I needed to specify `sudo ufw allow 218` for ssh to work.

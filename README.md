# ansible-playbook-core

Core ansible playbook for setting up a new server. This is basically just for me.

I am trying to re-use well-written Ansible playbooks where I can; I've started with the debops project since they have a pretty comprehensive set of modules that work well together and are extensible. More roles from debops are available at https://docs.debops.org/en/master/ansible/roles/index.html.

I assume you just spun up a VPS and want to *pull* your configuration from the web, rather than the default Ansible assumption of setting up a server from a controller somewhere.

### List of playbooks in this repo

- core (basic setup): https://github.com/devvmh/ansible-playbook-core/tree/core
- mail-server: https://github.com/devvmh/ansible-playbook-core/tree/mail-server

### Usage:

First, log on as root to a new Debian 10 host. Then install two core packages to get started:

    apt install ansible git

Use `ansible --version` to confirm you are on ansible 2.9+. If not, visit https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-debian to install. You'll likely want to run these commands (i.e. use bionic or later, not trusty):

    apt install gnupg2
    echo 'deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main' >> /etc/apt/sources.list
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367
    apt update
    apt install ansible

Now you can pull the playbooks from debops + this repository, which should configure pretty much everything:

    ansible-galaxy collection install debops.debops
    export HOSTNAME=myhostname.mytld.com
    ansible-pull --url https://github.com/devvmh/ansible-playbook-core.git --checkout core \
      --extra-vars "netbase__hostname=${HOSTNAME} sshd__permit_root_login=yes sshd__password_authentication=yes"

Now set the password and you're done:

    passwd devin

Now pull the other playbooks as desired:

    ansible-pull --url https://github.com/devvmh/ansible-playbook-core.git --checkout mail-server

Once you have run this and tested ssh access, you can omit the extra vars; hostname doesn't need to be changed again, and you can drop ssh access to only passwordless access to a non-privileged account.

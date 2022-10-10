# ansible-playbook-core - openvpn branch

See https://github.com/devvmh/ansible-playbook-core/tree/master for README with usage and list of branches

You need to set some variables to make this run. Run like so:

    ansible-pull --url https://github.com/devvmh/ansible-playbook-core.git --checkout openvpn \
      --extra-vars "inventory_hostname=$HOSTNAME openvpn_server_ipv6_network=2607:8860::1616:144/120"

For the openvpn branch, you'll need to install dependencies:

    ansible-galaxy collection install ansible.posix community.general
    ansible-galaxy role install kyl191.openvpn

Link to the major dependency role: https://github.com/kyl191/ansible-role-openvpn

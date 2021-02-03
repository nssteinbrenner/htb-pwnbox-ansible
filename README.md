# htb-pwnbox-ansible

Ansible playbook to configure a Parrot Mate host to be as similar to HackTheBox.eu's pwnbox as possible.

Installs most of the packages installed on HTB's pwnbox by default, though not all (some are not in standard  package repositories). Sets up pre-installed pwnbox tools in /opt (e.g, Postman, pycharm-community, etc). Sets up passwordless sudo, and configure's the HTB visual theme.

[![htb-pwnbox-ansible](https://i.imgur.com/Udsh2Cp.png "htb-pwnbox-ansible")](https://i.imgur.com/Udsh2Cp.png "htb-pwnbox-ansible")

You can find the HTB theme files I used here: https://github.com/nssteinbrenner/htb-pwnbox

## Configuration
Copy examples/hosts to inventory/hosts, and edit it to fit your needs. For example:
```
10.10.10.11 ansible_ssh_user=pwnbox_user ansible_password=t0ps3cr3t become=true become_user=root
```
Would have ansible ssh to 10.10.10.11 as the user pwnbox_user, authentication with the password t0ps3cr3t and escalating to root.

Create a directory under inventory/host_vars with the name of your host. Continuing with the example above, we'd make the directory: `inventory/hosts/10.10.10.11`

After creating the directory for your host, copy examples/vars.yml to that directory, and edit it with your user's name and group. If needed, you can change the ansible_python_interpreter value,  but I did not test with python2.

## Usage
You can run the command with all roles with the following:
```
ansible-playbook -i inventory/hosts setup.yml --tags=setup-all
```

Or, you can change the --tags parameter to any of the following to only run individual portions:
- `setup-htb` - Sets up the HackTheBox theme. **Note**: The theme is configured identically to how it is on HTB's pwnbox, meaning it makes assumptions about what is installed. If you do this without installing, the tools in /opt and installing the packages, you will need to do some manual configuration to account for this afterwards.
- `setup-misctools` - Sets up Postman, pycharm and websockify in /opt. You can also install these three individually with `setup-postman`, `setup-pycharm`, and `setup-websockify` respectively.
- `setup-system` - This just modifies the sudoer's file so that members of the sudo group can run sudo without a password. Also adds your user to the sudo group if they are not already in it.
- `setup-repos` - Clones the repos that live under /opt/useful on HTB's pwnbox (SecLists, privilege-esclation-awesome-scripts-suite, PayloadsAllTheThings, linuxprivchecker)
- `setup-packages` - Runs `parrot-upgrade` then installs the packages that are installed on HTB's pwnbox by default. **Note**: `parrot-upgrade` took forever (multiple hours) on my VM after a fresh install from the Parrot Mate Security ISO. Your mileage may vary.

## Contributing
I've never used Ansible, and probably did things poorly or sub-optimally. If you'd like to improve this, it is very much appreciated.

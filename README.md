# Ansible Pi
Ansible playbooks for managing my various raspberry pi's

## Orchestration Environment
Ansible 1.9.1
Python 2.7.9
OSX 10.10.5

## Managed Environment
Raspbian: Linux 3.18.11+ armv6l GNU/Linux

## Pre-steps
- In order to allow ansible to connect without needing passwords it is necessary to copy your public ssh key to authorized-keys file on the pi. Personally I use ssh-copy-id to speed up this task.
- Use nmap to discover the IP of the Pi for your inventory as it will not initially be discoverable via host-name.<br> `nmap -sn 192.168.1.0/24`

## Usage
This project has in its root a site.yml which is a top level playbook which includes all the sub-playbooks that actually set-up various logical groupings of raspberries.

I tend to check what will happen first with something like:<br>
`ansible-playbook site.yml --diff -i inventory/ -s --ask-vault-pass --limit raspberry-2 --check`<br>
Then run it with something like:<br>
`ansible-playbook site.yml --diff -i inventory/ -s --ask-vault-pass --limit raspberry-2`

I will also often run one of the sub-playbooks directly typically becuase I only need to manage one grouping of raspberries at a time and bootstrap.yml in particular is quite slow to run every time due to the forced apt-cache update. I also provided a bootstrap playbook for specifically just running bootstrap which is useful when adding a new device as I have not yet found a way to automate the gui raspi-config step.

I sub-divide further using tags to restrict what actually gets run for example:<br>
`ansible-playbook bootstrap.yml --diff -i inventory/ -s --ask-vault-pass --limit raspberry-2 --tags network`

At present there are two logical groupings. NodeJS server raspberries configured to run NodeJS and not much else; providing a NodeJS playground. Then secondly Comms, which is intended to remain up to support various communication activities such as Mumble and an IRC Bouncer.

## Notes
- Systemd is used as the init system.
- Don't forget to forward the ports for murmur if it should be accessible outside your network.

## Roles
- noip - installs NoIP as a daemon.
- nodejs - sets up a simple NodeJS server.
- murmur - installs a Mumble server.
- bootstrap - sets up the basic system.

## Credits
Credit to https://github.com/jlund/ansible-mumble-server for the basis of the murmur role.

Socrata Gateway Agent
---------------------
Socrata connects to a gateway server in order to pull data from our internal systems into the open data portal.  The gateway agent is installed somewhere in our internal network.  The gateway agent is a java application that gets installed as a linux service daemon.

The gateway installer must be downloaded from our Socrata instance, as it contains the secret keys used to talk to our Socrata open data portal.

## Ansible
The ansible playbook does as much setup as possbile

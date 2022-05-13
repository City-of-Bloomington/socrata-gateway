Socrata Gateway Agent
---------------------
Socrata connects to a gateway server in order to pull data from our internal systems into the open data portal.  The gateway agent is installed somewhere in our internal network.  The gateway agent is a java application that gets installed as a linux service daemon.

The gateway installer must be downloaded from our Socrata instance, as it contains the secret keys used to talk to our Socrata open data portal.

## Ansible
The ansible playbook does as much setup as possbile

```bash
ansible-playbook deploy -i /path/to/inventory
```

## Installing the Gateway service
In Socrata, go to Administration > Gateway and provision a new gateway.  Socrata will have you download the installer for the new Gateway you provision.  Copy the installer onto the server and run it using sudo.

The user running the installer must be the same user that the service will run as.    For the installation, you will need to give the user a password and sudo access.  After the gateway service is running and connected to the open data portal, root access is no longer needed.  So, the password can be deleted and the user removed from the sudo and adm groups.

```bash
sudo ./install
systemd service name: socrata
User to run as: socrata
Directory to run in: /srv/socrata
Data directory: /srv/socrata/data
Java: /usr/bin/java
JAVA_HOME (leave blank if unnecessary):
HTTP proxy (leave blank if unnecessary):
HTTPS proxy (leave blank if unnecessary):
```

All future gateway configuration will need to be done as the socrata user.  The socrata user will always need a shell; /bin/bash is fine.

## Backup and Restore
* Make sure the gateway agent you're migrating is turned off on the source machine.
* Once that is confirmed, you'll run the-control-script run --save-state some-filename . This will create some-filename (which is a placeholder, feel free to name it whatever makes the most sense on your end) containing the complete state of the agent. You'll copy some-filename and the original zip you downloaded to initially install the gateway agent to your new linux deployment.
* Next you'll install the agent on the new linux deployment. You'll do this with the original zip file you copied over.
* The newly installed gateway agent will fail to start up due to bad credentials, which is expected. You'll then want to make sure the newly installed gateway agent is stopped and not in a restart loop. Once that is confirmed, run the-control-script run --restore-state some-filename and start the agent.

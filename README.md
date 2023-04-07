# Deployment

## Requirements

Install packages

    apt install python3 virtualenvwrapper

Optional for testing with vagrant install vagrant and virtualbox.

Place your SSH public key on the servers authorized_keys:

    ssh-copy-id root@hostname

## Setup SSH

Generate a deployment SSH key.

    ssh-keygen -f ~/.ssh/id_rsa_mohe
    ssh-add ~/.ssh/id_rsa_mohe
    
## Setup python virtualenv

    mkvirtualenv ansible
    pip install -U pip setuptools
    pip install -r requirements.txt

## Setup secrets

Copy the file secrets.enc.template to secrets.enc

Edit secrets.enc

**Do not commit secrets.enc to the repository!**

## Recommended: encrypt secrets.enc with ansible-vault

    ansible-vault encrypt secrets.enc

After encryption you can edit secrets.enc with the following command

    ansible-vault edit secrets.enc

## create inventory

Create a hosts file inventory/production/hosts (replace the hostnames)

    [database]
    database.mohe.com

    [admin]
    admin.mohe.com

    [datalab]
    datalab.mohe.com

    [facility]
    facility.mohe.com

    [patient]
    patient.mohe.com


## Bootstrap

Makes sure the mohe user exists on all hosts and you can ssh into the host using the deployment ssh key generated before.
 
    ansible-playbook -i inventory/demo --ask-vault-pass-e "@secrets_demo.enc" --user root bootstrap.yml

## Deployment

    ansible-playbook -i inventory/demo --ask-vault-pass -e "@secrets_demo.enc" --user mohe site.yml

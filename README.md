# SETUP
Please use Ubuntu 20.04 Focal as target host to make sure that everything runs smoothly.

## Setting up local environment
```
python3 -m venv venv
. venv/bin/activate
pip install --upgrade pip
pip3 install ansible==4.6.0
```

## Setting up vault
Create variables file:
```
export VAULT_FILE=roles/unitedforu-bot/vars/vault.yml

echo 'TELEGRAM_API_TOKEN: "<YOUR_TOKEN>"' > $VAULT_FILE
echo 'TELEGRAM_LIST_OF_ADMIN_IDS: "<COMMA_SEPARATED_LIST_OF_TELEGRAM_USER_IDS>"' >> $VAULT_FILE
echo 'GOOGLE_SPREADSHEET_ID: "<YOUR_SPREADSHEET_ID>"' >> $VAULT_FILE
echo 'GOOGLE_RANGE_NAME: "A1:C2"' >> $VAULT_FILE
echo 'GOOGLE_APPLICATION_CREDENTIALS_PATH: "~/.service_account.json"' >> $VAULT_FILE

# How to get a Google service account file (https://developers.google.com/workspace/guides/get-started)
echo 'GOOGLE_APPLICATION_CREDENTIALS_FILE: <GOOGLE_SERVICE_ACCOUNT_JSON>' >> $VAULT_FILE
```

Encrypt file with Ansible vault
```
ansible-vault encrypt $VAULT_FILE
```

Create a file with Ansible vault password
```
echo '<YOUR_VAULT_PASSWORD>' > .password
chmod 600 .password
```

## Setting up inventory
```
cp inventory.template inventory
```
then set HOST_IP in the inventory file as the target host IP.

# OPERATIONS

## Host configuration
```
ansible-playbook -i inventory --diff playbooks/preconfigration.yml
```

## unitedforu-bot container
### Deployment
```
ansible-playbook -i inventory --diff playbooks/unitedforu.yml
```

### Change to a specific state
```
ansible-playbook -i inventory --diff playbooks/unitedforu.yml -e container_state=<STATE>
```
Available stated:

* absent
* present
* started
* stopped

## Ansible Role: Telegram Alerts

![CI](https://github.com/jokerwrld999/ansible-role-telegram-alerts/actions/workflows/ci.yaml/badge.svg)

## Description

Send Telegram Alerts With Ansible.

**Features:**

- **Send Successful Completion Alert**
- **Send Failure Alert With Error Logs**

## Usage

**Requirements**

None.

**Install Role**

```
ansible-galaxy install jokerwrld999.telegram-alerts
```

## Role Variables

Available variables are listed below, along with default values:

**Alerts:**

- `telegram_chat_id`: String, empty by default. Set your Telegram chat ID if
  using Telegram alerts. Also can be set by exporting environment variable.

  Example: `export TELEGRAM_CHAT_ID=<telegram_chat_id>`

- `telegram_token`: String, empty by default. Set your Telegram bot token if
  using Telegram alerts. Also can be set by exporting environment variable.

  Example: `export TELEGRAM_TOKEN=<telegram_token>`

- `task_failed`: Boolean, defaults to `false`. Set to `true` to send failure
  alert otherwise send success alert. Recommended to use with `Blocks` to handle errors and send alerts.

  Blocks Example:

  ```yaml
  tasks:
    - name: Handle the error
      block:
        - name: Print a message
          ansible.builtin.debug:
            msg: "I execute normally"

        - name: Force a failure
          ansible.builtin.command: /bin/false

        - name: Never print this
          ansible.builtin.debug:
            msg: "I never execute, due to the above task failing, :-("
      rescue:
        - name: Telegram Alerts | Rescue
          ansible.builtin.set_fact:
            task_failed: true
  ```

Consider using Ansible Vault or other secure credential management solutions.

**Example Ansible Vault**

You can encrypt variables with `ansible-vault` ad-hoc commands:

```
ansible-vault encrypt_string <password_source> '<string_to_encrypt>' --name '<string_name_of_variable>'
```

OR:

```
ansible-vault encrypt_string --vault-password-file a_password_file 'foobar' --name 'the_secret'
```

Also, to decrypt variables:

```
ansible localhost -m ansible.builtin.debug -a var="telegram_token" -e "@vars/main.yml" --vault-password-file ./.vault_pass
```

1. Create encrypted variable:

   ```
   ansible-vault encrypt_string --vault-password-file .vault_pass '1111111' --name 'telegram_chat_id'
   ```

   Result:

   ```yaml
   Encryption successful
   telegram_chat_id: !vault |
           $ANSIBLE_VAULT;1.1;AES256
           31636361323166623735313934393462383337613263613330623137656339613963353633313265
           3363313265613563626336646132313331343931386235330a363137343665336362613734623437
           61636265366435666262643334656130656262353132663239306632326331636137346334303937
           6631343235306264320a636366313130656631396138383139393737356239323130376637326331
           6437
   ```

2. Compose variables file `main.yml`:

   ```yaml
   ---
   # Telegram Alerts
   telegram_chat_id: !vault |
     $ANSIBLE_VAULT;1.1;AES256
     31636361323166623735313934393462383337613263613330623137656339613963353633313265
     3363313265613563626336646132313331343931386235330a363137343665336362613734623437
     61636265366435666262643334656130656262353132663239306632326331636137346334303937
     6631343235306264320a636366313130656631396138383139393737356239323130376637326331
     6437
   telegram_token: !vault |
     $ANSIBLE_VAULT;1.1;AES256
     31303231653237663464363238643830613636323639373635306636353338643438383437646136
     3462346630323236366633616234323264636431376438320a643539386263346563373265663963
     62343134353131313637633034656334396339333532653839356565636532316337633764633139
     3564313664326165630a323732613765353434616531656461333633376639626131333933643833
     6564
   ```

3. Finally, include variables file in your playbook.

**Example Playbook:**

```yaml
- name: Telegram Alerts | Send alerts
  tags: always
  hosts: all
  vars_files:
    - main.yml
  vars:
    telegram_chat_id: "your_chat_id"
    telegram_token: "your_bot_token"
  tasks:
    - name: Include telegram-alerts role
      ansible.builtin.include_role:
        name: jokerwrld999.telegram-alerts
```

## Dependencies

None.

## License

MIT / BSD

## Author Information

This role was created in 2024 by [Joker Wrld](https://docs.jokerwrld.win/).

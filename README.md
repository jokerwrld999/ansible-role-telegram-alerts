## Ansible Role: Telegram Alerts

Send Telegram Alerts With Ansible.

**Features:**

- **Send Successful Completion Alert**
- **Send Failure Alert With Error Logs**

## Requirements:

None.

## Role Variables

Available variables are listed below, along with default values:

**Alerts:**

- `task_failed`: Boolean, defaults to `false`. Set to `true` to send failure
  alert otherwise send success alert.

- `telegram_chat_id`: String, empty by default. Set your Telegram chat ID if
  using Telegram alerts.

- `telegram_token`: String, empty by default. Set your Telegram bot token if
  using Telegram alerts.

Consider using Ansible Vault or other secure credential management solutions.

**Example Playbook:**

```yaml
- hosts: all
  roles:
    - jokerwrld999.telegram-alerts

  vars:
    task_failed: true
    telegram_chat_id: "your_chat_id"
    telegram_token: "your_bot_token"
```

## Dependencies

None.

## License

MIT / BSD

## Author Information

This role was created in 2024 by [Joker Wrld](https://docs.jokerwrld.win/).

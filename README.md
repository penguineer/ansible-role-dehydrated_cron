# Ansible Role: dehydrated_cron

[![Molecule](https://github.com/penguineer/ansible-role-dehydrated_cron/actions/workflows/molecule.yml/badge.svg)](https://github.com/penguineer/ansible-role-dehydrated_cron/actions/workflows/molecule.yml)

> Install systemd timers to run dehydrated and only send e-mails in case of changes.


## Usage


### Configuration

Please note that all variables have default values and usually do not need to be changed.

* `dehydrated_cron_script_dir`: Path to the helper scripts
* `dehydrated_config_dir`: Path to the Dehydrated configuration
* `dehydrated_certs_dir`: Path to the Dehydrated certificates
* `dehydrated_binary`: Path to the Dehydrated binary
* `dehydrated_check_span`: Time (in seconds) before expiration for a certificate to be reported as stale.
* `dehydrated_timer_renew`: `OnCalendar` expression for the renewal timer (default: `*-*-* 04:00:00`)
* `dehydrated_timer_check`: `OnCalendar` expression for the expiry-check timer (default: `*-*-* 05:00:00`)
* `dehydrated_mail_to`: Optional e-mail address to receive notifications when certificates are renewed or about to expire. Leave empty (default) to disable e-mail. Requires a working MTA (e.g. `mailutils`) to be installed on the host.
* `dehydrated_mail_subject_prefix`: Subject prefix used in notification e-mails (default: `[dehydrated]`)

The configuration variables are compatible with [24367dfa/ansible-role-dehydrated](https://github.com/24367dfa/ansible-role-dehydrated).

> **Migration note:** The former `dehydrated_cron_renew` and `dehydrated_cron_check` cron-format variables have been replaced by `dehydrated_timer_renew` and `dehydrated_timer_check`, which accept systemd `OnCalendar` expressions.


### Include

as role:

```yaml
vars:
  - dehydrated_…: …

roles:
  - role: penguineer.dehydrated_cron
```

as task:

```yaml
tasks:
  - name: Setup systemd timers for dehydrated
    include_role:
      name: penguineer.dehydrated_cron
    vars:
      dehydrated_…: …
```

### E-mail notifications

Set `dehydrated_mail_to` to an e-mail address to receive notifications:
- when certificates are **renewed** (i.e. dehydrated produces more output than expected for a no-change run)
- when **dehydrated exits with an error**
- when certificates are **about to expire** (detected by the check timer)

A working MTA such as `mailutils` must be installed on the host.

```yaml
vars:
  dehydrated_mail_to: "admin@example.com"
  dehydrated_mail_subject_prefix: "[dehydrated]"
```

### Cleanup

If you had dehydrated running before, make sure to disable any pre-existing crontab entries.
The role automatically removes `/etc/cron.d/dehydrated` if it exists.

## Maintainers

* Stefan Haun ([@penguineer](https://github.com/penguineer))


## Contributing

PRs are welcome!

If possible, please stick to the following guidelines:

* Keep PRs reasonably small and their scope limited to a feature or module within the code.
* If a large change is planned, it is best to open a feature request issue first, then link subsequent PRs to this issue, so that the PRs move the code towards the intended feature.


## License

[MIT](LICENSE.txt) © 2022 Stefan Haun and contributors

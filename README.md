# ansible-borgbackup

Install and configure BorgBackup on my home network.

If an already-installed machine is changed from pip to OS package the symlinks in /usr/local/bin will need to be removed.

## Install

### Requirements

* community.crypto - for ssh key generation. This brings in other Python module requirements, which is a headache on older systems.

### requirements.yml Entry

```yaml
roles:
  - name: borgbackup
    src: https://github.com/mkheironimus/ansible-borgbackup.git
```

## On the client

Role variables to set:

```yaml
borgbackup_repo: "borg@SERVERHOSTNAME:system"
borgbackup_daily_time: HOURTORUNBACKUPS
borgbackup_backups:
  system:
    path: /
    exclude:
      - /some/path
    compression: zstd
    daily: 7
    weekly: 4
  specialbackup:
    path: /some/path
    compression: none
    daily: 3
```

Update the server with the new key (see below).

Put the password in `BORG_BASE_DIR/passphrase`. Generate it with pwgen or similar on a new client.

Run `/srv/borg/borginit.sh` to set up the new repository.

Copy the exported key from `BORG_BASE_DIR/repo.key` to somewhere safe.

## On the server

Role variables to set:

```yaml
borgbackup_client_keys:
  - path: /path/to/client/repository
    key: ssh-rsa KEYDATA borg@clienthost
```

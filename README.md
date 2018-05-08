# 

You know what would be cool? Syncing SSH keys from other services to auth yo' self.

## Sources
- AWS (IAM)
- Github
- Digital Ocean
- Bitbucket
- Azure
- Heroku
- Vault

## Credentials


## Providers

### Digitalocean

#### Teams 
Switch to the context of your team and create a new API token
https://cloud.digitalocean.com/settings/api/tokens/new

Name the token `ssh-access` and make the role readonly

#### Personal
The steps are similar to teams

## Provider Contract
```yaml
interval: 10 # Number of minutes between syncs
require_mfa: true # Ensure that account has mfa enabled

github:
  organizations:
    - Netflix
    - SnakesOn*
  users:
    - admin*
  groups:
    - developers

aws:
  organizations:
    - o-p78kesmg8l
  accounts:
    - 141084547154

digitalocean:
  teams:
    whales
  roles:
    member
```

### get_users()
`Array.<{uid: String, username: String, org: String, src: String}>`

You can pass an organization or list of users, or a list of user patterns


### get_groups(username: String)
`Array.<{uid: String, groupname: String, src: String}>`

For each user get the roles/groups they belong to


### get_keys(username: String)
`Array.<{uid: String, keybody: String, format: String, src: String}>`


A statefile is generated with the data & actions that occurred during synchronization

```json
{
  "config": {
    "interval": 1,
    "github": {
      "organizations":["Netflix", "SnakesOn*"], "users":[], "groups":[]
    }
  },
  "users": [
    {"uid":"6d1ff4c9", "username":"jane", "org":"Netflix", "src":"github", "groups": []}
  ],
  "keys": [
    {"uid":"943c5fbd", "username":"jane", "keybody":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC/...", "format":"open-ssh", "src":"github"}
  ],
  "log": [
    {"action":"removed", "entity":"key", "uid":"...", "username":"jane"},
    {"action":"added", "entity":"group",  "uid":"...", "username":"jane"},
    {"action":"added", "entity":"key", "uid":"..." , "username":"jane"}
  ],
  "timestamp": "2018-06-12 02:12:245"
}
```


## References
- https://github.com/trevoro/sshauth
- https://github.com/hashicorp/vault-ssh-helper
- https://github.com/widdix/aws-ec2-ssh
- http://keymaker.readthedocs.io/en/latest/
- https://github.com/Netflix/bless
- https://gravitational.com/teleport/

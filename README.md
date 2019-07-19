# nft-firewall

Simple firewall based on nft and ufw base ruleset.

Copy all files to `/etc/firewall/`, copy `user.ruleset.EXAMPLE` to `user.ruleset` and edit as you want, then run `/etc/firewall/firewall install`.

* `firewall enable` will check and apply all rules
* `firewall disable` will flush all rules
* `firewall logging XXXX` will configure logging accordingly to log-XXX.ruleset files

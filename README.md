# unite, p.k.a. nft-firewall

Simple nftables firewall based on UFW ruleset.

This implementation will not create rules for your or anything like that, you have to manage your own ruleset file, it will just manage default ruleset(Derived from UFW) and logging ones.

## Requirements

To use the `install` command, your system must use `systemd` as init.

## Installation

To install, clone repo on /etc/, create a user.ruleset(The example ruleset _allow_ SSH connection on all interfaces) file and run ./unite install:

```
pushd /etc/
git clone https://github.com/0x3333/unite.git
pushd unite
cp user.ruleset.EXAMPLE user.ruleset
./unite install
```

## Configuration

All user rulesets must be provided in the `user.ruleset` file in the `unite` folder. The syntax is the same as nft config files.

You can override default file locations using `/etc/default/unite`:

```
RULESET_FOLDER = "/some/custom/location/unite"
MAIN_RULESET   = "${RULESET_FOLDER}/main.ruleset"
SYSCTL_FILE    = "${RULESET_FOLDER}/sysctl.conf"
SYSTEMD_FILE   = "${RULESET_FOLDER}/unite.service"
```

## Commands

| Command         | Description                                               |
| --------------- | --------------------------------------------------------- |
| `unite start`   | will check and apply nftables rules and sysctl properties |
| `unite stop`    | will flush all rules and sysctl properties                |
| `unite enable`  | will check and apply all rules and enable on bootup       |
| `unite disable` | will flush all rules and disable at startup               |
| `unite status`  | will show nftables status                                 |
| `unite list`    | will list all nftables rulesets                           |
| `unite logging` | will set logging level                                    |

## Logging

`unite` supports multiple logging levels. `unite` defaults to a loglevel of 'low' when a loglevel is not specified. Users may specify a loglevel with:

```
unite logging LEVEL
```

Log levels, `LEVEL`, are defined as:

| Level    | Description                                                                                                                                                      |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `off`    | disables `unite` managed logging                                                                                                                                 |
| `low`    | logs all blocked packets not matching the defined policy (with rate limiting), as well as packets matching logged rules.                                         |
| `medium` | log level low, plus all allowed packets not matching the defined policy, all `INVALID` packets, and all new connections. All logging is done with rate limiting. |
| `high`   | log level medium (without rate limiting), plus all packets with rate limiting/                                                                                   |

Log levels above `medium` generate a lot of logging output, and may quickly fill up your disk. Log level medium may generate a lot of logging output on a busy system.

Specifying '`on`' simply enables logging at log level '`low`' if logging is currently not enabled.

## Chains

This is the graphical representation of the `unite` chains.

![INPUT Chain](https://raw.githubusercontent.com/0x3333/unite/master/.github/input-chain-diagram.png)

![OUTPUT Chain](https://raw.githubusercontent.com/0x3333/unite/master/.github/output-chain-diagram.png)

![FORWARD Chain](https://raw.githubusercontent.com/0x3333/unite/master/.github/forward-chain-diagram.png)

## License

```
Copyright 2019 Tercio Gaudencio Filho

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

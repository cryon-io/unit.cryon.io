# About

[ANS (Autonomous Node System)](https://github.com/cryon-io/ans) is open source AGPL licensed shell script. 

It is designed automate and simplify node deployment and management. 

## Features

- modular and transparent - open source
- simple setup and management
- auto updates to both AMS and the node software (speeds up and simplifies node network upgrades)
- health checks with auto-heal to maximize uptime
- simplifies development (developers can target and test specific OS while AMS provides support for main server side OSes)
- security - all modules are audited (does not include audit of node code)

## Installation

1.  - `git clone "https://github.com/cryon-io/ans.git" [path] && cd [path] && chmod +x ./ans` # replace path with directory you want to store node in
   
    or  
    - `wget https://github.com/cryon-io/ans/archive/master.zip && unzip -o master.zip && mv ./ans-master [path] && cd [path] && chmod +x ./ans`
1. one of commands below depending of your preference (run as *root* or use *sudo*)
    - `./ans --full --node=<%NODE_TYPE%>` # full setup of Ether1 SN/MN for current user
    - `./ans --full --user=[user] --node=<%NODE_TYPE%> --auto-update-level=[level] <%ANS_ADDITIONAL_PARAM%>` # full setup of <%NODE_TYPE%> for defined user (directory location and structure is preserved) sets specified auto update level (Refer to [Auto updates](https://github.com/cryon-io/ans/wiki/Auto-Updates))

## Structure

- `ans`                             # `ans` core script - management and setup, for usage check Help
- `is-up`                           # `is-up` script - used to check whether node is running or not
- `cli`                             # `cli` script - wrapper for direct access to node cli
- `_ans_methods`                    # methods used by ans
- `node.info`                       # last retrieved node.info by `./ans -i` command
- `supported_nodes.json`            # list of supported nodes with links to repositories
- `state/`                          # contains configured parameters, env variables, configuration other ans state specifics
- `containers/`                     # contains current and past nodes configured by ANS
    - `[NODE_TYPE]`                 # contains node repository with node specific configuration
                                    # for node repository folder structure check wiki of node you are interested in

## Manual Updates

```sh
./ans --update-node       # updates node binaries
./ans --update-service    # updates service definition
./ans --update-ans        # updates ANS
```

## Configuration

Refer to nodes for configuration details.

### Auto Updates

```sh
./ans --auto-update-level=[level]   # enables auto updates based on level
                                    # 0 - node binary updates only
                                    # 1 - binary and service updates
                                    # 2 - above + ANS updates
./ans --disable-auto-update         # disables auto updates
``` 

### Multi Node Setups & Caveats 

Multi application setups may require additional configuration adjustment in case of applications of your choice uses same ports. 

In that case you can add binds into application template which specifies how should underlaying system treat ports. 

Bind format: `--bind=<ip>:<host_port>:<app_port_to_map>`
Example: `--bind=192.168.1.1:443:8080	`

**You have to specify `--project-name=<name>` to setup multiple nodes of same type.**
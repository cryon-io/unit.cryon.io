# Setup XEROM Nodes

Available setup options are Frontier (easiest), Unit (medium), ANS (for advanced users only). 

## Unit Frontier Setup 

1. [Setup Node](https://nodes.xerom.org/index.html)

## Unit Core Setup 

1. Install [Unit Core](/docs/Core/README?id=installation)
2. Edit file `/etc/unit/unit.json` with text editor of your choice
    ```json
    {
        "logLevel": 0,
        "maintenance": {
            "wait_for_sync":true,
            "sync_timeout": "20m",
            "auto_docker_prune": "all",
            "unit_auto_update": true
        },
        "nodes": [
        {
            "id": "<nodeid>",
            "type": "<node type>",
            "parameters": [ 
                "STAKE_ADDR=<stake_addr>", 
                "IP=<public ip>"
                ],
            "auto_updates": "node",
            "binds": [ "<public ip>:30306:30305" ]
        }
        ]
    }
    ```
    - Set `<nodeid>` to id you would like to identify node with. E.g. `xero1` (Spaces are not allowed)
    - Set `<node type>`. Available XEROM node types are:
        * `XERO_CN` - Chain Node
        * `XERO_XN` - Xero Node 
        * `XERO_LN` - Link Node
        * `XERO_SN` - Super Node
    - Replace `<stake_addr>`with address which you use as collateral address.
    - If your machine **has public ip (NOT PORT FORWARDED)** - **Replace** `<public ip>` with actual public IP address of your machine.
    - If your host machine **uses port forwarding of any kind** - set IP to **empty** `"IP="` and binds to `[ "30306:30305" ]`
3. Run `unit setup <node id>`
4. Run `unit start <node id>`
5. Check node is working properly with `unit i`
    - `status` should say `Up`
    - `mn_status` should say `Not Found`
    - block_number is increasing
6. Get your enodeid with `/mns/<node id>/containers/<node type>/enodeid`
7. Activate your node with enodeid you got based on [Node Activation](https://nodes.xerom.org/index.html). (Skip over the Frontier setup part directly to node activation in web wallet)
8. Check node is working properly with `unit i`
    - `mn_status` should say `Active`
*In case your mn_status is not active verify whether you registered right enodeid and whether you set right STAKE_ADDR with `/mns/<node id>/containers/<node type>/dashboard`.*

## ANS Setup 

1. Install [ANS](/docs/Core/AMM/ANS/README?id=installation)
2. Change cwd to ANS install directory.
3. Setup node with `sh ./ans --full --type=<node type> -sp="STAKE_ADDR=<stake_addr>"`
    Available XEROM `<node type>`s are:
    * `XERO_CN` - Chain Node
    * `XERO_XN` - Xero Node 
    * `XERO_LN` - Link Node
    * `XERO_SN` - Super Node
4. Start node `./ans --start`
5.  Check node is working properly with `./ans -i`
    - `Status` should say `Up`
    - `mn_status` should say `Not Found`
    - block_number is increasing
6. Get your enodeid with `<cwd>/containers/<node type>/enodeid`
7. Activate your node with enodeid you got based on [Node Activation](https://nodes.xerom.org/index.html). (Skip over the Frontier setup part directly to node activation in web wallet)
8. Check node is working properly with `./ans -i`
    - `mn_status` should say `Active`
*In case your mn_status is not active verify whether you registered right enodeid and whether you set right STAKE_ADDR with `/mns/<node id>/containers/<node type>/dashboard`.*
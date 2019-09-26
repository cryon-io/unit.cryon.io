# Setup SnowGem Masternode

Available setup options are: 
- [Unit (medium)](/docs/Core/AMM/ANS/Templates/XEROM/Setup?id=unit-core-setup).

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
        "global": {
            "environment": [ ],
            "parameters": [
                "txindex=<tx_index>",
                "bootstrap=https://files.equihub.pro/snowgem/blockchain_index.zip"
            ],
            "auto_updates": "all"
        },
        "nodes": [
        {
            "id": "<node_id>",
            "type": "XSG_MN",
            "path": "/mns/<folder_name>",
            "parameters": [
                "ip=<ip>",
                "nodeprivkey=<node_private_key>",
                "txid=<tx_id>"
            ],
            "binds": [ "<ip>:16113:16113" ],
            "user": "<user>"
        }
            ]
    }
    ```
    - Set `<tx_index>` to the `Index` of the Masternode you got from **Modern Wallet**
    - Set `<node_id>` to id you would like to identify node with. E.g. `Snow1` (Spaces are not allowed)
    - Set `<folder_name>` to the name of the folder you would like to store your node. E.g. `XSG_MN_1` (Spaces are not allowed)
    - Set `<ip>` to the public IP address of your machine
    - Set `<node_private_key>` to the `Private Key` of the Masternode you got from **Modern Wallet**
    - Set `<tx_id>` to the `Transaction ID` of the Masternode you got from **Modern Wallet**
    - Set `<user>` to the user under which to setup the node. E.g. `snowgem` (Spaces are not allowed)
3. Run `unit setup <node_id>`
4. Run `unit start <node_id>`
5. Check node is working properly with `unit i`
    - `status` should say `Up`
    - `mn_status` should say `Hot node, waiting for remote activation`
    - block_number is increasing
6. Wait till your node is synced.
7. [Activate Node](Node Activation.md)
8. Check node is working properly with `unit i`
    - `mn_status` should say `Masternode successfully started`

_If you have any questions please contact us on [Discord](https://discord.gg/zumGnbg) or [Telegram](https://t.me/snowgemofficial)._

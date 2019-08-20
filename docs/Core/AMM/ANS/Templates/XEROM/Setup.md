# Setup XEROM Nodes

Available setup options are: 
- [Frontier (easiest)](/docs/Core/AMM/ANS/Templates/XEROM/Setup?id=unit-frontier-setup),
- [Unit (medium)](/docs/Core/AMM/ANS/Templates/XEROM/Setup?id=unit-core-setup), 
- [ANS (for advanced users only)](/docs/Core/AMM/ANS/Templates/XEROM/Setup?id=ans-setup). 

## Unit Frontier Setup 

1. [Install Unit Frontier](/docs/Frontier/README?id=setup) into some directory on your computer. 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/frontier-folder.png" alt="Frontier install folder" /></div>

2. When opening Frontier for the first time, you will have to create a username and password that will be used to log into the application upon each launch.  **These credentials are ONLY stored locally on your computer.**
3. Once logged into Frontier, add your server by navigating to **Servers > New Server** 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/frontier-new-server.png" alt="Add New Server" /></div>

  1. Give your server a name and fill out the root OR sudo-enabled credentials.  You also have the ability to use certificates to authenticate (expert level) 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/new-server-details.png" alt="New Server Details" /></div>

4. After having added your VPS into Frontier, you can now install Unit on it by navigating over to **Servers > (Server Name) > … > Reinstall Unit** (note: you can also install unit from the main server menu, to the right-most of the server name) 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/reinstall-unit.png" alt="Reinstall Unit" /></div>

5. Add node by navigating to **Servers > (Server Name) > New Node** 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/new-node.png" alt="New Unit Node" /></div>

  1. Fill out the **Node ID**, select your preferred **node type** and select **ADD** 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/new-node-select.png" alt="New Node Selection" /></div>

6. Once your node is added, you will need to configure a few parameters like IP address, port binds and your stake address for status tracking purposes. Follow the instructions below if you use a VPS with multiple public IP addresses attached directly to the VPS network interface. If you happen to use NAT behind a single public IP address (home setup), contact someone from the Xerom or Ether-1 teams to help you out.
  1. First we will add the **IP address** parameter by navigating over to **Servers > (Server Name) > (Server Name) > Paremeters > Add** and fill out your IP address in the format seen below `IP=Public_Address_Here`
  2. If you already have a STAKE_ADDR=XXXXXXXX set, you can edit it using the edit icon and inputting your wallet’s public address.  If you don’t have the STAKE_ADDR= parameter you can add it using the ADD button. The final Parameters section should look like below. (note: Neither parameter has any empty spaces in the string) 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/unit-node-parameters.png" alt="Node Parameters" /></div>

  3. Now you will need to set the binding of the ports need for the public address you used. Expand the Binds section and add the bind using the following format `Public_Address_Here:30306:30305` 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/new-bind.png" alt="New Node Bind" /></div>

7. You’re ready to push the configuration over to your VPS by using the **Push Configuration** button. Pushing configuration should only be done when making changes to your node parameters. 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/push-configuration.png" alt="Push Unit Configuration" /></div>

8. Once the configuration is finished pushing, you’re ready to setup the Xerom node on the VPS by hitting the **… > Setup** button 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/setup-node.png" alt="Setup Node" /></div>

9. Node setup will take around 5-15 minutes depending on your VPS specifications. When setup is finished, you can start the node as shown below, by using the **… > Start** button 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/start-node.png" alt="Start Unit Node" /></div>

10. You can monitor the node startup & sync status by using the refresh button and monitoring the block height in the Stats area (both shown below) 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/node-refresh.png" alt="Refreshing Node Status" /></div>

11. A properly added and synchronized node will look like the one below 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/synched-node.png" alt="Synced Node" /></div>

12. Before heading on over to register your node, you will need to grab your ENODE id.
  1. You can retrieve your ENODE ID by using Frontier’s SSH capability. 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/unit-ssh.png" alt="Unit Server SSH" /></div>

  2. Navigate over to the /mns/(NODE_ID)/containers/(NODE_TYPE) directory and execute the ./enodeid app (note: the () fields are going to be replaced with your unique information, depending on your previous selections. If you're just dealing with a single node, hitting the TAB key once at the () level will auto-type your node name (or node type) for you. If hitting the TAB key once doesn't produce a result, hit it twice to display the contents of the directory) 
<div class="center-content-horizontaly"><img src="/assets/templates/xero/frontier/enode.png" alt="Retrieving ENODE id" /></div>

13. Now you’re ready to activate the node using the information from the STAKE_ADDR and the web wallet over at https://wallet.xerom.org
14. [Activate Node](/docs/Core/AMM/ANS/Templates/XEROM/Node%20Activation?id=activate-xerom-nodes)
15. Refresh node status to confirm it has been activated.

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
6. Get your enodeid with `/mns/<node id>/containers/<node type>/enodeid` and save it for later - needed for node activation
7. [Activate Node](/docs/Core/AMM/ANS/Templates/XEROM/Node%20Activation?id=activate-xerom-nodes)
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
6. Get your enodeid with `<cwd>/containers/<node type>/enodeid` and save it for later - needed for node activation
7. [Activate Node](/docs/Core/AMM/ANS/Templates/XEROM/Node%20Activation?id=activate-xerom-nodes)
8. Check node is working properly with `./ans -i`
    - `mn_status` should say `Active`
*In case your mn_status is not active verify whether you registered right enodeid and whether you set right STAKE_ADDR with `/mns/<node id>/containers/<node type>/dashboard`.*
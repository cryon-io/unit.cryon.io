```sh
            `
            ======= Unit Core =======

    about                                           # prints about message
    help                                            # prints this help

    configure-swap                                  # configures swap to GB based on 'swap' value in unit.json
                                                    # runs automatically on setup
    
    prune-docker                                    # removes dangling docker images
    prune-docker all                                # removes all unused docker images

    # Multi app commands
    list                                            # lists configured apps in unit.json
    setup                                           # setups all apps in unit.json
    setup [appId] [appId]                           # setups selected apps from unit.json
    start                                           # starts all apps in unit.json
    start [appId] [appId]                           # starts selected apps from unit.json
    full-update                                     # updates all apps in unit.json
                                                    # performs full updates, ignores update settings in unit.json
    full-update [appId] [appId]                     # updates selected apps from unit.json
                                                    # performs full updates, ignores update settings in unit.json
    update-app                                      # updates all apps in unit.json
                                                    # update-app tolerates app specific auto-update settings in unit.json 
                                                    - performs docker prune automatically, if set in unit.json through 'auto_docker_prune': 'basic|all'
    update-app [appId] [appId]                      # updates selected apps from unit.json
                                                    # update-app tolerates app specific auto-update settings in unit.json 
                                                    - performs docker prune automatically, if set in unit.json through 'auto_docker_prune': 'basic|all'
    restart                                         # restarts all apps in unit.json
    restart [appId] [appId]                         # restarts selected apps from unit.json
    stop                                            # stops all apps in unit.json
    stop [appId] [appId]                            # stops selected apps from unit.json
    
    i|info|information|details                      # prints stats about all apps from unit.json 
    i|info|information|details [appId] [appId]      # prints stats about selected apps from unit.json   
    
    prune                                           # deletes service and data (chain, wallet, etc.) of all apps from unit.json 
    prune [appId] [appId]                           # deletes service and data (chain, wallet, etc.) of selected apps from unit.json 
    prune-data                                      # deletes data (chain, wallet, etc.) of all apps from unit.json 
    prune-data [appId] [appId]                      # deletes data (chain, wallet, etc.) of selected apps from unit.json 
    reset-app                                       # resets all apps from unit.json = 'stop' + 'prune-data' + 'setup' + 'start'
    reset-app [appId] [appId]                       # resets selected apps from unit.json = 'stop' + 'prune-data' + 'setup' + 'start'

    # Single app commands
    [appId] setup                                  # setups specific app from unit.json
    [appId] start                                  # starts specific app from unit.json
    [appId] update                                 # updates specific app from unit.json
    [appId] update-app                             # update-app of specific app from unit.json
                                                    - performs docker prune automatically, if set in unit.json through 'auto_docker_prune': 'basic|all'
    [appId] restart                                # restarts specific app from unit.json
    [appId] stop                                   # stops specific app from unit.json

    [appId] i|info|information|details             # prints stats about specific app from unit.json
    [appId] ic|info-compact                        # prints compact stats about specific app from unit.json
    
    [appId] prune                                  # deletes app service and data (chain, wallet, etc.)
    [appId] prune-data                             # deletes app data (chain, wallet, etc.)
    
    [appId] ans <arguments>                        # passes arguments to ans for specific app from unit.json
    [appId] cli <arguments>                        # passes arguments to cli for specific app from unit.json

    # Updates               
    update-unit                                     # updates unit binary
    auto-update [status|apply]                      # prints whether auto updates (AU) enabled | applies auto-update based on unit.json  
                                                    # is automatically enabled on setup, if unit.json does not contain 'auto_update':'false'
                                                    # if enabled automatically runs:
                                                    - "update-app" on all apps from unit.json
                                                    - unit binary updates (can be disabled with 'unit_auto_update':'false' under 'auto_update' object)
    status-report                                   # generates status report from machine (for troubleshooting purposes)

`       )
    },
```
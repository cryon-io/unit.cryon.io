# AMM Standard 
## Executables

- `amm`   # management binary

## AMM Properties

```json
{
    "additional_parameters" : [
        { "template": "--no-cache", "value": false, "multiple": false },
        { "template": "--bind", "value": true, "multiple": true },
        { "template": "--external-source", "value": true, "multiple": false }
    ]
}
```
## AMM command line interface

```sh
--help                          # Prints help
--setup-dependencies            # Setups dependencies required for application
--build                         # Builds application an prepares environment
--type                          # Sets type of application
--app-source                    # path to application source (for unofficial types)
--start                         # Starts application
--stop                          # Stops application
--restart                       # Restarts application
--update-app                    # Updates application
--update-template               # Updates application template
--update-amm                    # Updates application manager module
--app-id=<app-id>               # Sets application id (Required for single type multi application setups)
--set-file-permissions          # Sets required files permissions (Only inside folder structure of application)
--set-parameter=<parameter>     # Sets parameter of application (Refer to application manual)
--rm-parameter=<parameter>      # Removes single parameter
--clean-parameters              # Cleans all parameters
--set-envvar=<envvar>           # Sets env variable of application (Refer to application manual)
--rm-envvar=<envvar>            # removes single environment variable
--clean-envvars                 # Removes all environment variables
--prune-app                     # Removes application (keeps AMM)
--prune-app-data                # Removes application data (keeps AMM and template)
--status                        # Prints application status
--about                         # Prints "About" the configured application 
                                # - includes parameters and envvar configured 
                                # - includes information you want to show to user of the template
--cli                           # Executes command with application specific cli if aplicable                    
--is-running                    # if running exits with 0 and prints true
                                # else exits with 1 and prints false
--backup=<dest-dir>             # Backups application 
                                # * Requires confirmation if destination file/folder already exists
--restore=<source-dir>          # Restores application      
                                # * Requires confirmation
--reset                         # Resets application to default state (stop, prune-app-data, setup, start)
                                # * Requires confirmation
--remove                        # Removes application from AMM 
                                # * Requires confirmation
```
## Common AMM Template Hooks

AMM Hooks are application specific operations which are called on specific events. They have to be defined in def.json in application template root as `"hooks"` with hook name as key and value as path to executable or script to execute with this hook.

Available hooks:
- `on-start`    - executes before start of the application
    - if exists with non zero code the start is interrupted 
- `on-started`  - executes after application start
- `on-stop`     - hook to ensure application exists gracefully if required
- `on-updated`  - hook to perform post update routines
    - if exists with non zero code the update failed 

## Common AMM Template Methods

AMM Methods provides applications way to act based on AMM commands application specific way. They have to be defined in def.json in application template root as `"methods"` with method name as key and value as path to executable or script to execute with this method.

Available methods:

- `start`           - ability to run application specific start
- `stop`            - ability to run application specific stop

- `fs-permissions`  - configures application specific fs permissions
- `setup`           - ability to run application specific setup

- `remove`          - ability to run application specific removal
- `reset`           - executes when there is requested application 

- `update`          - performs application specific operations required to update of application
    - if exists with non zero code the start is interrupted 
- `set-parameter`   - sets application parameter application specific way
    - gets one parameter in format: `<key>=<value>`
- `set-envvar`      - sets application env variable application specific way
    - gets one parameter in format: `<key>=<value>`
- `cli`             - executes command in application specific cli

- `backup`          - creates application specific backup package 
    - path to backup destination is $1
- `restore`         - takes path to backup package parameter and restores application from it 

- `status`          - returns application specific status details
- `running`         - returns true of application is in running state
- `about`           - returns application specific about

!> Non zero exit code = method failed.

?> Each application should expose at least `about` and `status` methods.

## Common AMM Application definition

def.json provides application definition for AMM. 
Has to define: 
- `version`                 - for comparison of versions during updates

It should define: 
- `required-amm`            - AMM application is designed for
- `required-amm-version`    - required AMM version for this application to work properly

```json
{
    "version" : 0.21,
    "name" : "Awesome application",
    "dependencies" : [

    ],
    "hooks" : {
        "on-start": "hooks/before-start.sh",
        "on-started": "hooks/after-start.sh",
        "on-stop": "hooks/after-start.sh"
    },
    "methods": {
        "update" : "methods/update-app.sh",
        "reset" : "methods/reset.sh",
        "backup" : "methods/backup",
        "restore" : "methods/restore",
        "set-param": "methods/set-param.sh",
        "fs-permissions": "methods/fs-permissions.sh",
        "about" : "methods/about",
        "status": "methods/status"
    },
    "required-amm": "ANS", 
    "required-amm-version" : 0
}
```
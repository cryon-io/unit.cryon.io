# AMM Standard 
## Executables

- `amm`   # management binary
- `cli`   # cli access to application
- `is-up` # determines whether application runs

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
- `on-started`     - executes after application start
- `on-stop`     - hook to ensure application exists gracefully if required

## Common AMM Template Methods

AMM Methods provides applications way to act based on AMM commands application specific way. They have to be defined in def.json in application template root as `"methods"` with method name as key and value as path to executable or script to execute with this method.

Available methods:
- `reset`       - executes when there is requested application 
- `update`          - performs application specific operations required to update of application
- `set-parameter`   - sets application parameter application specific way
                    - gets one parameter in format: `<key>=<value>`
- `set-envvar`      - sets application env variable application specific way
                    - gets one parameter in format: `<key>=<value>`
- `status`          - returns application specific status details
- `about`           - returns application specific about
- `backup`          - creates application specific backup package in /tmp/ and returns path to it
- `restore`         - takes path to backup package parameter and restores application from it 
- `fs-permissions`  - configures application specific fs permissions
- `cli`             - executes command in application specific cli

*Each application should expose at least `about` and `status` methods*

## Common AMM Application definition - def.json

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
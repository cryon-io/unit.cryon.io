# AMM Standard 
## Executables

- `amm`   # management binary
- `cli`   # cli access to application
- `is-up` # determines whether application runs

## Properties

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
--detail                        # Prints "About" the configured application 
                                # - includes parameters and envvar configured 
                                # - includes information you want to show to user of the template
```

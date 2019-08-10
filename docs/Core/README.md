<div style="display: flex; align-items: center; justify-content:center; margin-top: 20px">
<img src="assets/core-logo.png" width="125" />
</div>

# Unit Core

Templating and management engine for simple setup and management of server side applications.

## Features

- modular
- simple setup and management
- security - all modules are audited (does not include audit of application code)
- auto updates to Unit Core and applications
- templated setups 
    - simplifies migration and reinstallation
    - ability to share setup templates
- simplified setup and management of multiple applications
- additional server options (e.g. swap configuration)

## Installation

```sh
    wget https://raw.githubusercontent.com/cryon-io/unit/master/install.sh -O /tmp/install.sh && sh /tmp/install.sh
```
You can see script code by opening URL from command above. 

For latest prerelease use: 
```sh
    wget https://raw.githubusercontent.com/cryon-io/unit/master/install-prerelease.sh -O /tmp/install.sh && sh /tmp/install.sh
```

## Structure

Unit is installed by above script into `/usr/sbin/unit` but you can install it anywhere you like. 
Configuration file is loaded from below paths in order and loads only first existing:
- `<unit binary directory>/unit.json`
- `<cwd>/unit.json`
- `/etc/unit/unit.json`

## Manual Updates

You can update `unit` binary by running `unit update-unit`.
For updating applications you can run: 
- `unit update`  - fully updates all applications 
- `unit smart-update` - updates applications respecting auto update levels in configuration of the applications.

## Configuration

1. create file `/etc/unit/unit.json`
2. configure [VPS specific options]()
3. apply configuration
    - `unit auto-update apply`
    - `unit configure-swap`

For more check out [examples]()

### Auto Updates

Unit uses for auto updates cron job. `unit` binary auto updates are part of `maintenance` are enabled for by default. You can disable `unit` auto updates by setting `"unit_auto_update": false` in `"maintenance"`

Auto updates for applications are disabled by default and can be in each application separately and based on underlaying AMM. 

## Applications

Unit Core is built for standardized application deployment and management with focus on cryptocurrency nodes. Although there is currently available AMM only for cryptocurrency nodes the support of applications and various scenarios is planned. 

If you would like to build your own AMM please check out [AMM standard docs]().

You can find the list of official and available application manager modules [here](). 

### Adding applications

1. Prepare application template based on [examples](). 
    * configure parameters and environment variables accordingly.
2. add application template to configuration file - `/etc/unit/unit.json`
3. run application setup - `unit setup <application_id>`
    * `application_id` is id you used as `id` in application template
4. start application - `unit start <application_id>`

NOTE: You can control multiple applications at once. 
    Example: `unit start app1 app2 app3`

### Multi Application Setups & Caveats 

Multi application setups may require additional configuration adjustment in case of applications of your choice uses same ports. 

In that case you can add binds into application template which specifies how should underlaying system treat ports. 

Bind format: `<ip>:<host_port>:<app_port_to_map>`
Example: `192.168.1.1:443:8080	`

*Remember, you CAN combine different types of nodes inside unit.json without need for binds. Binds and separate IPs are required only, if you run nodes of same type and these nodes has to run on same public port*
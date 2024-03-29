# a runit-inspired init system
## vulnerabilities
- configuration values are NOT verified
- input values (such as service names) are NOT sanitized
- failure to observe locks on the `./state` directory

## usage
- `service-core`: start/maintain services
- `service`: manage a service
- `init`: \*nix `init` replacement

### configuration
via `./config` (POSIX shell script)

## runlevels
### post-boot
- entered immediately
- located in `./post-boot.d`
- contains scripts executed in order according to `sort -h`

### services
- entered once post-boot is done
- located in `./enabled`
- contains service directories to be executed in any order

#### interface (from the context of `./enabled`)
- `my-service/run` is the entry point for a service
- adding/removing a service to this directory will logically start/stop it
- the presence of `my-service/hold` will also stop the service (or keep it stopped); removing `my-service/hold` will reenable it
- output is logged to `my-service/log`

### pre-halt
- entered when `init` receives a signal (ABRT, HUP, INT, or TERM)
- located in `./pre-halt.d`
- contains scripts executed in order according to `sort -h`


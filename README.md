# v10-postmortem
With the move to API Connect v10, **helm** is not longer used as part of the deployment process.


## Notes
- For usage information with the tool, use the command `./generate_postmortem.sh --help`
- The namespace is now automatically detected.  If the namespace is not correctly detected, use the switch `--extra-namespaces` to set the correct value.  For example `--extra-namespaces=apiconnect`.

## Pre-Requisite
- If EDB is deployed you will need the kubectl-cnp [plugin](https://www.enterprisedb.com/docs/postgres_for_kubernetes/latest/cnp-plugin) to gather appropriate EDB logs

## Deployment Instructions
### OVA
1. Connect to the target appliance via SSH then switch to the _root user_ using the following commands:
```shell
ssh {ova appliance hostname} -l apicadm
sudo -i
```
2.  Download the script using the following command:
```shell
curl -s -o generate_postmortem.sh https://raw.githubusercontent.com/ibm-apiconnect/v10-postmortem/master/generate_postmortem.sh
```
3.  Add execution permissions to file using the command `chmod +x generate_postmortem.sh`.
4.  Run the tool using the command `./generate_postmortem.sh --ova`.

### Cloud Pak 4i, Kubernetes, OpenShift
1.  Download the script using the following command:
```shell
curl -s -o generate_postmortem.sh https://raw.githubusercontent.com/ibm-apiconnect/v10-postmortem/master/generate_postmortem.sh
```
2.  Add execution permissions to file using the command `chmod +x generate_postmortem.sh`.
3.  Run the tool using the command `./generate_postmortem.sh`.


## Working a specific subsystem issue?
Enable the following if troubleshooting an issue for the following subsystems:  
> **Note**: Enabling diagnostics may cause the script to take much longer to complete (especially over a VPN connection).
### All (if requested by support)
- `--diagnostic-all`
### Manager
- `--diagnostic-manager`
- `--collect-crunchy`
- `--collect-edb`<br />
> **Note**: To use this option make sure to download the `crunchy_gather.py` script then place in the same directory as the postmortem script.   
> **Note**: To use this option make sure to download the `edb_mustgather.py` script then place in the same directory as the postmortem script.
### Gateway
`--diagnostic-gateway`
> **Note**: In order for this switch to function, make sure connections to `127.0.0.1` are not restricted on the local machine.
### Portal
`--diagnostic-portal`
### Analytics
`--diagnostic-analytics`

### Running EDB Mustgather on it's own   
To run the edb mustgather you need to pass the script 2 values:   
`EDB_CLUSTER_NAMESPACE`: the namespace where the edb cluster is running (eg apic)  
`LOG_PATH`: An **existing** folder in which you want to store the mustgather logs
Example of how to run the edb mustgather script
```
    ./edb_mustgather.sh apic edb
```

## Need help?
-  Open an issue to submit any feedback
-  Problem with the script?  Run the following command:
```shell
./generate_postmortem.sh --debug 2>&1 | tee /tmp/debug.log
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;then open an issue on the github page attaching the `debug.log` file.

## hauler target update vcenter

This command will update a vcenter target

### Synopsis


Description:
This command will update a vcenter target

Example Command:
```
hauler target update vcenter dev --host https://127.0.0.1 --user administrator@vsphere.local --password password --ignore_ssl true -s STORE --network "DEFAULT NETWORK" -r "CLUSTER/Resources"
```
		

```
hauler target update vcenter [Env] [flags]
```

### Options

```
  -c, --data_center string     Username (required)
  -s, --data_store string      Username (required)
  -h, --help                   help for vcenter
      --host string            Host Host (default "https://192.168.1.2")
  -i, --ignore_ssl             Ignore SSL
  -n, --network string         Username (required)
  -p, --password string        Password (required)
  -r, --resource_pool string   Username (required)
  -u, --user string            Username (required)
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler target update](hauler_target_update.md)	 - Update a target

###### Auto generated by spf13/cobra on 17-Dec-2022
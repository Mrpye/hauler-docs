## hauler target hypervisor datastores

This command will list all the hypervisor datastores

### Synopsis


Description:
This command will list all the hypervisor datastores

Example Command:
```
hauler target hypervisor datastore dev dev
```

Example Result:
```
+--------------------------+
|        DATASTORES        |
+--------------------------+
| NFS-001                  |
+--------------------------+
```
		

```
hauler target hypervisor datastores [env] [data_center] [flags]
```

### Options

```
  -h, --help   help for datastores
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler target hypervisor](hauler_target_hypervisor.md)	 - hypervisor helper functions

###### Auto generated by spf13/cobra on 17-Dec-2022

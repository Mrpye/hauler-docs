## hauler target hypervisor resource_pools

This command will list all the hypervisor resource_pools

### Synopsis


Description:
This command will list all the hypervisor resource_pools

Example Command:
```
hauler target hypervisor resource_pools dev dev
```

Example Result:
```
+---------------------------------------+
|            RESOURCE POOLS             |
+---------------------------------------+
| /DEV/host/DEV/Resources               |
+---------------------------------------+
```
		

```
hauler target hypervisor resource_pools [env] [data_center] [flags]
```

### Options

```
  -h, --help   help for resource_pools
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler target hypervisor](hauler_target_hypervisor.md)	 - hypervisor helper functions

###### Auto generated by spf13/cobra on 17-Dec-2022

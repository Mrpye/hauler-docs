## hauler target hypervisor networks

This command will list all the hypervisor networks

### Synopsis


Description:
This command will list all the hypervisor networks

Example Command:
```
hauler target hypervisor networks dev dev
```

Example Result:

```
+-------------------------------+
|            NETWORKS           |
+-------------------------------+
| /Dev/network/epg100           |
| /CG61/network/test-DPortGroup |
+-------------------------------+
```
		

```
hauler target hypervisor networks [env] [data_center] [flags]
```

### Options

```
  -h, --help   help for networks
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler target hypervisor](hauler_target_hypervisor.md)	 - hypervisor helper functions

###### Auto generated by spf13/cobra on 17-Dec-2022

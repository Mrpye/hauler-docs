## hauler workspace set project

This command set the current project

### Synopsis


Description:
This command set the current project

Example Command:
```
hauler workspace set 0
hauler workspace set "Demo App"
```

Result Example:
```
+-------+-----------------------+--------------+---------+
| INDEX |       FILE NAME       | PROJECT NAME | VERSION |
+-------+-----------------------+--------------+---------+
|     0 | Demo App-1.0.0.tar.gz | Demo App     | 1.0.0   |
|     1 | Netbox-1.0.0.tar.gz   | Netbox       | 1.0.0   |
+-------+-----------------------+--------------+---------+
```
		

```
hauler workspace set project [index, name or ?] [flags]
```

### Options

```
  -h, --help   help for project
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler workspace set](hauler_workspace_set.md)	 - Set a project

###### Auto generated by spf13/cobra on 17-Dec-2022
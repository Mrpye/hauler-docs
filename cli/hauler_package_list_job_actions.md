## hauler package list job_actions

This command will list the jobs in the package manifest

### Synopsis


Description:
This command will list the jobs in the package manifest

Example:
```
hauler package list jobs
```
Result:
```
+---------+-------------+-------------+-------------+
|   KEY   |    TITLE    | DESCRIPTION | APP PROFILE |
+---------+-------------+-------------+-------------+
| install | Install App |             |             |
+---------+-------------+-------------+-------------+
```


```
hauler package list job_actions [flags]
```

### Options

```
  -h, --help   help for job_actions
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler package list](hauler_package_list.md)	 - lists items from the package manifest

###### Auto generated by spf13/cobra on 17-Dec-2022
## hauler package add job

This command will add a job to the package manifest

### Synopsis


Description:
This command will add a job to the package manifest

Example:
```
hauler package add job "install" "Install App"
```
Result:
```
*******************************************
** Created Job install Added to Manifest **
*******************************************
```
		

```
hauler package add job [key] [title] [flags]
```

### Options

```
  -d, --desc string      Description
  -h, --help             help for job
  -p, --profile string   App Profile
  -s, --section string   Section
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler package add](hauler_package_add.md)	 - Adds an item to the package manifest

###### Auto generated by spf13/cobra on 17-Dec-2022

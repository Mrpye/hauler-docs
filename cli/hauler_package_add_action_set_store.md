## hauler package add action set_store

This command adds action set_store to the package manifest

### Synopsis


Description:
This command adds action set_store to the package manifest

The set_store stores a value

Example Command:
```
hauler package add action set_store store_value
```
Result Example:
```
actions:
	- key: store_value
	description: Add a description here
	action: action_set_store
	config:
		key: key
		store: system
		value: value
```
Parameters:
- store: the store where the value will be saved
- key: the key used to store the value
- value: the value to store

Other Parameters
```
# Run the below command to see help regarding result_action, result_format, target_action
hauler package add action -h
```



```
hauler package add action set_store [name] [flags]
```

### Options

```
  -d, --desc string      Description
  -h, --help             help for set_store
  -o, --override         Target Override
  -p, --profile string   App Profile
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler package add action](hauler_package_add_action.md)	 - Adds an action to the package manifest

###### Auto generated by spf13/cobra on 17-Dec-2022

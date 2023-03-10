## hauler package add action k8_apply

This command adds action k8_apply to the package manifest

### Synopsis


Description:
This command adds action k8_apply to the package manifest

k8_apply is used to apply k8s manifest files to a k8 cluster


Example Command:
```
hauler package add action k8_apply apply_yaml
```
Result Example:
```
actions:
	- key: apply_yaml
	description: Add a description here
	action: action_k8_apply
	config:
		file: ""
		namespace: default
		process_tokens: true
		target_action: default
```
Parameters:
- file: the path to the k8s manifest yaml file
- namespace: the namespace to use
- process_tokens: should the golang template tokens be processed


Other Parameters
```
# Run the below command to see help regarding result_action, result_format, target_action
hauler package add action -h
```



```
hauler package add action k8_apply [name] [flags]
```

### Options

```
  -d, --desc string      Description
  -h, --help             help for k8_apply
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

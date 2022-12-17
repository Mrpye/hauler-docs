## hauler package add action import_images

This command adds action import_images to the package manifest

### Synopsis


Description:
This command adds action import_images to the package manifest

Example Command:
```
hauler package add action import_images import_images
```

Result Example:
```

- ignore_images: images to ignore just put a comer separated list of the images to ignore 
- include_images: images to include just put a comer separated list of the images to include 


actions:
    - key: import_images
      description: Add a description here
      action: action_import_images
      config:
        ignore_images: ""
        include_images: ""
        target_action: default
```


## target_action

- target_action: target_action is used override the default target values not all actions require a target
	- default use the default target values
	- update update a parameter this will only be valid for this action
	- create create a new target you will need to supply all the parameters


To supply the parameters you must prefix the config with target_[property].

```
...
	config:
        ignore_images: ""
        include_images: ""
        target_Authorization: "123456"
        target_Host: "http://192.168.1.2"
        target_IgnoreSSL: true
        target_action: update
```

The easiest way to make sure you get the required values is to use the override flag when creating the action with the CLI.

Example Command:
```
hauler package add action import_images import_images -o
hauler package add action import_images import_images --override
```


		

```
hauler package add action import_images [name] [flags]
```

### Options

```
  -d, --desc string      Description
  -h, --help             help for import_images
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
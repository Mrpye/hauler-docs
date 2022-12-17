## hauler package add action

Adds an action to the package manifest

### Synopsis


Description:
Adds an action to the package manifest

*** Below is the help for handling results and overriding targets ***


## result_action

- result_action: is used handle the data returned by the action
	Options:
		- key_pair: will try to process the data into a key pair value this is mainly used with govc
		- js: the data processed using js see result_js for more details
		- log: the result is written to the log
		- print: the result is just printed

- result_format: This is used to format the result the data is formatted before been processed base on the selection in result_action
	- YAML
	- JSON
	- TOML
	- CSV
	- XML
	- Plain
	- none (default) data is just passed straight through
	- gotemplate: you can create a custom format bases on a golang template use result_template_file to pass the template to use

- result_template_file: used to specify the golang template
- result_store: this is the name of the store where the results will be kept
- result_js: this js the function that will be called this can either be a file or a function you must call the function ActionResults

	Function Parameters:
	- store: is the name of the store that was passed to the action
	- result: is the result data after been proceeded by result_format
	- return: is used to determine if the function successful (true/false)

```
#File Example
function ActionResults(store, result) {
    var parts=result[0].split(";")
    store_set(store,"service_ip", parts[1].trim())
    return true;
}

#Inline Example
config:
        command: vm.info vm_name
        result_action: 'js'
        result_format: 'none'
        result_js:|
			function ActionResults(store, result) {
				var parts=result[0].split(";")
				store_set(store,"service_ip", parts[1].trim())
				return true;
			}
        result_store: govc
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



	

### Options

```
  -h, --help   help for action
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler package add](hauler_package_add.md)	 - Adds an item to the package manifest
* [hauler package add action api_call](hauler_package_add_action_api_call.md)	 - This command adds action api_call to the package manifest
* [hauler package add action copy](hauler_package_add_action_copy.md)	 - This command adds action copy to the package manifest
* [hauler package add action count_to_error](hauler_package_add_action_count_to_error.md)	 - This command adds action count_to_error to the package manifest
* [hauler package add action govc](hauler_package_add_action_govc.md)	 - This command adds action govc to the package manifest
* [hauler package add action helm_delete](hauler_package_add_action_helm_delete.md)	 - This command adds action helm_delete to the package manifest
* [hauler package add action helm_deploy](hauler_package_add_action_helm_deploy.md)	 - This command adds action helm_deploy to the package manifest
* [hauler package add action import_images](hauler_package_add_action_import_images.md)	 - This command adds action import_images to the package manifest
* [hauler package add action k8_apply](hauler_package_add_action_k8_apply.md)	 - This command adds action k8_apply to the package manifest
* [hauler package add action k8_copy](hauler_package_add_action_k8_copy.md)	 - This command adds action k8_copy to the package manifest
* [hauler package add action k8_create_ns](hauler_package_add_action_k8_create_ns.md)	 - This command adds action k8_create_ns to the package manifest
* [hauler package add action k8_delete](hauler_package_add_action_k8_delete.md)	 - This command adds action k8_delete to the package manifest
* [hauler package add action k8_delete_ns](hauler_package_add_action_k8_delete_ns.md)	 - This command adds action k8_delete_ns to the package manifest
* [hauler package add action k8_deployments](hauler_package_add_action_k8_deployments.md)	 - This command adds action k8_deployments to the package manifest
* [hauler package add action k8_exec](hauler_package_add_action_k8_exec.md)	 - This command adds action k8_exec to the package manifest
* [hauler package add action k8_pods](hauler_package_add_action_k8_pods.md)	 - This command adds action k8_pods to the package manifest
* [hauler package add action k8_secrets](hauler_package_add_action_k8_secrets.md)	 - This command adds action k8_secrets to the package manifest
* [hauler package add action k8_service_ip](hauler_package_add_action_k8_service_ip.md)	 - This command adds action k8_service_ip to the package manifest
* [hauler package add action k8_services](hauler_package_add_action_k8_services.md)	 - This command adds action k8_services to the package manifest
* [hauler package add action k8_status](hauler_package_add_action_k8_status.md)	 - This command adds action k8_status to the package manifest
* [hauler package add action parse_token](hauler_package_add_action_parse_token.md)	 - This command adds action parse_token to the package manifest
* [hauler package add action patch](hauler_package_add_action_patch.md)	 - This command adds action patch to the package manifest
* [hauler package add action run_js](hauler_package_add_action_run_js.md)	 - This command adds action run_js to the package manifest
* [hauler package add action scp_copy](hauler_package_add_action_scp_copy.md)	 - This command adds action scp_copy to the package manifest
* [hauler package add action set_env](hauler_package_add_action_set_env.md)	 - This command adds action set_env to the package manifest
* [hauler package add action set_store](hauler_package_add_action_set_store.md)	 - This command adds action set_store to the package manifest
* [hauler package add action sleep](hauler_package_add_action_sleep.md)	 - This command adds action govc to the package manifest
* [hauler package add action template](hauler_package_add_action_template.md)	 - This command adds action template to the package manifest

###### Auto generated by spf13/cobra on 17-Dec-2022

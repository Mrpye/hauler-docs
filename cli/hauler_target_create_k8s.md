## hauler target create k8s

This command will create a K8 target

### Synopsis


Description:
This command will create a K8 target

Example Command:
```
hauler  target create k8s dev --context admin@dev-cluster
```
		

```
hauler target create k8s [Env] [flags]
```

### Options

```
  -p, --config_path string   Path to the kube config
  -c, --context string       Default Context (default "admin@k8")
  -h, --help                 help for k8s
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler target create](hauler_target_create.md)	 - Create a target

###### Auto generated by spf13/cobra on 17-Dec-2022

## hauler target create gitlab

This command will create a gitlab target

### Synopsis


Description:
This command will create a gitlab target

Example Command:
```
hauler target create github dev --host https://api.github.com -t RX8POFDW0W5ORo --ignore_ssl true
```
		

```
hauler target create gitlab [Env] [flags]
```

### Options

```
  -h, --help           help for gitlab
      --host string    Host (default "https://gitlab.com")
  -i, --ignore_ssl     Ignore SSL
  -t, --token string   GitLab Access Token (required)
```

### Options inherited from parent commands

```
      --config string   config file (default is $HOME/.hauler/config.json)
```

### SEE ALSO

* [hauler target create](hauler_target_create.md)	 - Create a target

###### Auto generated by spf13/cobra on 17-Dec-2022

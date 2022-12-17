# Manifest Template Tokens

[2. Hauler Guide](../2%20Hauler%20Guide%2025fbeb71ea224a1c96202b377a583fb4.md)

[Hauler Documentation](../../Hauler%20Documentation%203bf9a712efcd43a696ef5eb0b209c943.md)

As part of Hauler manifest you can embed GoLang template tokens into parameter to assist with building values . 

## Functions

Below is a list of the available helper fuctions. 

| Token | Description |
| --- | --- |
| {{ lc [string] }} | Makes the string Lower case |
| {{ uc [string] }} | Makes the string uppercase |
| {{ domain [string] }} | Get  the domain or IP from a URL |
| {{ port_int [string] }} | Get the port from a URL and returns it as a integer |
| {{ port_string [string] }}  | Get the port from a URL and returns as a string |
| {{ clean [string]  [replace with]}} | find any special chars and remove |
| {{ replace [string] [find] [replace] }} |  find any special chars and replace with space |
| {{ config [config value path] }} | get config from the config file |
| {{ target_config [string] }} | get target from config from config file |
| {{ get_param [string] }} | Gets the parameter value |
| {{ get_package_path }} |  get the path to package |
| {{ get_package_manifest  }} | get the deployment-package.yaml path |
| {{ file_path [string] }}  | add the package folder to a file   |
| {{ ReadFile  [string] }}  | reads a file as string |
| {{ get_store [store] [key] }} | Get the value from the store returns as a string |
| {{ get_store_int [store] [key] }} | Reads value from the store returns as a Integer |
| {{ get_store_bool [store] [key] }} | Reads value from the store return as a bool |
| {{ get_repo_image_url [image name] }} |  [docker.io/circleci/slim-base:latest] |
| {{ get_repo_image_name [image name] }} |  docker.io/circleci/[slim-base]:latest |
| {{ get_repo_image_name_tag [image name] }} | docker.io/circleci/[slim-base:latest] |
| {{ get_repo_image_account [image name] }} | docker.io/[circleci]/slim-base:latest |
| {{ get_repo_image_short_name [image name] }}  | docker.io/[circleci/slim-base]:latest |
| {{ get_repo_image_registry [image name] }} | [docker.io/circleci/slim-base]:latest |
| {{ get_repo_image_tag [image name] }} |  docker.io/circleci/slim-base:[latest] |
| {{ concat [separator]  [strings]}} | concat string |

### Functions Examples

```yaml
config:
        file: yaml/netbox-prereqs.template.yaml
        namespace: '{{get_param `namespace`}}'
        process_tokens: true
        target_action: default
```

## Action Tokens

When dealing with actions you also have access some extra tokens

| Token | Description |
| --- | --- |
| Package | Allows you to get data from the package |
| CurrentAction | Allows you to get info from the current action |
| CurrentImage | Allows you to get info from the current Container image |
| AppProfile | Gets the current app profile |
| CurrentTarget | Allows you to get info from the current target |

### Action Tokens Examples

```yaml
- key: import_images
      description: Import our images into docker hub or harbor
      action: action_import_images
      config:
        ignore_images: ""
        include_images: ""
        remap_docker: '{{.CurrentTarget.Host}}/{{.CurrentTarget.User}}/{{.CurrentImage.ImageNameTag}}'
        remap_harbor: '{{.CurrentTarget.Host}}/library/{{.CurrentImage.ImageNameTag}}'
        target_action: default
```
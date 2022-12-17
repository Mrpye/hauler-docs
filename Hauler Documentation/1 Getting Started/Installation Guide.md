# Installation Guide

[1. Getting Started](../1%20Getting%20Started.md)

# 1.1 Download Binary

The easiest way to het Hauler is to download the current stable release <Add Link to Download> 

- Copy the binary file to a suitable location

---

# 1.2 Create a basic config and setup directories

- run hauler this will create a basic config and setup the directories

```yaml
./hauler

2022/12/15 05:10:27 **************************************************************
2022/12/15 05:10:27 ** Creating config file: C:/Users/mrpye/.hauler/config.json **
2022/12/15 05:10:27 **************************************************************
2022/12/15 05:10:27 Creating default instance settings: OK
2022/12/15 05:10:27 Created jobs directory ./jobs: OK
2022/12/15 05:10:27 Created repo directory ./repo: OK
2022/12/15 05:10:27 Created projects directory ./projects: OK
2022/12/15 05:10:27 --------------------------------------------------------------
2022/12/15 05:10:27 ** Creating config file: C:/Users/mrpye/.hauler/config.json **
2022/12/15 05:10:27 --------------------------------------------------------------
```

- you can view the config file by typing or go to C:/Users/[user]/.hauler/config.json

```yaml
./hauler target edit
```

- this will show you the basic config

```yaml
{
  "config": {
    "default_instance": "default",
    "instances": {
      "default": {
        "current_project": ""
      }
    },
    "jobs": "./jobs",
    "repo": "./repo",
    "workspace_folder": "./projects"
  }
}
```

- you can edit config and relocate the folders

# 1.3 Create targets

In order to communicate with target endpoint you will need to create and environment and targets.

### Requirements:

- Git repository access token
    - **Gitlab:**  [https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html)
    - **Github:** [https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
- Docker Repo e.g. docker hub , harbor
- Kubenetes Cluster kubeconfig located user/.kube folder

<aside>
💡 You will need to use the cli to create the targets as passwords are encrypted

</aside>

- Target help

```yaml
hauler target -h

Available Commands:
  create      Create a target
  delete      Delete a target
  edit        This command will open the target config in text editor
  env         Lists Environments
  hypervisor  hypervisor helper functions
  list        list targets in an environment
  test        Test targets
  update      Update a target
```

- create a target

<aside>
💡 the basic targets required to build a simple K8 application package are git ,k8s cluster and registry

</aside>

```yaml
Usage:
  hauler target create github [env] [flags]

./hauler target create github dev --host https://api.github.com -t 12345678TOKEN --ignore_ssl true
./hauler  target create k8s dev --context admin@dev-cluster
./hauler  target create registry dev --host docker.io --user mrpye --password password --library library --ignore_ssl

```

<aside>
💡 Currently k8s uses the local kubeconfig to connect but token based is on the roadmap

</aside>

- Test the target endpoints

```yaml
Usage:
  hauler target test [env] [flags]

./hauler target test dev

+-----+--------------+-----------------+--------+---------+
| ENV | TARGET GROUP |   TARGET TYPE   | RESULT | ERR MSG |
+-----+--------------+-----------------+--------+---------+
| dev | git          | github          | Pass   |         |
| dev | hypervisor   | vcenter         | Pass   |         |
| dev | k8s          | k8s             | Pass   |         |
| dev | registry     | docker_registry | Pass   |         |
+-----+--------------+-----------------+--------+---------+
```
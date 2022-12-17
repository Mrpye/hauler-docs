# Power DNS

## Power DNS

**This Example installs Power DNS or pdns an open-source (GPL) software. It provides software to create authoritative DNS, Recursive DNS, DNS loading balancer, Debugging tools, and APIs to provision zones and records.**

This is a simple demonstration showing how to use helm charts in conjunction with parameters to build a package install and uninstall and to study how the package manifest is constructed.

### Build your first package

In order to install the package first we need to build the package, to do this we will pull the package manifest from the git repo 

[https://github.com/Mrpye/hauler-package-power-dns](https://github.com/Mrpye/hauler-package-power-dns)

The project is actually located under the master branch  and we will see how we can choose the branch where to pull the code from

**Requirements**

You will need to have setup an public access token for GitHub and configured hauler see  ([Install Guide for more details](../Installation%20Guide.md))

```yaml
# You can get help on the build process
./hauler package build -h

Usage:
  hauler package build [env] [flags]

Flags:
  -r, --add_repo        copy the package to the repo
  -b, --branch string   branch name
  -c, --clean           Clean the project folder before build
  -g, --git string      Github or Gitlab project of the hauler-package-manifest.yaml
  -h, --help            help for build
  -s, --skip_image      Skip the image
  -t, --tar             Tar The project

# This command will buld the package
./hauler package build dev -g Mrpye/hauler-package-power-dns -b master -c

# if you want tot build the package without pulling down the docker images 
# append --skip_image
./hauler package build dev -g Mrpye/hauler-package-power-dns -b master -c --skip_image

```

![Example output](Power%20DNS/Untitled.png)

Example output

---

### Create Answer file

The package has a set of parameters with default values but these default values are probably not suitable for you environment so we use answer files to allow you to override  these values. This step will show you how to create and answer file.

- see what profiles are alliable
    
    In this example we only have one the default
    

```yaml
./hauler package info

"profiles": [
    {
      "name": "default",
      "description": ""
    }
  ],
```

- Build the answer file

<aside>
💡 Below are a few ways you can generate the answer file try them out and see how they differ

</aside>

```yaml
./hauler workspace answer -h

Usage:
  hauler workspace answer [app_profile or ?] [? to prompt] [flags]

#build answer file to be edited the answer the file will be saved in the hauler directory
./hauler workspace answer “default” 

#build answer file to be edited the answer the file will be saved in the hauler directory
./hauler workspace answer “default”  

#on screen prompt	 the file will be saved in the hauler directory
./hauler workspace answer ? ?

```

![example using ./hauler workspace answer ? ?](Power%20DNS/Untitled%201.png)

example using ./hauler workspace answer ? ?

---

### Installing the package

Now we have our answer file we are ready to install the package. The following example will install Power DNS and create a job called “Power DNS”

<aside>
💡 Remember to change the environment to what you set when you configured your targets

</aside>

```yaml
./hauler workspace run job -h

Usage:
  hauler workspace run job [job_id and ?] [env] [app_profile] [flags]

Flags:
  -a, --answer string   Answer file
  -d, --dry_run         Will run the job without performing the action
  -h, --help            help for job
  -j, --job string      save as a job
  -s, --skip_remap      Skips remapping images and uses the original source location
  -v, --verbose         Will output verbose information

#Run install in prod using the default profile using answer file answer_power_dns-1.0.0_default.yaml and save job as “Power DNS”
./hauler workspace run job "install" "prod" "default" -a "answer_power_dns-1.0.0_default.yaml" -j "Power DNS"
```

![Example screen output](Power%20DNS/Untitled%202.png)

Example screen output

---

### Manage the package using the job feature

One a package is install and if you used the -j, --job flag you can then see the state of the job and available action you can perform when the application is in a particular state.

```yaml
./hauler job -h

Available Commands:
  list        This list the jobs history the state what actions can be performed
  run         This allow you to run job against a job history item
```

- First lets see what jobs we have (the output will be different for you)

```yaml
./hauler job list
```

![Untitled](Power%20DNS/Untitled%203.png)

You can see that we have our job Power DNS and that the last job run was install and the available jobs we can run are uninstall and get_service_ip. Lets run the get_service_ip

<aside>
💡 Because the answer file is saved with the job it also now also contains the environment and other setting we passed when installing we only need the job name and job to run.

</aside>

```yaml
./hauler job run -h

Usage:
  hauler job run [[job name] [job]] [?] [flags]

Flags:
  -d, --dry_run      Will run the job without performing the action
  -f, --force        This will force the job to run even if it not allowed to run list of job
  -h, --help         help for run
  -s, --skip_remap   Skips remapping images and uses the original source location
  -v, --verbose      Will output verbose information
```

Below are a few example on how we can run a job

```yaml
#Run a job from a historical job
./hauler job run "Power DNS" "get_service_ip"

#Prompted for values
./hauler job run ?
```

![Example output](Power%20DNS/Untitled%204.png)

Example output

- now you have the skills to manage the job try uninstalling power DNS and the reinstalling

### Summary

You have now seen how you can build a package install, build answer files and manage the install with jobs. now you have the basic skill to perform these task why not take a look at how the package is constructed  you can find package located in your workspace folder.
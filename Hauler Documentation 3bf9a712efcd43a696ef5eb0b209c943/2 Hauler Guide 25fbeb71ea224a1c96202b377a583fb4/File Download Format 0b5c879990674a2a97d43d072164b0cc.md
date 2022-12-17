# File Download Format

[2. Hauler Guide](../2%20Hauler%20Guide%2025fbeb71ea224a1c96202b377a583fb4.md)

[Hauler Documentation](../../Hauler%20Documentation%203bf9a712efcd43a696ef5eb0b209c943.md)

# File Download Format

This guide talks about the file download format, currently it only support downloading files from git repo GitHub, GitLab. 

There are two formats:

## Single location Format

hauler will only download the file from the specified location. If the target type is not configured then the download will fail.

```yaml
#Single location Format
git|**[git repo (github/gitlab)]**:**[project]**@**[branch or tag]**:**[file_name]**:**[target file name and path]

#Download from github
git|github:Mrpye/NetboxPackage@main:netbox-3.0.0.tgz:netbox-3.0.0.tgz

#Download from Gitlab
git|gitlab:123@main@main:netbox-3.0.0.tgz:netbox-3.0.0.tgz**
```

## Choice of locations:

hauler will only download the file from the location where the target is configured

```yaml
#Single location Format
git|[**github:Mrpye**:**[project]**@**[branch or tag]**:**[file_name]|gitlab:[project]**@**[branch or tag]**:**[file_name]]**:**[target file name and path]

#Example
git|[github:Mrpye/package_manager@main:README.md|gitlab:36@main:README.md]:docs/README2.md**

```
# Package Manifest Format

This guide will walk through the element of the package manifest.

## Table of content

# Meta Data

Meta data is used to store information about the package also configuration information.

```yaml
meta_data:
    package_name: Netbox
    description: This installs Netbox into K8s Cluster
    app_version: 1.0.0
    package_version: 1.0.0
    author: Andrew Pye
    contact: test@test.com
    git_package: https://api.github.com/Mrpye/NetboxPackage
    git_project: ""
    git_branch: master
    create_date: "2022-06-30 17:06:18"
    update_date: "2022-06-30 17:11:42"
    vars:
        default_netbox_namespace: netbox
        github_project: Mrpye/NetboxPackage@main@main
        gitlab_project: null
```

| Property | Data Type | Description |
| --- | --- | --- |
| package_name: | string | name of the package |
| description: | string | Describe what the package installs |
| version: | string | Version of the package |
| package_version:  | string | Version of the package format |
| contact: | string | Contact information |
| git_package: | string | Git repo relating to this package) |
| git_project: | string | Git Repo relating to the application |
| git_branch: | string | git_project branch |
| create_date: | string | The date the package was created |
| update_date:  | string | The date the package was updated |
| vars: | map[string] interface{} | Used to store custom values |

# Application Profile

Application is used so that you can alter the way the package works based on what profile you choose.

```yaml
application_profiles:
    - default
default_application_profile: default
```

| Property | Data Type | Description |
| --- | --- | --- |
| default_application_profiles: | string | Set the default application profile |
| application_profiles: | []string | List of application profile available for this package |

# Files

Files is used to specify files to download from the git repo into the package

```yaml
files:
    - git|github:{{.Package.Meta.Vars.github_project}}:yaml/netbox-prereqs.template.yaml:yaml/netbox-prereqs.template.yaml
    - git|github:Mrpye/NetboxPackage@main:yaml/netbox-svc-dns.yaml:yaml/netbox-svc-dns.yaml
    - git|github:{{.Package.Meta.Vars.github_project}}:netbox-3.0.0.tgz:netbox-3.0.0.tgz
```

| Property | Data Type | Description |
| --- | --- | --- |
| files: | []string | List of application profile available for this package |

## File Download Format

[File Download Format](guide_file_download_format.md)

# Images

```yaml
images:
    - image: netboxcommunity/netbox:v2.10.4
    - image: busybox:1.32.1
    - image: docker.io/bitnami/postgresql:11.11.0-debian-10-r0
    - image: docker.io/bitnami/postgres-exporter:0.8.0-debian-10-r354
    - image: docker.io/bitnami/minideb:buster
    - image: docker.io/bitnami/redis:6.0.10-debian-10-r19
    - image: docker.io/bitnami/redis-sentinel:6.0.10-debian-10-r18
    - image: docker.io/bitnami/redis-exporter:1.16.0-debian-10-r7
    - image: docker.io/bitnami/postgresql:12.11.0-debian-11-r13
skip_image_download: true
```

| Property | Data Type | Description |
| --- | --- | --- |
| images: | []image | List of container images that the package requires |
| - image: | string | Image that the package requires |
| skip_image_download | bool | setting this to true images wont be downloaded false then images will be download at build time |

# Actions

Actions are steps that will perform some task, the two examples below import docker images based on the image in images property, and the second action apply a YAML to kubernetes cluster.

```yaml
actions:
    - key: import_images
      description: Import our images into docker hub or harbor
      action: action_import_images
      config:
        ignore_images: ""
        include_images: ""
        remap_docker: '{{.CurrentTarget.Host}}/{{.CurrentTarget.User}}/{{.CurrentImage.ImageNameTag}}'
        remap_harbor: '{{.CurrentTarget.Host}}/library/{{.CurrentImage.ImageNameTag}}'
        target_action: default

    - key: k8_apply_netbox_prereqs
      description: Add a description here
      action: action_k8_apply
      config:
        file: yaml/netbox-prereqs.template.yaml
        namespace: '{{get_param `namespace`}}'
        process_tokens: true
        target_action: default
```

The following properties are common to all actions. depending on the action additional properties will need to be defined.

| Property | Data Type | Description |
| --- | --- | --- |
| key: | string | unique identifier for each action (if you use app_profile you can set the same key on multiple actions as long as they have different app_profile ) |
| description: | string | Description for the action |
| action | string | Action to perform |
| config | map[string] interface{} | Configuration for the action |
| app_profile | string | used to filter action based on the selected app_profile  |

# Jobs

Jobs are a sequence of actions to be performed, jabs also have a simple control flow such as loops and go-to and error branching. Multiple jobs can be defined in a package that can perform tasks for example Install/Uninstall.

```yaml
jobs:
  - key: install
    title: Install the Package
    description: "This job will install images and install netbox"
    actions:
      - action: import_images
      - action: k8_apply_netbox_prereqs
      - action: helm_deploy

  - key: delete
    title: Install the Package
    description: "This job will delete netbox"
    actions:
      - action: helm_delete
      - action: k8_delete_netbox_prereqs
```

## Job Properties

| Property | Data Type | Description |
| --- | --- | --- |
| key: | string | unique identifier for each action (if you use app_profile you can set the same key on multiple actions as long as they have different app_profile ) |
| title | string | Title of the job |
| decription | string | A description of the job |
| actions | action[] | A list of actions to be perfomred |

## Action Properties

| Property | Data Type | Description |
| --- | --- | --- |
| action | string | An action key that has been defined in the package. also some reserved keyworks can be used here (see Job Action Keywords) |
| label | string | A label for this step used to unequley identify this step |
| fail | string | an action or label to jump to if the action fails |
| disable | bool | disable this action (default false) |

## Job Action Keywords

| keyworks | Example | Description |
| --- | --- | --- |
| end | end | Stops the workflow and exist the job |
| label | print [string] | Prints a value to the console |
| do | do | A control loop will return here when a do-end is reached. These can me nested |
| do-end | do-end | the return statement for the do control loop |
| wait-seconds | wait-seconds [int] | This will pause the workflow for x seconds |
| wait-minutes | wait-minutes [int] | This will pause the workflow for x minutes |
| loop | loop [int] | A control loop will return here when a loop-end is reached and will loop x amount of time. these can me nested |
| loop-end |  | the return statement for the loop control loop |

# Documents

This section allows multiple read me file to downloaded from git repo and be combined and converted 

```yaml
documents:
	- name: Name of the document
		title: Title of the document
		description: Put a description here
	  document_path: doc
	  convert_to_html: true
	  parts:
		  - 'git|git_hub:{{.Package.Meta.Vars.github_project}}:README.md:'
      - 'git|git_hub:{{.Package.Meta.Vars.github_project}}:Howto.md:'
```

| Property | Data Type | Description |
| --- | --- | --- |
| name: | string | The name of the document will be used to name the document. |
| title: | string | Tile of the document |
| description: | string | Description of the document |
| document_path | string | where the document will be stored |
| convert_to_html | bool | true if to convert the document to html |
| parts | []string | The element of the readme file |

## Document Parts format

# Parameters

Parameters can be used to inject values into properties of the package manifest or into file

```yaml
parameters:
    - key: instance_name
      title: Instance Name
      description: ""
      value: netbox-v3
      input_type: text
			hidden: false

    - key: admin_api_token
      title: API Token
      description: ""
      value: CHANGE_ME
			value_from_file: /certs/ca.crt
      input_type: text
			validation_rule: "required"
			validation_convert:

    - key: nfs_path
      title: Path for the NFS
      description: ""
      value: /data/nfs/netboxmedia/
      input_type: text
			hidden: true
```

| Property | Data Type | Description |
| --- | --- | --- |
| key: | string | unique identifier for each property (if you use app_profile you can set the same key on multiple actions as long as they have different app_profile ) |
| title: | string | Tile of the document |
| description: | string | Description of the document |
| value: | interface{} | The default value of the property |
| value_from_file: | string | file to read the value from |
| input_type: | string | what type of data is stored in the value used for validation |
| validation_rule: | string | validation rule |
| validation_convert: | string | convert value before validation  |
| hidden: | bool | is hidden |
| app_profile: |  | used to filter action based on the selected app_profile  |

## Built in Input Types

| Data Type | Description |
| --- | --- |
| text | Simple text |
| password | password fields data wil be encryted |
| int | integert value e.g 1 2 3 4 |
| float | float value 1.2 9.5  |
| bool | bool value true false |
| ip | an ip address 172.0.0.1 |
| email | an email addresss test@test.com |
| date | a date value |
| base64 | base64 value |
| url | URL value |
| mac | mac address |
| json | json string |

## Validation Rules

You also expand on the validation set the datatype to **text**  then use the validation rules below. 

The validation uses the below validation libary

[https://github.com/gookit/validate](https://github.com/gookit/validate)

| validator/aliases | description |
| --- | --- |
| required | Check value is required and cannot be empty. |
| required_if/requiredIf | required_if:anotherfield,value,... The field under validation must be present and not empty if the anotherField field is equal to any value. |
| requiredUnless | required_unless:anotherfield,value,... The field under validation must be present and not empty unless the anotherField field is equal to any value. |
| requiredWith | required_with:foo,bar,... The field under validation must be present and not empty only if any of the other specified fields are present. |
| requiredWithAll | required_with_all:foo,bar,... The field under validation must be present and not empty only if all of the other specified fields are present. |
| requiredWithout | required_without:foo,bar,... The field under validation must be present and not empty only when any of the other specified fields are not present. |
| requiredWithoutAll | required_without_all:foo,bar,... The field under validation must be present and not empty only when all of the other specified fields are not present. |
| -/safe | The field values are safe and do not require validation |
| int/integer/isInt | Check value is intX uintX type, And support size checking. eg: "int" "int:2" "int:2,12" |
| uint/isUint | Check value is uint(uintX) type, value >= 0 |
| bool/isBool | Check value is bool string(true: “1”, “on”, “yes”, “true”, false: “0”, “off”, “no”, “false”). |
| string/isString | Check value is string type. |
| float/isFloat | Check value is float(floatX) type |
| slice/isSlice | Check value is slice type([]intX []uintX []byte []string …). |
| in/enum | Check if the value is in the given enumeration "in:a,b" |
| not_in/notIn | Check if the value is not in the given enumeration "contains:b" |
| contains | Check if the input value contains the given value |
| not_contains/notContains | Check if the input value not contains the given value |
| string_contains/stringContains | Check if the input string value is contains the given sub-string |
| starts_with/startsWith | Check if the input string value is starts with the given sub-string |
| ends_with/endsWith | Check if the input string value is ends with the given sub-string |
| range/between | Check that the value is a number and is within the given range |
| max/lte | Check value is less than or equal to the given value |
| min/gte | Check value is greater than or equal to the given value(for intX uintX floatX) |
| eq/equal/isEqual | Check that the input value is equal to the given value |
| ne/notEq/notEqual | Check that the input value is not equal to the given value |
| lt/lessThan | Check value is less than the given value(use for intX uintX floatX) |
| gt/greaterThan | Check value is greater than the given value(use for intX uintX floatX) |
| email/isEmail | Check value is email address string. |
| intEq/intEqual | Check value is int and equals to the given value. |
| len/length | Check value length is equals to the given size(use for string array slice map). |
| regex/regexp | Check if the value can pass the regular verification |
| arr/list/array/isArray | Check value is array, slice type |
| map/isMap | Check value is a MAP type |
| strings/isStrings | Check value is string slice type(only allow []string). |
| ints/isInts | Check value is int slice type(only allow []int). |
| min_len/minLen/minLength | Check the minimum length of the value is the given size |
| max_len/maxLen/maxLength | Check the maximum length of the value is the given size |
| eq_field/eqField | Check that the field value is equals to the value of another field |
| ne_field/neField | Check that the field value is not equals to the value of another field |
| gte_field/gteField | Check that the field value is greater than or equal to the value of another field |
| gt_field/gtField | Check that the field value is greater than the value of another field |
| lte_field/lteField | Check if the field value is less than or equal to the value of another field |
| lt_field/ltField | Check that the field value is less than the value of another field |
| file/isFile | Verify if it is an uploaded file |
| image/isImage | Check if it is an uploaded image file and support suffix check |
| mime/mimeType/inMimeTypes | Check that it is an uploaded file and is in the specified MIME type |
| date/isDate | Check the field value is date string. eg 2018-10-25 |
| gt_date/gtDate/afterDate | Check that the input value is greater than the given date string. |
| lt_date/ltDate/beforeDate | Check that the input value is less than the given date string |
| gte_date/gteDate/afterOrEqualDate | Check that the input value is greater than or equal to the given date string. |
| lte_date/lteDate/beforeOrEqualDate | Check that the input value is less than or equal to the given date string. |
| has_whitespace/hasWhitespace | Check value string has Whitespace. |
| ascii/ASCII/isASCII | Check value is ASCII string. |
| alpha/isAlpha | Verify that the value contains only alphabetic characters |
| alphaNum/isAlphaNum | Check that only letters, numbers are included |
| alphaDash/isAlphaDash | Check to include only letters, numbers, dashes ( - ), and underscores ( _ ) |
| multiByte/isMultiByte | Check value is MultiByte string. |
| base64/isBase64 | Check value is Base64 string. |
| dns_name/dnsName/DNSName/isDNSName | Check value is DNSName string. |
| data_uri/dataURI/isDataURI | Check value is DataURI string. |
| empty/isEmpty | Check value is Empty string. |
| hex_color/hexColor/isHexColor | Check value is Hex color string. |
| hexadecimal/isHexadecimal | Check value is Hexadecimal string. |
| json/JSON/isJSON | Check value is JSON string. |
| lat/latitude/isLatitude | Check value is Latitude string. |
| lon/longitude/isLongitude | Check value is Longitude string. |
| mac/isMAC | Check value is MAC string. |
| num/number/isNumber | Check value is number string. >= 0 |
| cn_mobile/cnMobile/isCnMobile | Check value is china mobile number string. |
| printableASCII/isPrintableASCII | Check value is PrintableASCII string. |
| rgb_color/rgbColor/RGBColor/isRGBColor | Check value is RGB color string. |
| url/isURL | Check value is URL string. |
| fullUrl/isFullURL | Check value is full URL string(must start with http,https). |
| ip/isIP | Check value is IP(v4 or v6) string. |
| ipv4/isIPv4 | Check value is IPv4 string. |
| ipv6/isIPv6 | Check value is IPv6 string. |
| CIDR/isCIDR | Check value is CIDR string. |
| CIDRv4/isCIDRv4 | Check value is CIDRv4 string. |
| CIDRv6/isCIDRv6 | Check value is CIDRv6 string. |
| uuid/isUUID | Check value is UUID string. |
| uuid3/isUUID3 | Check value is UUID3 string. |
| uuid4/isUUID4 | Check value is UUID4 string. |
| uuid5/isUUID5 | Check value is UUID5 string. |
| filePath/isFilePath | Check value is an existing file path |
| unixPath/isUnixPath | Check value is Unix Path string. |
| winPath/isWinPath | Check value is Windows Path string. |
| isbn10/ISBN10/isISBN10 | Check value is ISBN10 string. |
| isbn13/ISBN13/isISBN13 | Check value is ISBN13 string. |

## Validation Convert

| filter/aliases | description |
| --- | --- |
| int/toInt | Convert value(string/intX/floatX) to int type v.FilterRule("id", "int") |
| uint/toUint | Convert value(string/intX/floatX) to uint type v.FilterRule("id", "uint") |
| int64/toInt64 | Convert value(string/intX/floatX) to int64 type v.FilterRule("id", "int64") |
| float/toFloat | Convert value(string/intX/floatX) to float type |
| bool/toBool | Convert string value to bool. (true: “1”, “on”, “yes”, “true”, false: “0”, “off”, “no”, “false”) |
| trim/trimSpace | Clean up whitespace characters on both sides of the string |
| ltrim/trimLeft | Clean up whitespace characters on left sides of the string |
| rtrim/trimRight | Clean up whitespace characters on right sides of the string |
| int/integer | Convert value(string/intX/floatX) to int type v.FilterRule("id", "int") |
| lower/lowercase | Convert string to lowercase |
| upper/uppercase | Convert string to uppercase |
| lcFirst/lowerFirst | Convert the first character of a string to lowercase |
| ucFirst/upperFirst | Convert the first character of a string to uppercase |
| ucWord/upperWord | Convert the first character of each word to uppercase |
| camel/camelCase | Convert string to camel naming style |
| snake/snakeCase | Convert string to snake naming style |
| escapeJs/escapeJS | Escape JS string. |
| escapeHtml/escapeHTML | Escape HTML string. |
| str2ints/strToInts | Convert string to int slice []int |
| str2time/strToTime | Convert date string to time.Time. |
| str2arr/str2array/strToArray | Convert string to string slice []string |
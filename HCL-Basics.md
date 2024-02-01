---
tags:
  - Terraform
  - DevOps
  - Concepts
  - "#Configuration_Language"
---

local-file-type
random-pet

HCL Syntax:
- consists of a block and argument
```
<block> <parameter> {
	key1 = value1
	key2 = value2
}
```

A block is defined within curly braces and it contains a set of arguments in key value pair format representing the configuration detail.

A block in terraforms contains information about infrastructure platform and set of resources withing that platform we want to create.

For example to create a local file we will use local_file resource and we'll create `local.tf` file cotaining following configuration:

```
resource "local_file" "pet" {
	filename = "/root/pets.txt"
	content = "We love pets!"
}
```

![[./metadata/attachments/Images/Pasted image 20240116162945.png|Pasted image 20240116162945.png]]
Here is `resource` is a block name (keyword) which shows the type of block we are using.
after the resource declaration we have "resource type" called `local_file` its fixed value.  
	Here "local" is a provider and "file" follwing the underscore _ shows the type of resource which is `file`
Next is a Resource name `pet` which can be anything.
Next we have Arguments specific to the resource type in a key value pair format. In this case for `local_file` we have `filename` and `content` which are self explanatory.


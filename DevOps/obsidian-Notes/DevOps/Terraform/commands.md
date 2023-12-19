#terraform #commands 

### Init
`terraform init`

### Plan
`terraform plan`

### Apply
`terraform apply`  


### Show
`terraform show`
`terraform show -json`

### Destroy
`terraform destroy`


### Validate
`terraform validate`

### Format
`terraform fmt`

### Providers
`terraform providers`
`terraform providers mirror /root/terraform/new_local_file`


### Output 
`terraform output`

### Refresh - if there is any manual update, it refreshes the state file
`terraform refresh`

	### Graph
`terraform graph`
apt install graphviz
`terraform graph | dot -Tsvg > graph.svg`

	![[Pasted image 20231208201331.png]]
	




The `terraform apply` failed in spite of our validation working! This is because the validate command only carries out a general verification of the configuration. It validated the resource block and the argument syntax but not the `values` the arguments expect for a specific resource!



### State list
`terraform state list`

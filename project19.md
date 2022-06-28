# PROJECT 19: AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM - PART 4

- Migrate codes to Terraform cloud.

- Created a terraform cloud action

- Created an organization

- Configured variable

- Installed packer on computer

![packer](images/project-19/packer.png)

- Build AMIs with packer

- Ran the following packer command

```
packer build bastion.pkr.hcl
packer build web.pkr.hcl
packer build nginx.pkr.hcl
packer build ubuntu.pkr.hcl
```

![vscode](images/project-19/vscode-packer.png)

- AMIs created in AWS console.

![amis](images/project-19/AMIs.png)

- Initiate the following command below from the web console

```
terraform plan
terraform apply
```

![total files](images/project-19/total-files-created.png)

![files created](images/project-19/files-created-in-cloud-terraform.png)

- Website opened in the browser to confirm working.

![domain active](images/project-19/domain-active.png)

![domain secured](images/project-19/domain-secured.png)

- Terraform destroyed

![destroyed](images/project-19/terraform-destroyed.png)

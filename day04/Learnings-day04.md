# TerraWeek Day-04

Terraform state is a crucial component of managing infrastructure using Terraform, an infrastructure as code (IaC) tool. It helps track and maintain the current state of provisioned resources, enabling accurate resource management and ensuring consistency throughout the infrastructure lifecycle.
## Why TerraForm state is a neccessity
1) Tracking Current State:
Terraform state records the current state of deployed resources, including configurations, attributes, and dependencies. It provides a comprehensive view of the infrastructure, helping operators understand what resources are deployed and their relationships.
2) Accurate Resource Management:
By comparing the desired state with the recorded state, Terraform determines the necessary actions to align the infrastructure with the desired configuration. It ensures precise resource management by creating, updating, or destroying resources as needed.
3) Dependency Management:
Terraform state manages dependencies between resources, ensuring the correct order of provisioning or updates. It avoids errors caused by improper resource ordering and ensures consistent infrastructure deployment.

- Real time use case: Assume a scenario where you are applying changes to a infrastructure simultaneously a colleague of your's who is also assigned to the same project wishes to apply changes, Now in this scenario if the state file is stored on remote backend then a state lock is accquired the moment you hit the command `terraform apply` ,
since state is applied your colleague won't be able to make the changes at the same time while you are still updating the infrastructure . This strategy is helpful in a case when more than one individual are working on the same infrastructure.

## Types of State management
When working with Terraform, you have the option to store the state file locally or remotely. The choice between local and remote storage depends on factors like team collaboration, infrastructure complexity, and security requirements. Let's explore the different methods:

### 1) Local State Storage:
- Default Behavior: By default, Terraform stores the state file locally in the same directory as the Terraform configuration files (e.g., terraform.tfstate).
- Advantages: Local state storage is simple and suitable for individual developers or small projects. It requires no additional setup or dependencies.
- Limitations: Local state storage lacks collaboration features and can cause issues when multiple team members work on the same infrastructure simultaneously. It is also prone to loss if the state file is accidentally deleted or misplaced.
- Snapshot of my Local terraform state file's content:
 ![Screenshot 2023-06-08 at 9 12 34 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/36056083-2e42-4341-868c-0c741860bd5f)
 

### 2) Remote State Storage:
Overview: Remote state storage involves storing the state file in a remote location accessible to the Terraform team. This can be achieved using various methods and supported backend providers.
- Advantages: Remote state storage provides several benefits:
- Collaboration: Multiple team members can work on the same infrastructure simultaneously without conflicts.
- Consistency: Remote state storage ensures that all team members are working with the latest state, reducing the risk of diverging infrastructure.
- Security: Remote storage can offer advanced security features, such as access controls and encryption, protecting sensitive infrastructure data.
- Scalability: Remote state storage is more suitable for complex infrastructures or large-scale projects, as it supports concurrent operations and can handle higher workloads.
- Backend Providers: Terraform supports several backend providers for remote state storage, including AWS S3, Azure Storage, Google Cloud Storage, HashiCorp Consul, and more. Each provider has its own setup requirements and configuration options.

-> In order to make our infrastructure's state remote we need to attach the remote backend (`backend block`) in our terraform file before initialising it ,like depicted in below example:
```
terraform {

required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "4.66.1"
    }
  }

backend "s3" {
        bucket = "terraweek-demo-state-bucket-aditya" 
        key = "terraform.tfstate"
        region = "ap-south-1"
        dynamodb_table = "terraweek-demo-state-table"
}
}
```



- The `terraform state` command is helpful for examining the current state of your infrastructure, making changes to the state, importing existing resources, and managing the state file itself. It provides control and visibility into the state of your infrastructure managed by Terraform.
- Example : A screenshot of the command from a terraform file which i used to create remote Backend infrastructure to store my state files: `terraform state list` 
![Screenshot 2023-06-08 at 9 21 50 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/4ac7b1fc-b6b5-4748-922e-b183169a2f58)


### Screenshots of Remote backend for state :
- Storing state on S3:
![Screenshot 2023-06-08 at 9 25 52 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/f6ec4aaf-a01b-454b-a5e6-645e8be34f4f)
- Logs of lock variable creation on DynamoDb:
![Screenshot 2023-06-08 at 9 27 11 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/fcb2f33f-810f-4495-a965-b5e2cc5e7e3d)

( ps : references for setting up remote backend infrastructure and a remote backend demo can be found in the same day's task)

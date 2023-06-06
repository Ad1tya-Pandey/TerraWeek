# TerraWeek Day - 2

- Example of a terraform Main file:
 
 ![Screenshot 2023-06-06 at 8 27 52 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/d29a73c7-18ca-4a9f-8f29-e56b9cec860c)

## Main File:
- The main file in Terraform is the heart of your infrastructure configuration. It defines the resources and their properties that you want to create and manage. Let's take a closer look at an example main file:



## Understanding the resource block : (Line by Line)

1) A `resource block` is declared then it's type ( i.e "local_file) is defined and then the name to resource is attached in this case ("devops")
2) `Filename attrubute` is given (notice how we're using a variable , will explain later in this section) this would be the name of our file which will be created using terraform 
3) Here we are storing the content for our file which is also a variable in the `content attribute`
      - We have such two rosources in our main file ( we can have as many resource as we want in our main file 

- It was noticeable that the above resource block has values that are fetched from somewhere else, Now although we can assign values statically to the main.tf configuration file 
but it's not recommended practice as the config template remains the same and we need to change the resources requirement as per out need.
That's why we are using here variables to assign values to our attributes.

- There are various ways to assign variables in terraform but the most preffered one it to create a variable file which would store our values and then be acceseed 
by our  `main.tf` file 

- Here's the `variable.tf` file which i used for the above mentioned `main.tf` file:

![Screenshot 2023-06-06 at 9 04 32 PM](https://github.com/Ad1tya-Pandey/TerraWeek/assets/101057601/597f3cd4-ba0e-4a47-b3be-e4342462dc4b)

## Understanding the variables for resource block :

1) filename: This variable represents the path to a file and has a default value of "/home/ubuntu/TerraForm/terraVariables/automated-day2.txt". You can customize it based on your specific needs.
2) content: The content variable holds the content that will be generated through a variable. In this case, it has a default value of "This content is generated through a variable".
3) Community: This variable is left empty, allowing you to provide a value during runtime. It adds an element of interactivity to your Terraform configuration.(`export TF_VAR_Community="TWS community"` using this command we set value
4) content_map: The content_map variable is of type map and represents a key-value pair mapping. It defines different content values for specific keys. In our example, "day1" maps to "Introduction to Terraform and HCL" and "day2" maps to "Variables and their types in HCL".
5) file_list: This variable is of type list and represents a list of file paths. It includes two file paths: "/home/ubuntu/TerraForm/terraVariables/file-1.txt" and "/home/ubuntu/TerraForm/terraVariables/file-2.txt".

## Output block :
 Output block provides us with ability of displaying a certain message on the execution of our `main.tf` file 
 - It can be noticed that we have used a variable (`Community`) to assign our output message 
 
## Terraform.tfstate :
In the world of Terraform, the .tfstate file holds great importance as it manages the state of your infrastructure. This blog post aims to provide a concise summary of the .tfstate file, its purpose, and its significance in infrastructure provisioning and management.
- Terraform state is a record of the resources created by Terraform and their current state. It serves as a crucial piece of information for managing resources effectively. The .tfstate file acts as the storage for this data.

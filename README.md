# oci-arch-rubyonrails-mds

MySQL Database Service is a fully-managed Oracle Cloud Infrastructure native service. It's developed, managed, and supported by the MySQL team at Oracle. Tasks such as backup and recovery, database and operating system patching, and so on are automated. You are responsible solely for managing your data, schema designs, and access policies.

## Prerequisites

- Permission to `manage` the following types of resources in your Oracle Cloud Infrastructure tenancy: `vcns`, `internet-gateways`, `route-tables`, `network-security-groups`, `subnets`, `mysql-family`, and `instances`.

- Quota to create the following resources: 1 VCN, 3 subnets, 1 Internet Gateway, 1 NAT Gateway, 2 route rules, 1 MDS instance, and 3 compute instances (bastion host + 2 Ruby on Rails servers).

If you don't have the required permissions and quota, contact your tenancy administrator. See [Policy Reference](https://docs.cloud.oracle.com/en-us/iaas/Content/Identity/Reference/policyreference.htm), [Service Limits](https://docs.cloud.oracle.com/en-us/iaas/Content/General/Concepts/servicelimits.htm), [Compartment Quotas](https://docs.cloud.oracle.com/iaas/Content/General/Concepts/resourcequotas.htm).

## Deploy Using Oracle Resource Manager

1. Click [![Deploy to Oracle Cloud](https://oci-resourcemanager-plugin.plugins.oci.oraclecloud.com/latest/deploy-to-oracle-cloud.svg)](https://cloud.oracle.com/resourcemanager/stacks/create?region=home&zipUrl=https://github.com/haj/oci-arch-rubyonrails-mds/archive/refs/tags/latest.zip)

    If you aren't already signed in, when prompted, enter the tenancy and user credentials.

2. Review and accept the terms and conditions.

3. Select the region where you want to deploy the stack.

4. Follow the on-screen prompts and instructions to create the stack.

5. After creating the stack, click **Terraform Actions**, and select **Plan**.

6. Wait for the job to be completed, and review the plan.

    To make any changes, return to the Stack Details page, click **Edit Stack**, and make the required changes. Then, run the **Plan** action again.

7. If no further changes are necessary, return to the Stack Details page, click **Terraform Actions**, and select **Apply**. 

## Deploy Using the Terraform CLI

### Clone the Module

Now, you'll want a local copy of this repo. You can make that with the commands:

```
    git clone https://github.com/oracle-quickstart/oci-arch-rubyonrails-mds.git
    cd oci-arch-rubyonrails-mds
    ls
```

### Prerequisites
First off, you'll need to do some pre-deploy setup.  That's all detailed [here](https://github.com/cloud-partners/oci-prerequisites).

Create a `terraform.tfvars` file, and specify the following variables:

```
# Authentication
tenancy_ocid         = "<tenancy_ocid>"
user_ocid            = "<user_ocid>"
fingerprint          = "<finger_print>"
private_key_path     = "<pem_private_key_path>"

# Region
region = "<oci_region>"

# Availablity Domain 
availablity_domain_name = "<availablity_domain_name>"

# Compartment
compartment_ocid = "<compartment_ocid>"

# Number of Ruby on Rails nodes (optional)
numberOfNodes = 2

# MySQL DBSystem Admin Password 
mysql_db_system_admin_password = "<mysql_password>"

````

### Create the Resources
Run the following commands:

    terraform init
    terraform plan
    terraform apply


### Testing your Deployment
After the deployment is finished, you can test that your Ruby on Rails was deployed correctly and can access the Ruby on Rails demo page with MDS as database system. Pick up the value of the railsapp_url:

````
railsapp_url = http://193.122.204.54/
`````

Then copy it into Web browser. Here is the example of the succesfull outcome:

![](./images/todoapp.png)

As the load balancer alternates between the 2 Tomcat nodes, the session data should persist.

### Destroy the Deployment
When you no longer need the deployment, you can run this command to destroy the resources:

    terraform destroy

## Architecture Diagram

![](./images/architecture-deploy-tomcat-mds.png)

## Reference Archirecture

For details of the architecture, see [_Deploy Apache Tomcat connected to MySQL Database Service_](https://docs.oracle.com/en/solutions/deploy-tomcat-mysql/index.html)

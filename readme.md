
## Azure Bicep Virtual Machine (VM) Module Documentation

### Overview:

This module is designed to provision a complete Virtual Machine (VM) setup in Azure. It creates a storage account, a public IP address, a network interface, and the VM itself. The module provides customization based on `location`, `environment`, `purpose`, and several other configurations to cater to different scenarios.

### Module Parameters:

1. **location**:
   - Description: Azure region where the VM resources should be created.
   - Allowed values: 'westeurope', 'northeurope'
   - Type: String
   
2. **environment**:
   - Description: Indicates the type of environment for which the VM is being provisioned.
   - Allowed values: 'acceptance', 'production', 'integration'
   - Type: String

3. **purpose**:
   - Description: Specifies the intended use or role of the VM.
   - Type: String

4. **vmAdmin**:
   - Description: Administrator username for the VM.
   - Type: String

5. **vmSize**:
   - Description: Size or SKU of the VM.
   - Type: String

6. **bootStorage**:
   - Description: Name of the boot storage account.
   - Type: String

7. **vmAdminPassword**:
   - Description: Administrator password for the VM.
   - Type: String (Secure Parameter)

8. **osPublisher**, **osOffer**, **osSKU**, **osVersion**:
   - Description: Details specifying the OS image for the VM.
   - Type: String

### Resource Details:

1. **myStorage**:
   - Resource Type: `Microsoft.Storage/storageAccounts`
   - Function: Provides storage capabilities for the VM's boot diagnostics.

2. **myPIP**:
   - Resource Type: `Microsoft.Network/publicIPAddresses`
   - Function: Offers a dynamic public IP address for external connectivity.

3. **myNIC**:
   - Resource Type: `Microsoft.Network/networkInterfaces`
   - Function: Provides the network interface card, linking the VM to a subnet and associating the public IP.

4. **myVM**:
   - Resource Type: `Microsoft.Compute/virtualMachines`
   - Function: Represents the virtual machine resource, created with specified OS image, network, and storage configurations.

### Naming Convention:

Names for resources are derived from the given `location`, `environment`, and `purpose`. Examples include:
- Virtual Network Name: `'vnet-{environmentShortName}-{locationShortName}'`
- VM Name: `'vm-{environmentShortName}-{locationShortName}-{purpose}'`
- etc.

### GitHub Actions Pipelines:

#### 1. GitVersion Tag:
This pipeline automatically tags the repository using GitVersion when changes are pushed to the master branch.

- **Trigger**: Push to the master branch or manual dispatch.
- **Jobs**:
  - **build**:
    - Runs on: `ubuntu-latest`
    - Steps:
      - Checkout repository.
      - Automatically tag the repository based on the GitVersion configuration.

#### 2. Publish Module via action:
This pipeline publishes your Bicep module when a new tag is created.

- **Trigger**: Tag creation or manual dispatch.
- **Jobs**:
  - **build**:
    - Environment: `prd`
    - Runs on: `ubuntu-latest`
    - Steps:
      - Publish the Bicep module using given credentials and module details.

### Usage:

1. To utilize the VM module, reference it in your main Bicep file and provide necessary parameters.
2. Ensure that you have set up GitHub secrets (`AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID`) to be used by the "Publish Module via action" pipeline.
3. Follow naming conventions and allowed parameter values for smooth deployment.

### Best Practices:

1. Rotate the `vmAdminPassword` regularly and avoid using common or easily guessable passwords.
2. Limit the exposure of VMs to the internet. Utilize Azure security practices like NSGs (Network Security Groups) and ASGs (Application Security Groups).
3. Regularly review and update the GitHub Actions configurations to ensure CI/CD best practices and to integrate any new features or enhancements provided by GitHub.
4. Keep modules and configurations in sync with Azure's best practices and recommendations.

### Note:

Ensure you have necessary permissions to create Azure VM resources in your subscription and the chosen location. Also, regularly review and manage access to your GitHub repository to ensure security and compliance.


### steps to clone module for github

1. copy module
2. copy workflows / modify workflow parameters
3. create federated reference in app registration with proper environment
4. add secrets to github repo
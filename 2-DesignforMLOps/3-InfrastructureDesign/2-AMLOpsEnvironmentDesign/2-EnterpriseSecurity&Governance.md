---
sort: 2
---
# Enterprise security and governance for Azure Machine Learning
**Source:**[Enterprise security and governance for Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/concept-enterprise-security)

In this article, you'll learn about security and governance features available for Azure Machine Learning. These features are useful for administrators, DevOps, and MLOps who want to create a secure configuration that is compliant with your companies policies. With Azure Machine Learning and the Azure platform, you can:

* Restrict access to resources and operations by user account or groups
* Restrict incoming and outgoing network communications
* Encrypt data in transit and at rest
* Scan for vulnerabilities
* Apply and audit configuration policies

## Restrict access to resources and operations

[Azure Active Directory (Azure AD)](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis) is the identity service provider for Azure Machine Learning. It allows you to create and manage the security objects (user, group, service principal, and managed identity) that are used to _authenticate_ to Azure resources. Multi-factor authentication is supported if Azure AD is configured to use it.

Here's the authentication process for Azure Machine Learning using multi-factor authentication in Azure AD:

1. The client signs in to Azure AD and gets an Azure Resource Manager token.
1. The client presents the token to Azure Resource Manager and to all Azure Machine Learning.
1. Azure Machine Learning provides a Machine Learning service token to the user compute target (for example, Azure Machine Learning compute cluster). This token is used by the user compute target to call back into the Machine Learning service after the run is complete. The scope is limited to the workspace.

[![Authentication in Azure Machine Learning](media/concept-enterprise-security/authentication.png)](media/authentication.png#lightbox)

Each workspace has an associated system-assigned [managed identity](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview) that has the same name as the workspace. This managed identity is used to securely access resources used by the workspace. It has the following Azure RBAC permissions on associated resources:

| Resource | Permissions |
| ----- | ----- |
| Workspace | Contributor |
| Storage account | Storage Blob Data Contributor |
| Key vault | Access to all keys, secrets, certificates |
| Azure Container Registry | Contributor |
| Resource group that contains the workspace | Contributor |

The system-assigned managed identity is used for internal service-to-service authentication between Azure Machine Learning and other Azure resources. The identity token is not accessible to users and cannot be used by them to gain access to these resources. Users can only access the resources through [Azure Machine Learning control and data plane APIs](2-how-to-assign-roles.md), if they have sufficient RBAC permissions.

The managed identity needs Contributor permissions on the resource group containing the workspace in order to provision the associated resources, 
and to [deploy Azure Container Instances for web service endpoints](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-deploy-azure-container-instance).

We don't recommend that admins revoke the access of the managed identity to the resources mentioned in the preceding table. You can restore access by using the [resync keys operation](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-change-storage-access-key).

> [!NOTE]
> If your Azure Machine Learning workspaces has compute targets (compute cluster, compute instance, Azure Kubernetes Service, etc.) that were created __before May 14th, 2021__, you may also have an additional Azure Active Directory account. The account name starts with `Microsoft-AzureML-Support-App-` and has contributor-level access to your subscription for every workspace region.
> 
> If your workspace does not have an Azure Kubernetes Service (AKS) attached, you can safely delete this Azure AD account. 
> 
> If your workspace has attached AKS clusters, _and they were created before May 14th, 2021_, __do not delete this Azure AD account__. In this scenario, you must first delete and recreate the AKS cluster before you can delete the Azure AD account.

You can provision the workspace to use user-assigned managed identity, and grant the managed identity additional roles, for example to access your own Azure Container Registry for base Docker images. For more information, see [Use managed identities for access control](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-use-managed-identities).

You can also configure managed identities for use with Azure Machine Learning compute cluster. This managed identity is independent of workspace managed identity. With a compute cluster, the managed identity is used to access resources such as secured datastores that the user running the training job may not have access to. For more information, see [Identity-based data access to storage services on Azure](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-identity-based-data-access).

> [!TIP]
> There are some exceptions to the use of Azure AD and Azure RBAC within Azure Machine Learning:
> * You can optionally enable __SSH__ access to compute resources such as Azure Machine Learning compute instance and compute cluster. SSH access is based on public/private key pairs, not Azure AD. SSH access is not governed by Azure RBAC.
> * You can authenticate to models deployed as web services (inference endpoints) using __key__ or __token__-based authentication. Keys are static strings, while tokens are retrieved using an Azure AD security object. For more information, see [Configure authentication for models deployed as a web service](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-identity-based-data-access).

For more information, see the following articles:
* [Authentication for Azure Machine Learning workspace](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-setup-authentication)
* [Manage access to Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-assign-roles)
* [Connect to storage services](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-access-data)
* [Use Azure Key Vault for secrets when training](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-use-secrets-in-runs)
* [Use Azure AD managed identity with Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-use-managed-identities)
* [Use Azure AD managed identity with your web service](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-use-azure-ad-identity)

## Network security and isolation

To restrict network access to Azure Machine Learning resources, you can use [Azure Virtual Network (VNet)](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview). VNets allow you to create network environments that are partially, or fully, isolated from the public internet. This reduces the attack surface for your solution, as well as the chances of data exfiltration.

You might use a virtual private network (VPN) gateway to connect individual clients, or your own network, to the VNet

The Azure Machine Learning workspace can use [Azure Private Link](https://docs.microsoft.com/en-us/azure/private-link/private-link-overview) to create a private endpoint behind the VNet. This provides a set of private IP addresses that can be used to access the workspace from within the VNet. Some of the services that Azure Machine Learning relies on can also use Azure Private Link, but some rely on network security groups or user-defined routing.

For more information, see the following documents:

* [Virtual network isolation and privacy overview](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-network-security-overview)
* [Secure workspace resources](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-secure-workspace-vnet)
* [Secure training environment](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-secure-training-vnet)
* [Secure inference environment](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-secure-inferencing-vnet)
* [Use studio in a secured virtual network](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-enable-studio-virtual-network)
* [Use custom DNS](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-custom-dns)
* [Configure firewall](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-access-azureml-behind-firewall)

<a id="encryption-at-rest"></a><a id="azure-blob-storage"></a>

## Data encryption

Azure Machine Learning uses a variety of compute resources and data stores on the Azure platform. To learn more about how each of these supports data encryption at rest and in transit, see [Data encryption with Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/concept-data-encryption).

When deploying models as web services, you can enable transport-layer security (TLS) to encrypt data in transit. For more information, see [Configure a secure web service](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-secure-web-service).

## Vulnerability scanning

[Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-introduction) provides unified security management and advanced threat protection across hybrid cloud workloads. For Azure machine learning, you should enable scanning of your [Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-intro) resource and Azure Kubernetes Service resources. For more information, see [Azure Container Registry image scanning by Security Center](https://docs.microsoft.com/en-us/azure/security-center/defender-for-container-registries-introduction) and [Azure Kubernetes Services integration with Security Center](https://docs.microsoft.com/en-us/azure/security-center/defender-for-kubernetes-introduction).

## Audit and manage compliance

[Azure Policy](https://docs.microsoft.com/en-us/azure/governance/policy/) is a governance tool that allows you to ensure that Azure resources are compliant with your policies. You can set policies to allow or enforce specific configurations, such as whether your Azure Machine Learning workspace uses a private endpoint. For more information on Azure Policy, see the [Azure Policy documentation](https://docs.microsoft.com/en-us/azure/governance/policy/overview). For more information on the policies specific to Azure Machine Learning, see [Audit and manage compliance with Azure Policy](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-integrate-azure-policy).
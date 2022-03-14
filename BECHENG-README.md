# Notes re the fhir-proxy for learning/upskilling

## Running the Proxy on an AIRS sub 
Based on the security restrictions that comes with our AIRS, I cannot grant admin access to application level permissions that are required to run the fhir-proxy with the fhir-api.
As a workaround: 
1.  Deployed the fhir-api and fhir-proxy as usual in my AIRS (e.g. [link](https://github.com/microsoft/azure-healthcare-apis-workshop/tree/main/Challenge-01%20-%20Deploy%20Azure%20API%20for%20FHIR%20(PaaS)%2C%20FHIR-Proxy%20(OSS)%2C%20and%20FHIR-Bulk%20Loader%20(OSS)))  
2.  Created the fhir-proxy service princinal in secondary AAD tenant (spawned from my AIRS).  Important:  Leave the fhir-api's **Authenication** as is, i.e. using [Azure RBAC](https://docs.microsoft.com/en-us/azure/healthcare-apis/azure-api-for-fhir/configure-azure-rbac#confirm-azure-rbac-mode) (and not [Local RBAC](https://docs.microsoft.com/en-us/azure/healthcare-apis/azure-api-for-fhir/configure-local-rbac)).
3.  Ensured the fhir-proxy Managed Service Identity (MSI) was enabled and the had access to the fhir-api as a "Fhir Data Contributor" within its IAM blade of the portal.
3.  Modified the fhir-proxy "Configuration" aka App Settings to ensure auth between the proxy and the api was using the MSI, by making sure _FS-RESOURCE_ was using the correct fhir reource, e.g. `https://<fhir-api-url>..azurehealthcareapis.com` and the FS-CLIENT-ID, FS-SECRET and FS-TENANT-NAME 

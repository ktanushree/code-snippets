## Child Tenant Retrieval using Parent Service Account

#### Description
Code Snippet to retrieve all child tenants details (name, tsg ID, parent TSG ID) part of the parent tenant hierarchy using the prisma_sase SDK

#### Requirement
Environment with the latest version of Prisma SASE SDK installed
Python >  3.7 or higher

#### Assumptions:
- prisma_sase SDK is installed
  
#### Code Snippet
```
############################
# Import prisma_sase SDK
############################
import prisma_sase

############################
# Instantiate SDK object
############################
parent_session = prisma_sase.API()

#####################################################################
# Authenticate using a Service Account created at the Parent Tenant
#####################################################################
client_id = "paste client ID"
client_secret = "paste client secret"
parent_tsg_id = "paste TSG ID"

parent_session.interactive.login_secret(client_id=client_id, client_secret=client_secret, tsg_id=parent_tsg_id)

######################################################################################
# Retrieve child tenants
# Iterate through child tenants and use child TSG ID to connect to child tenant
######################################################################################

url_tenantlist = "https://api.sase.paloaltonetworks.com/tenancy/v1/tenant_service_groups"
resp = parent_session.rest_call(url=url_tenantlist, method="GET")
if resp.cgx_status:
    itemlist = resp.cgx_content.get("items", None)
    for item in itemlist:
        print("Connecting to Child Tenant: {}[{}]".format(item["display_name"], 
                                                          item["id"]))
        child_session = prisma_sase.API()
        child_session.interactive.login_secret(client_id=client_id, 
                                          client_secret=client_secret, 
                                          tsg_id=item["id"])

        #
        # Use child_session to access child tenant data
        # for eg: resp = child_session.get.sites()
        #

else:
    print("ERR: Could not retrieve TSG list")
    prisma_sase.jd_detailed(resp)

```

The child_session object can now be used for making API calls on the child tenant. The parent_session object be used for making API calls on the parent tenant. 




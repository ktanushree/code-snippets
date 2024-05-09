# Authentication using Prisma SASE SDK (prisma_sase)

### Description
Code Snippet to instantiate and authenticate using the prisma_sase SDK

### Requirement
Environment with the latest version of Prisma SASE SDK installed
Python >  3.7 or higher

### Assumptions:
- prisma_sase SDK is installed
  
### Code Snippet
```
############################
# Import prisma_sase SDK
############################
import prisma_sase

############################
# Instantiate SDK object
############################
sase_session = prisma_sase.API()

#######################################
# Authenticate using Service Account 
#######################################
client_id = "paste client ID"
client_secret = "paste client secret"
tsg_id = "paste TSG ID"

sase_session.interactive.login_secret(client_id=client_id, client_secret=client_secret, tsg_id=tsg_id)

```

The sase_session object can now be used for making API calls.



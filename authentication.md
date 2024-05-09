## Authentication using Prisma SASE SDK (prisma_sase)

### Description
Code Snippet to instantiate and authenticate using the prisma_sase SDK

### Requirement
Environment with the latest version of Prisma SASE SDK installed
Python >  3.7 or higher

### Assumptions:
- prisma_sase SDK is installed
  
### Code Snippet
```ruby
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


## Authentication using CloudGenix SDK (cloudgenix)

### Description
Code Snippet to instantiate and authenticate using the cloudgenix SDK

### Requirement
Environment with the latest version of CloudGenix SDK installed
Python >  3.7 or higher

### Assumptions:
- cloudgenix SDK is installed
- Auth token is created via the Prisma SDWAN portal. Make sure IP Session Lock is disabled if you intend to share the token or use the token on a different machine or a VM.
  
### Code Snippet
```ruby
############################
# Import cloudgenix SDK
############################
import cloudgenix

############################
# Instantiate SDK object
############################
cgx_session = cloudgenix.API()

#######################################
# Authenticate using Auth Token 
#######################################
auth_token = "paste auth token"

cgx_session.interactive.use_token(token=auth_token)

```

The cgx_session object can now be used for making API calls.




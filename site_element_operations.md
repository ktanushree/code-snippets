### Description
Code Snippet to retrieve elements assigned to a site (**element_query** API)

### Requirement
Environment with the latest version of Prisma SASE SDK or CloudGenix SDK installed
Python >  3.7 or higher

### Assumptions:
- sdk is the instantiated SDK object. 
- Authentication to the Prisma SDWAN endpoint has completed.
  
### Code Snippet

```ruby
resp = sdk.get.sites()
if resp.cgx_status:
    sitelist = resp.cgx_content.get("items", None)

    for site in sitelist:
        data = {
            "query_params": {
                "site_id": {"in": [site["id"]]}
            }
        }
        resp = sdk.post.element_query(data=data)
        if resp.cgx_status:
            elements = resp.cgx_content.get("items", None)

            
            ####################################################
            # Iterate through elements to retrieve element data
            ####################################################
            
        else:
            print("ERR: Element Query Failed for site {}".format(site["name"]))
            prisma_sase.jd_detailed(resp)
else:
    print("ERR: Could not retrieve sites")
    prisma_sase.jd_detailed(resp)
```
                            
### Description
Code Snippet to retrieve elements matching a name (**element_query** API)

### Requirement
Environment with the latest version of Prisma SASE SDK or CloudGenix SDK installed
Python >  3.7 or higher

### Assumptions:
- sdk is the instantiated SDK object. 
- Authentication to the Prisma SDWAN endpoint has completed.
  
### Code Snippet

```ruby
data = {
    "query_params":{
        "or":{
            "name":{"re":"branch1"}
        }
    },
    "retrieved_fields_mask":False,
    "retrieved_fields":[],
    "sort_params":{"name":"asc"},
    "dest_page":1,
    "limit":100
}

resp = sdk.post.element_query(data=data)
prisma_sase.jd_detailed(resp)
```
  
                            

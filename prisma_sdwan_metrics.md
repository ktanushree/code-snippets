## Interface Stats Retrieval

### Description
Code Snippet to retrieve Interface Stats for the past hour from WAN interfaces from all devices on your tenant

### Requirement
Environment with the latest version of Prisma SASE SDK or CloudGenix SDK installed
Python >  3.7 or higher

### Assumptions:
- sdk is the instantiated SDK object. 
- Authentication to the Prisma SDWAN endpoint has completed.
  
### Code Snippet

```ruby
resp = sdk.get.elements()
if resp.cgx_status:
    elemlist = resp.cgx_content.get("items", None)
    for elem in elemlist:
        if elem["site_id"] in ["1", 1]:
            continue
        resp = sdk.get.interfaces(site_id=elem["site_id"], element_id=elem["id"])
        if resp.cgx_status:
            print("{}: {} retrieved interfaces".format(datetime.datetime.utcnow(), elem["name"]))
            interfaces = resp.cgx_content.get("items", None)

            interfacelist = []
            for intf in interfaces:
                if intf["used_for"] in ["public", "private"]:
                    interfacelist.append(intf["id"])

            #####################
            # Intf Stats
            #####################
            if len(interfacelist) > 0:
                data = {
                    "start_time": "2024-05-06T00:00:00Z",
                    "end_time": "2024-05-06T01:00:00Z",
                    "interval": "1min",
                    "metrics": [
                        {"name": "InterfaceBandwidthUsage", "statistics": ["average"], "unit": "Mbps"},
                        {"name": "InterfaceDroppedPackets", "statistics": ["sum"], "unit": "count"},
                        {"name": "InterfaceErrors", "statistics": ["sum"], "unit": "count"}
                    ],
                    "filter": {
                        "site": [elem["site_id"]],
                        "element": [elem["id"]],
                        "interface": interfacelist
                    }
                }

                resp = sdk.post.sys_metrics_monitor(data=data)
                if resp.cgx_status:
                    print("\t{}: Interface stats received\n\n".format(datetime.datetime.utcnow()))
                else:
                    print("ERR: Could not retrieve interface stats")
                    prisma_sase.jd_detailed(resp)
            else:
                print("\tNo Interfaces to query. Skipping..\n\n")

        else:
            print("ERR: Interfaces")
            prisma_sase.jd_detailed(resp)
else:
    print("ERR: Elements")
    prisma_sase.jd_detailed(resp)
```

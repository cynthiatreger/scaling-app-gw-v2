# Scaling with Azure Application Gateway v2

## Microsoft documentation recap

The AppGW autoscaling SKU (v2) can scale up to **125 instances** (see [App GW limits](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits#application-gateway-limits)).

Each [instance](https://learn.microsoft.com/en-us/azure/application-gateway/understanding-pricing#instance-count) can support at least 10 capacity units. The number of capacity unit per instance depends on the application processing requirement (I couldn't find any max value).

A single [capacity unit](https://learn.microsoft.com/en-us/azure/application-gateway/understanding-pricing#capacity-unit) is composed of **1 compute unit + 2500 persistent connections + 2,22 Mbps of throughput**:

- Each [compute unit](https://learn.microsoft.com/en-us/azure/application-gateway/understanding-pricing#compute-unit) supports ~50 connection requests/sec (10 with WAFv2)
- If any of the 3 above parameters (compute unit capacity, # connections, throughput) is exceeded, additional capacity units get triggered, as stated: 

&emsp;<img width="639" alt="image" src="https://user-images.githubusercontent.com/110976272/217456836-0f1849e5-e82c-4ca1-b1d8-6084f847c13a.png">

:arrow_right: See [Application Gateway high traffic volume support | Microsoft Learn](https://learn.microsoft.com/en-us/azure/application-gateway/high-traffic-support#scaling-for-application-gateway-v1-sku-standardwaf-sku) for further recommandations (max instance count set to 125 + min instance count based on the average compute unit usage).

💡 For a deep dive on Application Gateway, check out this great [AppGW MicroHack](https://github.com/dawlysd/azure-application-gateway-microhack) by [David Santiago](https://github.com/dawlysd)!
##
## Capacity Unit baseline

| Capacity unit | Compute unit | Persistent connections | Throughput (Mbps) | Connection requests/sec (Standard v2) | Connection request/sec (WAF v2) |
|---|---|---|---|---|---|
| 1 | 1 | 2500 | 2,22 | 50 | 10|

## Scaling table

| Instance | (Min) capacity unit | Persistent connections | Throughput | Connection requests/sec (Standard v2) | Connection request/sec (WAF v2) |
|---|---|---|---|---|---|
| 1 | 10 | 25000 | 22 Mbps | 500 | 100 |
| 25 | 250 | 625,000 | 555 Mbps | 12,500 | 2,500 |
| 50 | 500 | 1,250,000 | 1,11 Gbps | 25,000 | 5,000 |
| 75 | 750 | 1,875,000 | 1,665 Gbps | 37,500 | 7,500 |
| 100 | 1000 | 2,500,000 | 2,22 Gbps | 50,000 | 10,000 |
| 125 | 1250 | 3,125,000 | 2,775 Gbps | 62,500 |12,500 |

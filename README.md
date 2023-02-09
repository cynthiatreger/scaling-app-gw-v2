# Scaling with Azure Application Gateway v2

The AppGW autoscaling SKU (v2) can scale up to **125 instances** (see [App GW limits](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits#application-gateway-limits)).

Each [instance](https://learn.microsoft.com/en-us/azure/application-gateway/understanding-pricing#instance-count) can support at least 10 capacity units. The number of capacity unit per instance depends on the application processing requirement (I couldn't find any max value).

A single [capacity unit](https://learn.microsoft.com/en-us/azure/application-gateway/understanding-pricing#capacity-unit) is composed of **1 compute unit + 2500 persistent connections + 2,22 Mbps of throughput**:

- Each [compute unit](https://learn.microsoft.com/en-us/azure/application-gateway/understanding-pricing#compute-unit) supports ~50 connection requests/sec (10 with WAFv2)
- As stated: 

&emsp;<img width="639" alt="image" src="https://user-images.githubusercontent.com/110976272/217456836-0f1849e5-e82c-4ca1-b1d8-6084f847c13a.png">

&emsp;if any of the 3 above parameters (compute unit capacity, # connections, throughput) is exceeded, additional capacity units get triggered.

:arrow_right: See [Application Gateway high traffic volume support | Microsoft Learn](https://learn.microsoft.com/en-us/azure/application-gateway/high-traffic-support#scaling-for-application-gateway-v1-sku-standardwaf-sku) for further recommandations (max instance count set to 125 + min instance count based on the average compute unit usage).

:arrow_right: For a deep dive on Application Gateway, check out this great [AppGW MicroHack](https://github.com/dawlysd/azure-application-gateway-microhack) by [David Santiago](https://github.com/dawlysd)!

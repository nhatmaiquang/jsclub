# Azure Service Level Agreements (SLAs)

## Service-level agreements (SLAs)

Microsoft maintains its commitment to providing customers with high-quality products and services by adhering to comprehensive operational policies, standards, and practices. Formal documents known as Service-Level Agreements (SLAs) capture the specific terms that define the performance standards that apply to Azure.

![alt text](https://training.future-proof.net/assets/courseware/v1/3d31bd2dc8a6127a2e66b2ddb22beb9a/asset-v1:FP+AZ-900+2019_T1+type@asset+block/0405-sla-icon.png)

- SLAs describe Microsoft's commitment to providing Azure customers with certain performance standards.
- There are SLAs for individual Azure products and services.
- SLAs also specify what happens if a service or product fails to perform to a governing SLA's specification.

## SLAs for Azure products and services

There are three key characteristics of SLAs for Azure products and services, which the following sections detail.

**1. Performance Targets, Uptime and Connectivity Guarantees**

A SLA defines performance targets for an Azure product or service. The performance targets that a SLA defines are specific to each Azure product and service.

For example, performance targets for some Azure services are expressed in terms of uptime or connectivity rates.

![alt text](https://training.future-proof.net/assets/courseware/v1/cb1a1c18d2a2ffdbe59f3489fe7bd59f/asset-v1:FP+AZ-900+2019_T1+type@asset+block/0405-sla-target.png)

**2. Performance targets range from 99.9 percent to 99.99 percent**

A typical SLA specifics performance-target commitments that range from 99.9 percent (“three nines”) to 99.99 percent ("four nines"), for each corresponding Azure product or service. These targets can apply to such performance criteria as uptime, or response times for services.

For example, the SLA for the Azure Database for MySQL service guarantees 99.99 percent uptime. The Azure Cosmos DB (Database) service SLA offers 99.99 percent uptime, which includes low-latency commitments of less than 10 milliseconds on DB read operations and less than 15 milliseconds on DB write operations.

![alt text](https://training.future-proof.net/assets/courseware/v1/b8459523129de431b614b895e4863826/asset-v1:FP+AZ-900+2019_T1+type@asset+block/0405-sla-range.png)

**3. Service Credits**

SLAs also describe how Microsoft will respond if an Azure product or service fails to perform to its governing SLA's specification.

For example, customers may have a discount applied to their Azure bill, as compensation for an under-performing Azure product or service. The table below explains this example in more detail.

The first column in the table below shows monthly uptime percentage SLA targets for a single instance Azure Virtual Machine. The second column shows the corresponding service credit amount you receive, if the **actual** uptime is less than the specified SLA target for that month.

**MONTHLY UPTIME PERCENTAGE**  | **SERVICE CREDIT PERCENTAGE**
------------------------------ | -----------------------------
< 99.9                         | 10
< 99                           | 25
< 95                           | 100

**Note:** Azure does not provide SLAs for many services under the Free or Shared tiers. Also, free products such as Azure Advisor do not typically have a SLA.

**Note:** For more information about specific Azure SLAs for individual products and services, refer to [Service Level Agreements](https://azure.microsoft.com/en-us/support/legal/sla/summary/).

## Composite SLAs

When combining SLAs across different service offerings, the resultant SLA is a called a **_Composite SLA_**. The resulting composite SLA can provide higher or lower uptime values, depending on your application architecture.

Consider an App Service web app that writes to Azure SQL Database. At the time of this writing, these Azure services have the following SLAs:

- App Service Web Apps is 99.95 percent.
- SQL Database is 99.99 percent.

![alt text](https://training.future-proof.net/assets/courseware/v1/734784f5fe5b21924d435b0dfcac24f4/asset-v1:FP+AZ-900+2019_T1+type@asset+block/0405-sla-compositesla1.png)

### Maximum downtime you would expect for this example application

In the example above, if either service fails the whole application will fail. In general, the individual probability values for each service are independent. However, the composite SLA value for this application is: `99.9 percent × 99.99 percent = 99.94 percent`. This means the combined probability of failure value is lower than the individual SLA values. This isn't surprising, because an application that relies on multiple services has more potential failure points.

Conversely, you can improve the composite SLA by creating independent fallback paths. For example, if SQL Database is unavailable, you can put transactions into a queue for processing at a later time.

![alt text](https://training.future-proof.net/assets/courseware/v1/df883cd254dddebe81fda657ee89f493/asset-v1:FP+AZ-900+2019_T1+type@asset+block/0405-sla-compositesla2.png)

With the design shown in the image above, the application is still available even if it can't connect to the database. However, it fails if both the database **_and_**the queue fail simultaneously. If the expected percentage of time for a simultaneous failure is `0.0001 × 0.001`, the composite SLA for this combined path would be:

Database **_OR_** queue = `1.0 − (0.0001 × 0.001) = 99.99999 percent`

Therefore, the total composite SLA is:

Web app **_AND_** database **_OR_** queue = `99.95 percent × 99.99999 percent = ~99.95 percent`

However, there are tradeoffs to using this approach. The application logic is more complex, you are paying for the queue, and there may be data-consistency issues.

## Improving application SLAs

Azure customers can use SLAs to evaluate how their Azure solutions meet their business requirements and the needs of their clients and users. By creating your own SLAs, you can set performance targets to suit your specific Azure application. This is an **_Application SLA_**.

### Understand your requirements

Building an efficient and reliable Azure solution requires knowing your workload requirements. You then can select Azure products and services, and provision resources according to those requirements. To apply your solution successfully, it is important to understand the Azure SLAs that define performance targets for the Azure products and services within your solution. This understanding will help you create achievable Application SLAs.

In a distributed system, failures will happen. Hardware can fail. The network can have transient failures. It is rare for an entire service or region to experience a disruption, but even this must be planned for.

### Resiliency

**_Resiliency_** is the ability of a system to recover from failures and continue to function. It's not about avoiding failures, but responding to failures in a way that avoids downtime or data loss. The goal of resiliency is to return the application to a fully functioning state following a failure. High availability and disaster recovery are two important components of resiliency.

When designing your architecture you need to design for resiliency, and you should perform a **_Failure Mode Analysis_** (FMA). The goal of a FMA is to identify possible points of failure, and to define how the application will respond to those failures.

### Cost and complexity vs. high availability

**_Availability_** refers to time that a system is functional and working. Maximizing availability requires implementing measures to prevent possible service failures. However, devising preventative measures can be difficult and expensive, and often results in very complex solutions.

As your solution grows in complexity, you will have more services depending on each other. Therefore, you might overlook possible failure points in your solution if you have several interdependent services.

![alt text](https://training.future-proof.net/assets/courseware/v1/6a57bec534caa8e858f04db451d4b3fa/asset-v1:FP+AZ-900+2019_T1+type@asset+block/0405-sla-complex-scenario.png)

- For example: A workload that requires **_99.99 percent uptime shouldn't depend upon a service with a 99.9 percent SLA_**.

Most providers prefer to maximize the availability of their Azure solutions by minimizing downtime. However, as you increase availability, you also increase the cost and complexity of your solution.

- For example: An SLA that defines an **_uptime of 99.99% only allows for about 5 minutes of total downtime per month_**.

The risk of potential downtime is cumulative across various SLA levels, which means that complex solutions can face greater availability challenges. Therefore, how critical high availability is to your requirements will determine how you handle the addition of complexity and cost to your application SLAs.

The following table lists the potential cumulative downtime for various SLA levels over different durations:

**SLA percentage**  | **Downtime per week**  | **Downtime per month**  | **Downtime per year**
------------------- | ---------------------- | ----------------------- | ---------------------
99                  | 1.68 hours             | 7.2 hours               | 3.65 days
99.9                | 10.1 minutes           | 43.2 minutes            | 8.76 hours
99.95               | 5 minutes              | 21.6 minutes            | 4.38 hours
99.99               | 1.01 minutes           | 4.32 minutes            | 52.56 minutes
99.999              | 6 seconds              | 25.9 seconds            | 5.26 minutes

### Considerations for defining application SLAs

- If your application SLA defines four 9's (99.99 percent) performance targets, recovering from failures by manual intervention may not be enough to fulfill your SLA. Your Azure solution must be self-diagnosing and self-healing instead.
- It is difficult to respond to failures quickly enough to meet SLA performance targets above four 9's.
- Carefully consider the time window against which your application SLA performance targets are measured. The smaller the time window, the tighter the tolerances. If you define your application SLA in terms of hourly or daily uptime, you need to be aware that these tighter tolerances might not allow for achievable performance targets.

**Note**: For more information about improving application SLAs, refer to [Designing resilient applications for Azure](https://docs.microsoft.com/vi-vn/azure/architecture/reliability/).

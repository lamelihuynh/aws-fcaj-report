---
title: "Blog 3"
date: 2026-04-02
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Optimizing data transfer costs when using AWS Network Load Balancer

**Source:** [AWS Networking & Content Delivery Blog](https://aws.amazon.com/blogs/networking-and-content-delivery/optimizing-data-transfer-costs-when-using-aws-network-load-balancer/)

**Authors:** Luis Felipe Silveira da Silva, Lucas Rolim

**Published:** April 02, 2026

**Categories:** Best Practices, Cloud Cost Optimization, Elastic Load Balancing, Networking & Content Delivery

**Main services and concepts:** AWS Network Load Balancer, Elastic Load Balancing, Availability Zones, Amazon Route 53 Resolver, cross-zone load balancing, zonal affinity, Amazon VPC, AWS Transit Gateway, VPC peering, AWS Network Manager

---

## Introduction

This article focuses on one practical cost issue that appears in multi-AZ application networking: data transfer between Availability Zones when using an AWS Network Load Balancer. NLB is often selected for high-performance TCP, TLS or UDP traffic, but the placement of clients, NLB nodes and targets can still create inter-zone traffic charges.

The main technical message is simple: the cheapest traffic path is the path that stays inside the same Availability Zone. To get closer to that pattern, the article combines two configuration ideas:

- keep client-to-NLB traffic local with Availability Zone DNS affinity,
- keep NLB-to-target traffic local by controlling cross-zone load balancing.

![NLB data transfer cost overview](/images/3-BlogsPosted/blog3-00-nlb-cost-hero.png)

## Inter-zone data transfer cost model

When traffic crosses an Availability Zone boundary, AWS applies inter-zone data transfer charging. The article uses the rate of `0.01 USD per GB` at each side of the transfer, which means one cross-AZ hop is shown as `0.02 USD per GB`. In production, this number should still be checked against the current AWS pricing page before estimating a real bill.

The cost depends on where the client, NLB node and target are placed:

- Client in AZ A, NLB in AZ B, target in AZ B: the client-to-NLB hop crosses AZs, so the article counts `0.02 USD per GB`. The NLB-to-target hop stays in AZ B.
- Client in AZ A, NLB in AZ B, target in AZ A: traffic crosses from client to NLB and then from NLB back to the target AZ. The article counts two cross-AZ hops, or `0.04 USD per GB` in each direction for that path.
- Client, NLB and target all in the same AZ: no inter-zone data transfer charge is created by that flow.

![Client in another AZ while NLB and target stay together](/images/3-BlogsPosted/blog3-01-client-nlb-cross-az.png)

*Figure 1 - The client is in a different AZ, while the NLB and target are in the same AZ.*

![Traffic crosses AZ boundaries twice](/images/3-BlogsPosted/blog3-02-double-cross-az.png)

*Figure 2 - The client, NLB and target placement can create two cross-AZ hops.*

![Client, NLB and target stay in the same AZ](/images/3-BlogsPosted/blog3-03-same-az-no-transfer.png)

*Figure 3 - Same-AZ placement avoids inter-zone data transfer for the request path.*

## Client-to-NLB traffic

The first part of the architecture is the path from the client to the NLB. The article describes an internal NLB accessed by clients inside the same VPC, but the same placement logic also applies when clients reach the NLB through AWS Transit Gateway or VPC peering.

By default, an NLB uses `0 percent zonal affinity` for its DNS behavior. With this setting, Amazon Route 53 Resolver can return healthy NLB IP addresses from any AZ where the NLB is active. This usually helps spread traffic, but it also means a client in AZ A can connect to an NLB Elastic Network Interface in AZ B and create inter-zone traffic.

To reduce that cost, the article recommends enabling `100 percent zonal affinity` when the workload is designed for zone-local routing. With this setting, DNS responses prefer the NLB IP address in the same AZ as the client. If the client AZ has no healthy NLB IP address, DNS can still return an address from another zone so the application can continue to work.

### Trade-offs

Zonal affinity reduces cross-AZ traffic, but it also changes distribution behavior. If most clients are located in one AZ, then the NLB node and targets in that AZ can receive more traffic than the others. This imbalance can become more visible when cross-zone load balancing is disabled.

The operational response is capacity planning by AZ:

- keep client distribution reasonably balanced across AZs when possible,
- follow DNS and TCP connection reuse practices so clients do not pin traffic in unexpected ways,
- size target capacity per AZ according to expected traffic,
- use services such as Amazon EC2 Auto Scaling, Amazon ECS, Amazon EKS or AWS Elastic Beanstalk to adjust capacity.

### Turning on zonal affinity

In the AWS Management Console, the setting is under the NLB attributes:

1. Open the Amazon EC2 console.
2. Go to **Load Balancers**.
3. Choose the NLB.
4. Open the **Attributes** tab and choose **Edit**.
5. Under **Availability Zone routing configuration**, set **Client routing policy (DNS record)** to **Availability Zone affinity** or **Partial Availability Zone affinity**.
6. Save the change.

![Setting Availability Zone affinity for NLB](/images/3-BlogsPosted/blog3-04-nlb-az-affinity.png)

*Figure 4 - Client routing policy for NLB DNS records.*

The same idea can be applied with AWS CLI by modifying the `dns_record.client_routing_policy` attribute:

```bash
aws elbv2 modify-load-balancer-attributes \
  --load-balancer-arn <nlb-arn> \
  --attributes Key=dns_record.client_routing_policy,Value=availability_zone_affinity
```

## NLB-to-target traffic

The second part of the architecture is the path from the NLB node to registered targets. This is controlled by cross-zone load balancing at the load balancer and target group levels.

When cross-zone load balancing is enabled, an NLB node can send traffic to registered targets in any enabled AZ. That can make target utilization more even, but it can also create inter-zone data transfer whenever the selected target is in another AZ.

For Network Load Balancers, cross-zone load balancing is disabled by default at the load balancer and target group levels. That default supports zone-local routing, but the target layout must be planned carefully. If one AZ has fewer healthy targets than another AZ, disabling cross-zone load balancing can make the busy AZ run hot.

### Capacity considerations

For a cost-optimized NLB design, each enabled AZ should have enough local targets to serve the traffic expected in that zone. The target count does not always need to be identical, but it should be proportional to the client traffic and application capacity plan for that AZ.

Important checks:

- verify target health separately per AZ,
- compare request volume by NLB Availability Zone,
- compare target CPU, memory, connections and errors per AZ,
- scale targets per zone when the client distribution is not even,
- decide explicitly whether the target group should inherit the load balancer setting or override it.

### Turning off cross-zone load balancing

At the load balancer level, the console path is:

1. Open the Amazon EC2 console.
2. Under **Load Balancing**, choose **Load Balancers**.
3. Select the NLB.
4. Open **Attributes** and choose **Edit**.
5. Turn **Cross-zone load balancing** off.
6. Save the change.

![Disabling cross-zone load balancing for NLB](/images/3-BlogsPosted/blog3-05-disable-cross-zone-nlb.png)

*Figure 5 - Cross-zone load balancing disabled at the NLB level.*

The matching AWS CLI command uses `load_balancing.cross_zone.enabled`:

```bash
aws elbv2 modify-load-balancer-attributes \
  --load-balancer-arn <nlb-arn> \
  --attributes Key=load_balancing.cross_zone.enabled,Value=false
```

At the target group level, the same attribute can be managed from **Target Groups > Attributes > Edit**. For the cost-optimized local-AZ pattern, the target group policy should be consistent with the intended cross-zone behavior of the load balancer.

![Disabling cross-zone load balancing for NLB target group](/images/3-BlogsPosted/blog3-06-disable-cross-zone-target-group.png)

*Figure 6 - Cross-zone load balancing setting at the target group level.*

CLI example for a target group:

```bash
aws elbv2 modify-target-group-attributes \
  --target-group-arn <target-group-arn> \
  --attributes Key=load_balancing.cross_zone.enabled,Value=false
```

## Availability Zone Independence

The article also connects the cost optimization pattern to Availability Zone Independence. AZI means the application can keep serving traffic in each AZ without depending on another AZ for the request path.

For NLB, the pattern is:

- enable 100 percent zonal affinity so clients prefer the NLB IP in their own AZ,
- disable cross-zone load balancing so the NLB node sends traffic to targets in the same AZ,
- keep healthy and sufficient targets in every enabled AZ,
- prepare zonal evacuation procedures for a failed or impaired AZ.

![Availability Zone Independence with NLB](/images/3-BlogsPosted/blog3-07-azi-with-nlb.png)

*Figure 7 - Availability Zone Independence pattern with NLB.*

This configuration can reduce data transfer cost and can also reduce packet latency because the request path avoids unnecessary AZ boundaries. The article points to Infrastructure Performance in AWS Network Manager as a way to monitor real-time inter-AZ latency.

## Report notes

This blog is useful for the report because it shows that cost optimization is not only about choosing cheaper services. Network architecture and traffic placement can change the bill even when the same services are used.

Key points to reuse:

- NLB cost review should include client placement, NLB subnet placement and target placement.
- `0 percent zonal affinity` gives broad DNS distribution, while `100 percent zonal affinity` favors local-AZ routing.
- Cross-zone load balancing improves distribution but can introduce inter-zone data transfer.
- Disabling cross-zone load balancing requires target capacity to be balanced per AZ.
- Availability Zone Independence supports both cost control and resilience, but it needs operational monitoring.

## Implementation checklist

For a real workload, the configuration should be checked in this order:

1. Map the request path from client to NLB to target.
2. Identify how often traffic crosses AZ boundaries.
3. Check the NLB DNS client routing policy.
4. Check cross-zone load balancing at the load balancer level.
5. Check cross-zone behavior at the target group level.
6. Confirm target capacity per AZ.
7. Monitor NLB metrics, target health, target utilization and inter-AZ latency.
8. Recalculate the cost after traffic patterns change.

## Facebook Sharing Evidence

The technical summary was shared to the AWS Study Group VN Facebook community and is currently pending group approval. This screenshot is kept as temporary evidence until the public post URL is available.

![Facebook pending approval evidence for Blog 3](/images/3-BlogsPosted/blog3-fb-pending.png)

## About the authors

![Luis Felipe Silveira da Silva](/images/3-BlogsPosted/blog3-author-luis-felipe.jpg)

**Luis Felipe Silveira da Silva** is a Principal Solutions Architect in the AWS Application Networking team, based in Dublin. His work focuses on helping customers design resilient workloads with AWS networking and load balancing services.

![Lucas Rolim](/images/3-BlogsPosted/blog3-author-lucas-rolim.jpg)

**Lucas Rolim** is a Senior Solutions Architect at AWS, based in Sydney, working with the Application Networking team. His technical focus includes networking and security.

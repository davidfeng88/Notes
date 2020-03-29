# Load Balancer

[https://opensource.com/article/18/10/internet-scale-load-balancing](https://opensource.com/article/18/10/internet-scale-load-balancing)

## Load Balancing - High Availability

### No load balancing

One server, one IP.

### DNS \(Domain Name System\) load balancing

Two servers, two IPs. DNS is cached, what if one server goes down?

### Network load balancing

One data center: one load balancer + many backend servers.

Layer 4 \(TCP/IP\) load balancer: can be hardware or software. DNS points one virtual IP, and it points to different backend servers \(based on a hash of source/destination IP address/port and protocal \(TCP/UDP\)\). Does not maintain state per connection.

L4 balancers can do health-checking, weighted balancing \(for backends with different capacities\).

### Going multi-site

More data center: The edge routers in both data centers advertise the same VIP. Request sent to that VIP can reach either site \(Anycast\). Most of the time it works. If one site fails, it stops advertising the VIP, and traffic goes to the other site.

Problems: can't control where traffic flows or limit how much traffic is sent to a given site. Can't explicitly route users to the nearest site \(but usually users end up with nearest site\).

### Controlling inbound requests in a multi-site system

Use different VIPs for each site and use geo-aware DNS to balance them with simple or weighted round-robin. New problems: DNS, cache records. Cannot redirect traffic quickly.

### Layer 7 load balancing

Aware of the structure and contents of requests. L7 load balancer can do: caching, rate limiting, fault injection, cost-aware load balancing \(some requests require much more server time to process\). They can also balance based on a request's attributes \(e.g. HTTP cookies\), terminate SSL connections, and help defend against application layger DOS attacks. Problems: cost. Usually L4 balancers are running in front of one or more pools of L7 balancers.


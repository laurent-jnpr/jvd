# <h2> CSDS = Connected Security Distributed Services
# <h3> SCALEOUT = the technical architecture used by CSDS


# Public documents: 
- CSDS Experience First Page: https://www.juniper.net/documentation/product/us/en/connected-security-distributed-services/
- CSDS Release Notes: https://www.juniper.net/documentation/us/en/software/connected-security-distributed-services/csds-release-notes/index.html
- CSDS Deployment Guide: https://www.juniper.net/documentation/us/en/software/connected-security-distributed-services/csds-deploy/index.html
- RE-Based Health Check CLI - https://www.juniper.net/documentation/us/en/software/junos/cli-reference/topics/ref/statement/routing-engine-mode-edit-services-traffic-load-balance.html


# Solution architectures are explained in multiple Juniper Validated Design documents (JVD):

[Public JVDs](https://www.juniper.net/documentation/validated-designs/)
- [Service Provider]( https://www.juniper.net/documentation/validated-designs/us/en/service-provider-edge/)
- [Security]( https://www.juniper.net/documentation/validated-designs/us/en/security/)
- [Datacenter](https://www.juniper.net/documentation/validated-designs/us/en/data-center/)


# ScaleOut CSDS specific JVDs cover 4 scenarios:
Common general view for all CSDS ScaleOut scenarios: 
![CSDS topology](pics/CSDS-general.png)
MX are stateless load balancers for SRX statefull security services

## Service Provider JVDs:
![ScaleOut SP SFW CGNAT topology](./pics/ScaleOut-SP-SFW-CGNAT-general.png)
- [Juniper Scale-Out Stateful Firewall and CGNAT for SP Edge](https://www.juniper.net/documentation/us/en/software/jvd/jvd-offbox-cgnat-01-01-sp/index.html)
- [Juniper Scale-Out IPsec for Mobile Service Providers](https://www.juniper.net/documentation/us/en/software/jvd/jvd-scale-out-IPsec-solution-for-mobile-service-providers/index.html)

## Enterprise JVDs:
![ScaleOut ENT SFW SNAT topology](./pics/ScaleOut-ENT-SFW-SNAT-general.png)
- [Juniper Scale-Out Stateful Firewall and Source NAT for Enterprise](https://www.juniper.net/documentation/us/en/software/jvd/jvd-mse-cgnat-offbox-ent-01-01/index.html)
- [Juniper Scale-Out IPsec Solution for Enterprises](https://www.juniper.net/documentation/us/en/software/jvd/jvd-scale-out-ipsec-solution-for-enterprises/index.html)


## Common technical architectures:
Those JVDs are all based on the same technical architectures that has itself 4 technical options:
![CSDS ScaleOut architectures](./pics/ScaleOut-COMMON-architectures.png)
- Single MX with multiple standalone SRX/vSRX
- Dual MX (using SRD) with multiple pairs of SRX/vSRX (MNHA)
- Single MX with multiple pairs of SRX/vSRX (MNHA)
- Dual MX with multiple pairs of SRX/vSRX (MNHA)
Why 4 possible architectures? As some may want very lightweight option with small platforms (takes less place and no need for redundancy), some others may want to add redundancy (MNHA and/or SRD), and some want all of them (belt and suspenders).


## Traffic Load Balancing
Since 2 possible methods are used, but with again some common parts, we are exposing here the main elements:
- ECMP CHASH = it load balances the SRX by maintaining the next hops (each SRX) thanks to routing protocol (BGP) and fast error detection (BFD)
- TLB / TO = it load balances the SRX usinc ECMP CHASH but by adding some additional form of grouping (by service) and healt checking (to maintain the service)


# Configurations:
The exposed configurations shows the architecture using TLB as this is the most developped one, but other config examples are shown inside each JVD for ECMP CHASH and TLB.
This chapter exposes the configurations for:
- [CGNAT_AND_SFW using TLB](./CGNAT_AND_SFW/) Contains configuration for TLB of SFW and CGNAT on IPv4 and IPv6
- [IPSEC using TLB](./IPSEC/) Contains configuration for TLB of IPSEC on IPv4


# Acronysms:
- CSDS Traffic Orchestrator (CSDS-TO), formerly known as Traffic Load Balancer (TLB) but often still referenced to as TLB.
- TLB = Traffic Load Balancer -> become CSDS Traffic Orchestrator (TO)
- ECMP = Equal Cost Multi Path
- CHASH = ECMP Consistent Hashing mechanism
- SRD = Service Reundancy Daemon (communications and redundancy mechanism between 2 MX's RE)
- MNHA = Multi Node High Availability for SRX firewalls (used with 2 nodes, either Active/Backup or Active/Active)
- CGNAT = Carrier Grade NAT (Network Address Translation for very large pools using mechanisms such as Poirt Block Allocation)
- SFW = Stateful FireWall (regular way of using a security device by allowing specific traffics and maintaining session states)
- IPSEC = IP Security with IKE authentication (IKEv1 and IKEv2, with Presehared Keys or Certificates) and IPsec ESP encrypted encapsulation

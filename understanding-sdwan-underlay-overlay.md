# Understanding SD-WAN underlay and overlay networks

**Documentation type:** Explanation (Diátaxis)\
**Audience:** Network engineers and technical support professionals

## Overview

Software-defined wide area networking (SD-WAN) separates the logical connectivity between sites from the transport networks that carry their traffic. Understanding this separation helps engineers explain how SD-WAN selects paths, maintains connectivity and responds to network degradation.

## The underlay: physical and IP transport

The underlay provides IP connectivity between SD-WAN endpoints. It may consist of broadband internet, dedicated internet access (DIA), MPLS or cellular connections such as 4G and 5G.

Each transport has its own characteristics, including bandwidth, latency, jitter, packet loss and availability. The underlay must provide the reachability required by the overlay; an operational local interface alone does not guarantee end-to-end connectivity.

## The overlay: logical connectivity

The overlay is a logical network built on top of the underlay. SD-WAN endpoints establish tunnels or other logical paths across available transports. Depending on the implementation, these paths may use encryption, centralised policy and dynamic path selection.

The overlay allows an organisation to apply consistent connectivity and traffic policies across sites without requiring every site to use the same transport provider or technology.

## How the layers work together

``` mermaid
flowchart TB
    A[Branch applications] --> B[Branch SD-WAN edge]
    B --> C[Logical overlay paths]
    C --> D[Internet underlay]
    C --> E[MPLS underlay]
    D --> F[Remote SD-WAN endpoint]
    E --> F
    F --> G[Remote applications]
```

The diagram illustrates two possible transport paths between endpoints. Overlay traffic is carried by the underlay; the overlay is not a separate physical connection.

  ----------------------------------------------------------------------------------------------------------------------------------------------------
  Characteristic              Underlay                                            Overlay
  --------------------------- --------------------------------------------------- --------------------------------------------------------------------
  Primary purpose             Transport packets between endpoints                 Provide logical connectivity and policy-based forwarding

  Examples                    DIA, MPLS, broadband, cellular                      SD-WAN tunnels and logical paths

  Typical health indicators   Interface status, IP reachability, transport loss   Tunnel state, path quality, application reachability

  Common failure causes       Circuit outage, routing failure, congestion         Tunnel negotiation, policy, authentication or configuration issues
  ----------------------------------------------------------------------------------------------------------------------------------------------------

## Path selection and application experience

SD-WAN can measure the quality of available paths and use configured policies to determine how application traffic is forwarded. The exact measurement methods, thresholds and failover behaviour depend on the platform and configuration.

Consider a branch with MPLS and DIA connections. Voice traffic normally uses MPLS, but the MPLS path begins experiencing excessive jitter and packet loss. If the configured policy permits it and the DIA path meets the required quality criteria, SD-WAN may steer voice traffic to DIA.

This illustrates two different failure conditions:

-   **Blackout:** A path becomes unavailable, for example because a circuit fails.
-   **Brownout:** A path remains reachable but its performance degrades enough to affect applications.

A brownout can be particularly difficult to recognise because an interface or tunnel may still appear operational. Path-quality measurements provide additional evidence of the user experience.

## Why the distinction matters in troubleshooting

An underlay problem can cause overlay tunnels to fail or degrade. However, a healthy underlay does not guarantee a healthy overlay. For example, an endpoint may have IP reachability while tunnel establishment fails because of authentication or configuration problems.

  --------------------------------------------------------------------------------------------------------------------
  Observation                                              Possible interpretation
  -------------------------------------------------------- -----------------------------------------------------------
  WAN interface is down                                    Local physical or access-circuit issue

  WAN interface is up but remote endpoint is unreachable   Underlay routing, provider or filtering issue

  Endpoint is reachable but tunnel is down                 Overlay negotiation, authentication or filtering issue

  Tunnel is up but an application performs poorly          Path degradation, policy, congestion or application issue
  --------------------------------------------------------------------------------------------------------------------

These observations are starting points for investigation, not definitive diagnoses. Engineers should correlate transport metrics, tunnel status, routing information and application behaviour before identifying a root cause.

## Redundancy and its limitations

Multiple underlay connections can improve resilience, but two circuits do not necessarily provide independent failure domains. They may share a provider, physical cable route, power supply or upstream infrastructure.

Similarly, overlay failover depends on path availability, policy and the capabilities of the SD-WAN endpoints. Redundant circuits do not guarantee uninterrupted application sessions or protect against every device failure.

## Summary

The underlay provides transport connectivity, while the overlay provides logical connectivity and policy-driven forwarding. Understanding both layers helps engineers distinguish transport failures from overlay problems and interpret application performance more accurately.

## Further reading

-   [RFC 7364: Problem Statement: Overlays for Network Virtualization](https://www.rfc-editor.org/rfc/rfc7364) --- background on network overlays.
-   [Diátaxis: Explanation](https://diataxis.fr/explanation/) --- documentation approach used for this sample.

------------------------------------------------------------------------

*Portfolio sample: vendor-neutral educational documentation. The scenario is illustrative and does not describe a specific customer deployment. AI-assisted draft for author review and adaptation.*

---
title: "A YANG Network Data Model for Layer 2 VPNs"
abbrev: "ietf-l2vpn-ntw-bis"
category: info

docname: draft-bg-onsen-l2sm-l2nm-bis-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Operationalizing Network workgroup: WG Working Groupamp; SErvice abstractioNs"
keyword:
 - VPNs
 - Network Connectivity
 - Service Provision
venue:
  group: "Operationalizing Network   group: ONSENamp; SErvice abstractioNs"
  type: "Working Group"
  mail: "onions@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/onions/"
  github: "sbarguil/LxNM-Updates"
  latest: "https://sbarguil.github.io/LxNM-Updates/draft-bg-onsen-l3sm-l3nm-bis.html"

author:
 -
    fullname: Oscar Gonzalez de Dios
    organization: Telefonica
    email: oscar.gonzalezdedios@telefonica.com
 -
    fullname: Samier Barguil
    organization: Nokia
    email: samier.barguil_giraldo@nokia.com

normative:

informative:


--- abstract

TODO Abstract


--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Operational Considerations

TBC per {{?I-D.opsarea-rfc5706bis}}.

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.

# Overall Structure of the Enhanced L2NM Module

## Explicit RD Assignment at the VPN Node Level

   In the L2NM {{?RFC9291}}, a Route Distinguisher (RD) can be
   provided in two places:

   * In a global parameters profile ('global-parameters-profile')
     defined at the VPN service level. A VPN node activates one or
     more of these profiles through
     'active-global-parameters-profiles', but cannot override the RD
     of the profile.
   * Under the 'bgp-auto-discovery' container of a VPN node. This
     container only applies when BGP auto-discovery is enabled for
     the service ('bgp-ad-enabled' set to 'true').

   As a consequence, when BGP auto-discovery is not used, the only way
   to assign a different RD per VPN node is to define one global
   parameters profile per node. Operators have requested the ability
   to set an explicit RD directly at the node level, overriding the
   value of the profile (GitHub issue #9). This allows a common
   profile to be used across all services for use cases where only
   the RD changes per node.

   This revision adds the 'route-distinguisher' grouping of the
   'ietf-vpn-common' module {{!RFC9181}} to the list of active global
   parameters profiles of a VPN node, next to the other parameters
   that a VPN node can already override (e.g.,
   'local-autonomous-system' or 'svc-mtu'):

~~~
  +--rw vpn-node* [vpn-node-id]
     ...
     +--rw active-global-parameters-profiles
     |  +--rw global-parameters-profile* [profile-id]
     |     +--rw profile-id                  leafref
     |     +--rw (rd-choice)?
     |     |  +--:(directly-assigned)
     |     |  |  +--rw rd?
     |     |  |          rt-types:route-distinguisher
     |     |  +--:(directly-assigned-suffix)
     |     |  |  +--rw rd-suffix?            uint16
     |     |  +--:(auto-assigned)
     |     |  |  ...
     |     |  +--:(auto-assigned-suffix)
     |     |  |  ...
     |     |  +--:(no-rd)
     |     |     +--rw no-rd?                empty
     |     +--rw local-autonomous-system?
     |     |       inet:as-number
     |     ...
~~~

   The RD is resolved for a VPN node as follows:

   1. If BGP auto-discovery is enabled and an RD is configured under
      'bgp-auto-discovery' of the VPN node, that RD is used.
   2. Otherwise, if an RD is configured for the active global
      parameters profile of the VPN node, that RD is used.
   3. Otherwise, the RD of the global parameters profile defined at
      the VPN service level is used.

   <check here -- confirm the precedence between the RD under
   'bgp-auto-discovery' and the new node-level override. The
   'bgp-auto-discovery' RD is given precedence here because it is the
   more specific configuration when BGP auto-discovery is used.>

   For example, the following JSON fragment defines a common profile
   at the service level and overrides only the RD on each VPN node:

~~~ json
"global-parameters-profiles": {
  "global-parameters-profile": [
    {
      "profile-id": "common-profile",
      "rd-suffix": 1,
      "svc-mtu": 1514
    }
  ]
},
"vpn-nodes": {
  "vpn-node": [
    {
      "vpn-node-id": "pe1",
      "ne-id": "pe1.example.com",
      "active-global-parameters-profiles": {
        "global-parameters-profile": [
          {
            "profile-id": "common-profile",
            "rd": "0:65000:101"
          }
        ]
      }
    },
    {
      "vpn-node-id": "pe2",
      "ne-id": "pe2.example.com",
      "active-global-parameters-profiles": {
        "global-parameters-profile": [
          {
            "profile-id": "common-profile",
            "rd": "0:65000:102"
          }
        ]
      }
    }
  ]
}
~~~

   This change is backward compatible: the new nodes are optional,
   and existing clients that do not populate them continue to obtain
   the RD from the global parameters profile, as per {{?RFC9291}}.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.

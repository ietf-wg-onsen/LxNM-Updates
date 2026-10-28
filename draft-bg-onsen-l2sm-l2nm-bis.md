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

# Changes Inherited from the VPN Common Module

## Multiple Routing Policies

   The L2NM {{?RFC9291}} uses the `vpn-route-targets` grouping of the
   `ietf-vpn-common` module {{!RFC9181}} to associate import and
   export routing policies with a VPN service and a VPN node. The
   revision of `ietf-vpn-common` changes `import-policy` and
   `export-policy` from a single `leaf` to a user-ordered
   `leaf-list` (GitHub issue #23), so that multiple routing policies
   can be applied in the order listed.

   The L2NM inherits this change without modifying the
   `ietf-l2vpn-ntw` module. As with the L3NM, the change is not
   backward compatible in the strict YANG sense ({{?RFC7950}},
   Section 11): clients MUST encode these nodes as a list, even when
   only one policy is configured.

# Operational Considerations

TBC per {{?I-D.opsarea-rfc5706bis}}.

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.

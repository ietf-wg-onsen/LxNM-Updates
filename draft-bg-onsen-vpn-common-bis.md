---
title: "A Common YANG Data Model for Layer 2 and Layer 3 VPNs"
abbrev: "ietf-vpn-common-bis"
category: info

docname: draft-bg-onsen-vpn-common-bis-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Operationalizing Network workgroup: WG Working Groupamp; SErvice abstractioNs"
keyword:
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

   Several years have passed since the publication of the LxNM YANG data
   models, which has since been widely adopted in the industry.
   Implementations in network controllers have revealed gaps in {
   {!RFC9181}}. This document updates the common YANG data models for
   L2 and L3 VPN provision to address those gaps.

--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Changes to the VPN Common Module

## Multiple Routing Policies

   The `vpn-route-targets` grouping defined in {{!RFC9181}} includes
   a `vpn-policies` container with two leaves, `import-policy` and
   `export-policy`. Each leaf is defined as a single `string`, which
   restricts the operator to referencing exactly one routing policy
   per direction (GitHub issue #23).

   Network devices commonly support an ordered list of import and
   export routing policies per VRF, evaluated in the order in which
   they are listed. This revision changes `import-policy` and
   `export-policy` from `leaf` to `leaf-list`, with `ordered-by user`
   so that the order provided by the client is the order in which
   the policies are applied.

   Both the L3NM {{?RFC9182}} and the L2NM {{?RFC9291}} use the
   `vpn-route-targets` grouping and inherit this change. This change
   is not backward compatible in the strict YANG sense ({{?RFC7950}},
   Section 11), as the cardinality of the nodes changes from a single
   value to a list. Existing single-value configurations can be
   expressed as a one-element list without semantic loss.

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

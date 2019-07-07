# draft-6man-pmo
IPv6 RA PathMTU Option
```




Internet Engineering Task Force                                 E. Kline
Internet-Draft                                                  Loon LLC
Intended status: Standards Track                             May 7, 2019
Expires: November 8, 2019


                          IPv6 PathMTU Option
                          draft-kline-6man-pmo

Abstract

Status of This Memo

   This Internet-Draft is submitted in full conformance with the
   provisions of BCP 78 and BCP 79.

   Internet-Drafts are working documents of the Internet Engineering
   Task Force (IETF).  Note that other groups may also distribute
   working documents as Internet-Drafts.  The list of current Internet-
   Drafts is at http://datatracker.ietf.org/drafts/current/.

   Internet-Drafts are draft documents valid for a maximum of six months
   and may be updated, replaced, or obsoleted by other documents at any
   time.  It is inappropriate to use Internet-Drafts as reference
   material or to cite them other than as "work in progress."

   This Internet-Draft will expire on November 8, 2019.

Copyright Notice

   Copyright (c) 2019 IETF Trust and the persons identified as the
   document authors.  All rights reserved.

   This document is subject to BCP 78 and the IETF Trust's Legal
   Provisions Relating to IETF Documents
   (http://trustee.ietf.org/license-info) in effect on the date of
   publication of this document.  Please review these documents
   carefully, as they describe your rights and restrictions with respect
   to this document.  Code Components extracted from this document must
   include Simplified BSD License text as described in Section 4.e of
   the Trust Legal Provisions and are provided without warranty as
   described in the Simplified BSD License.

1.  notes

   [RFC2119] IPv6 Router Advertisement Path MTU Option TLV Type = TBD
   Length = units of 8 octets, including TL bytes, with zero padding at
   the end as required { 4 byte MTU, 1 byte # of prefixes, [ { 1 byte of



Kline                   Expires November 8, 2019                [Page 1]

Internet-Draft             IPv6 PathMTU Option                  May 2019


   prefixlen, 0-16 bytes of prefix address, trailing zeros dropped },
   ...  ] } { 4 byte MTU, 1 byte # of prefixes, [ { 1 byte of prefixlen,
   0-16 bytes of prefix address, trailing zeros dropped }, ...  ] } MTUs
   less than IPv6 min MTU must be ignored and treated as 1280.  Regular
   MTU option applies to on-link destinations.  Examples: [1] RA MTU
   1500 RA PathMTU - 1422, 1, 0 1500 on-link (fe80::/10, ff02::/16, and
   2001:db8::/64), and 1422 for all other destinations.  [2] RA MTU 9000
   PIO 2001:db8:1:2::/64 RA PathMTU - 1500, 1, 0, - 9000, 1, 48,
   2001:db8:1::: 9k to 2001:db8:1::/48, fe80::/10, and ff02::/16 1500 to
   ::/0 [RFC4861]

2.  Normative References

   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119,
              DOI 10.17487/RFC2119, March 1997, <https://www.rfc-
              editor.org/info/rfc2119>.

   [RFC4861]  Narten, T., Nordmark, E., Simpson, W., and H. Soliman,
              "Neighbor Discovery for IP version 6 (IPv6)", RFC 4861,
              DOI 10.17487/RFC4861, September 2007, <https://www.rfc-
              editor.org/info/rfc4861>.

Author's Address

   Erik Kline
   Loon LLC
   1600 Amphitheatre Parkway
   Mountain View, California  94043
   US

   Email: ek@loon.com



















Kline                   Expires November 8, 2019                [Page 2]
```

# draft-6man-pmo
IPv6 RA PathMTU Option
```




Internet Engineering Task Force                                 E. Kline
Internet-Draft                                                  Loon LLC
Intended status: Standards Track                            July 7, 2019
Expires: January 8, 2020


                          IPv6 PathMTU Option
                        draft-kline-6man-pmo-00

Abstract

   This document describes an IPv6 Neighbor Discovery (ND) PathMTU
   Option (PMO) for inclusion in Router Advertisements (RAs).  This
   allows some environments greater flexibility to support, for example,
   a higher MTU for on-link or intra-administrative-domain
   communications than for broader Internet communications.

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

   This Internet-Draft will expire on January 8, 2020.

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



Kline                    Expires January 8, 2020                [Page 1]

Internet-Draft             IPv6 PathMTU Option                 July 2019


1.  Introduction

   This document describes an IPv6 Neighbor Discovery (ND) PathMTU
   Option (PMO) for inclusion in Router Advertisements (RAs).  This
   allows some environments greater flexibility to support, for example,
   a higher MTU for on-link or intra-administrative-domain
   communications than for broader Internet communications.

2.  Terminology

2.1.  Requirements Language

   The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
   "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and
   "OPTIONAL" in this document are to be interpreted as described in BCP
   14 [RFC2119] [RFC8174] when, and only when, they appear in all
   capitals, as shown here.

2.2.  Summary of key terms

   Path MTU Path MTU Discovery MTU Option [RFC4861] Path MTU Option
   (this document)

3.  Option Format

   XXX Any IPv6 link-local prefixes listed within a PathMTU Option MUST
   be ignored and SHOULD be logged for review by an administrator.

4.  Receiver Processing Rules

   Nodes compliant with this specification do not change their
   processing of RAs that contain no PathMTU Options.  Whether the RA
   contains an MTU Option or not, the processing is unaltered by this
   document.  Additionaly, for all Path MTU determination an effective
   Path MTU learned via a Path MTU Discovery mechanism ([RFC8201]) MUST
   take precedence.

   Any MTU value lower than the IPv6 minimum MTU (i.e. 1280, [RFC8200]
   section 5 [1]), SHOULD be logged for an administrator as a
   configuration error and MUST be treated as though the IPv6 minimum
   MTU had been the encoded value.

   Let the "Link MTU" be the MTU as it would be determined were no
   PathMTU Option present.  This may be: the value specified in an MTU
   Option, a value specified in a separate document that covers
   operating IP over a particular link type (e.g., [RFC2464]), or
   derived by other means (e.g.  administrative or a hint from a sub-IP




Kline                    Expires January 8, 2020                [Page 2]

Internet-Draft             IPv6 PathMTU Option                 July 2019


   layer mechanism).  The Link MTU MUST be used for all link-local
   destinations.

   The Link MTU SHOULD also be used for all on-link destinations, to
   maintain compatibility with existing behavior and expectations.  For
   the same reason, the Link MTU SHOULD be used for destinations within
   any PIO prefix in the RA, even if the L bit is not set.  As noted in
   [RFC5942], a destination may at some time be learned to be on-link,
   and this information may expire or be changed.

   For all other destinations, an initial Path MTU SHOULD be determined
   by first looking for a prefix that includes the destination in a Path
   MTU Option and using the corresponding MTU value.  If no such prefix
   exists, the Link MTU SHOULD be assumed to be the default.

   Note that as a matter of convenience a Path MTU Option may contain an
   entry for ::/0 even when the router lifetime is zero.  This in no way
   indicates that the router will function as a default gateway.
   Rather, it may be used to apply a Path MTU to all prefixes listed in
   an RIO (for example).

5.  notes

   IPv6 Router Advertisement Path MTU Option TLV Type = TBD Length =
   units of 8 octets, including TL bytes, with zero padding at the end
   as required { 4 byte MTU, 1 byte # of prefixes, [ { 1 byte of
   prefixlen, 0-16 bytes of prefix address, trailing zeros dropped },
   ...  ] } { 4 byte MTU, 1 byte # of prefixes, [ { 1 byte of prefixlen,
   0-16 bytes of prefix address, trailing zeros dropped }, ...  ] } MTUs
   less than IPv6 min MTU must be ignored and treated as 1280.  Regular
   MTU option applies to on-link destinations.  In environments where
   on-link nodes do not communicate with one another, either by policy
   or by practice, setting the MTU option value to the path MTU value
   for all destinations through the advertising router is a practical
   measure.  In other words, since there is no on-link communication of
   note, supporting higher MTUs for on-link destinations is of no
   operational value, especially when not doing so can burden hosts with
   endless PathMTU discovery.  However, even in these environments there
   may be changes in Path MTU (e.g.  a change in upstream provider to
   one with a lower MTU).  It can still be operationally useful to
   separate the Path MTU from the on-link MTU.  Examples: [1] RA MTU
   1500 RA PathMTU - 1422, 1, 0 1500 on-link (fe80::/10, ff02::/16, and
   2001:db8::/64), and 1422 for all other destinations.  [2] RA MTU 9000
   PIO 2001:db8:1:2::/64 RA PathMTU - 1500, 1, 0, - 9000, 1, 48,
   2001:db8:1::: [3] RA MTU 1422 PIO 2001:db8:1:2::/64 RA PathMTU -
   9000, 2, 64, fe80::, 64, 2001:db8:1:2:: 9k to 2001:db8:1::/48,
   fe80::/10, and ff02::/16 1500 to ::/0




Kline                    Expires January 8, 2020                [Page 3]

Internet-Draft             IPv6 PathMTU Option                 July 2019


6.  Security Considerations

   TBD

7.  References

7.1.  Normative References

   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119,
              DOI 10.17487/RFC2119, March 1997, <https://www.rfc-
              editor.org/info/rfc2119>.

   [RFC4861]  Narten, T., Nordmark, E., Simpson, W., and H. Soliman,
              "Neighbor Discovery for IP version 6 (IPv6)", RFC 4861,
              DOI 10.17487/RFC4861, September 2007, <https://www.rfc-
              editor.org/info/rfc4861>.

   [RFC8174]  Leiba, B., "Ambiguity of Uppercase vs Lowercase in RFC
              2119 Key Words", BCP 14, RFC 8174, DOI 10.17487/RFC8174,
              May 2017, <https://www.rfc-editor.org/info/rfc8174>.

   [RFC8200]  Deering, S. and R. Hinden, "Internet Protocol, Version 6
              (IPv6) Specification", STD 86, RFC 8200,
              DOI 10.17487/RFC8200, July 2017, <https://www.rfc-
              editor.org/info/rfc8200>.

   [RFC8201]  McCann, J., Deering, S., Mogul, J., and R. Hinden, Ed.,
              "Path MTU Discovery for IP version 6", STD 87, RFC 8201,
              DOI 10.17487/RFC8201, July 2017, <https://www.rfc-
              editor.org/info/rfc8201>.

7.2.  Informative References

   [RFC2464]  Crawford, M., "Transmission of IPv6 Packets over Ethernet
              Networks", RFC 2464, DOI 10.17487/RFC2464, December 1998,
              <https://www.rfc-editor.org/info/rfc2464>.

   [RFC5942]  Singh, H., Beebee, W., and E. Nordmark, "IPv6 Subnet
              Model: The Relationship between Links and Subnet
              Prefixes", RFC 5942, DOI 10.17487/RFC5942, July 2010,
              <https://www.rfc-editor.org/info/rfc5942>.

7.3.  URIs

   [1] https://tools.ietf.org/html/rfc8200#section-5





Kline                    Expires January 8, 2020                [Page 4]

Internet-Draft             IPv6 PathMTU Option                 July 2019


Author's Address

   Erik Kline
   Loon LLC
   1600 Amphitheatre Parkway
   Mountain View, California  94043
   US

   Email: ek@loon.com










































Kline                    Expires January 8, 2020                [Page 5]
```

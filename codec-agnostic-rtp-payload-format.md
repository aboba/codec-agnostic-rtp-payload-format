--
docname: draft-codec-agnostic-rtp-payload-format-00
title: Codec agnostic RTP payload format for video
category: std
ipr: trust200902
area: ART
workgroup: AVTCORE
stand_alone: yes

pi: [toc, sortrefs, symrefs]

author:
-
  ins: S. Garcia Murillo
  name: Sergio Garcia Murillo
  org: CoSMo
  email: sergio.garcia.murillo@cosmosoftware.io

-
  ins: A. Gouaillard
  name: Alex Gouaillard
  org: CoSMo
  email: alex.gouaillard@cosmosoftware.io

normative:
  RFC2119:
  RFC3550:
  RFC3711:
  RFC4566:
  RFC8285:

informative:
  RFC6464:
  RFC6465:
  RFC6904:

--- abstract



--- middle

Introduction
============

The objective of this spec is to create a generic RTP packetization format that can be use with any video codec or encrypted context and that allows SFUs to perform layer selection without requiring access to the codec payload.

RTP packetization
=======================

The packetization will work on a complete frame in byte format (h264 annex b for example) or the encrypted transformation of it.

The RTP packetizer will split the byte contents of each frames in one or more chunk and create one RTP packet for each chunk as the RTP payload. It will insert a Dependency Descriptor header extension with start_of_frame equals to 1 if it is the first packet of the frame and a end_of_frame equals to 1 if it is the last pacekt of the frame. 

In case an SVC codec supporting spatial scalaibility, each of the spatial layer frames will be packetized independently and sent together in the same RTP frame. The spatial layers must be sent in ascending order, with the same RTP timestamp, and only the latest RTP paekt of the latest spatial layer frame will have the marker bit set to 1.

Layer selection
=======================

The information required to perform the layer selection on an SFU is included in the Dependency descriptor which works even if the payload is encryped.

SDP negotiation
=======================
Each video formats will require a different payload type so they can be negotiated by both peers.

fmtp codec includes the FOURCC of the codec.

no need to negotiate the standard packetization for each coded, they are independent

rtx and fec are negotiated normally

Q: should we define what are the negotiated parameters for each codec? should we reuse mpeg/dash ones?


m=video 9 UDP/TLS/RTP/SAVPF 98 100 99 101
a=rtpmap:98 generic/90000
a=fmtp:98 codec=vp09,profile-id=0
a=rtpmap:99 rtx/90000
a=fmtp:99 apt=98
a=rtpmap:100 generic/90000
a=fmtp:100 codec=vp09,profile-id=2
a=rtpmap:101 rtx/90000
a=fmtp:101 apt=100


Security Considerations
=======================

IANA Considerations
===================


--- back

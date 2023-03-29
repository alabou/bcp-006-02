# AMWA BCP-006-02: NMOS With H.264

> WORK IN PROGRESS ... DOCUMENT UNDER CONSTRUCTION

{:.no_toc}

- A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

_(c) AMWA 2023, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

![NMOS logo](images/NMOS-logo.png)

> ## Instructions
>
> Add your content as (GitHub Flavoured) Markdown documents.
>
> Put diagrams (ideally PNG with encapsulated draw.io source) in the `images/` sub-directory.
>
> Follow the [Style Guide](Style%20Guide.md).
>
> Make a bulleted list of documents in `README.md` in this directory.
>
> Set the repo name used to get the Lint and Render status in the top-level `README.md` (four changes needed).
>
> Set the value of `amwa_id` in `.render/_config.yml` to the AMWA-assigned ID.

## Introduction

H.264 is a video compression technology standardized in Rec. [ITU-T H.264][H.264] | ISO/IEC 14496-10.
A companion RTP payload format specification was developed through the IETF Payloads working group, IETF [RFC 6184][RFC-6184] for the transport of an H.264 bitstream over RTP. 

The BCP-006-02 specification includes support for H.264 bitstreams that are compliant with the clauses of the main document and annexes A, B, C, D and E of the Rec. [ITU-T H.264][H.264] specification. It excludes support for bitstreams that are compliant with other annexes of the specification.
> Annex F (deprecated), Annex G (scalable video coding), Annex H (multiview video coding), Annex I (multiview and depth video coding) and Annex J (multiview and depth video with enhanced non-base view coding) are not supported.

The Rec. [ITU-T H.222.0][H.222.0] | ISO/IEC 13818-1 specification and associated amendments describe the embedding of an H.264 stream in an MPEG2-TS transport stream. An RTP payload format specification for MPEG2-TS transport stream was developed through the IETF Payloads working group, IETF [RFC 2550][RFC-2550] for transport over RTP. Other normative documents describe the requirements for the streaming of an MPEG2-TS transport stream over other non-RTP transports.

The [Society of Media Professionals, Technologists and Engineers][SMPTE] developed Standard [ST 2110-22][ST-2110-22] of the ST 2110 suite of protocols, which cover the end-to-end application use of constant bit rate compression for video over managed IP networks.
> Note that the definition of constant bit rate of ST 2110-22 is very strict "The video compression or the packetization of the video compression shall produce a constant number of bytes per frame. The packetization shall produce a constant number of RTP packets per frame.". This definition of constant bit rate is hereafter described as strict-CBR, using the H.264 definition of constant bit rate for CBR.

The [Video Services Forum][VSF] developed Technical Recommendation [TR-10-11][TR-10-11] and [TR-10-??][TR-10-??] of the IPMX suite of protocols, which cover the end-to-end application use of constant and variable bit rate compression for video, using the SMPTE ST 2110 and IPMX suite of protocols.

TR-10-11 and TR-10-?? mandate the use of the AMWA [IS-04][IS-04] and [IS-05][IS-05] NMOS Specifications in IPMX compliant systems.

AMWA IS-04 and IS-05 have support for various transport protocols and can signal the media type `video/H264` as defined in RFC 6184.

## Use of Normative Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY",
and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119][RFC-2119].

## Definitions

The NMOS terms 'Controller', 'Node', 'Source', 'Flow', 'Sender', 'Receiver' are used as defined in the [NMOS Glossary](https://specs.amwa.tv/nmos/main/docs/Glossary.html).

The term 'strict-CBR' corresponds to the definition of constant bit-rate at section 4 "Video Compression and Packetization" of the ST 2110-22 standard.

The terms CBR and VBR are defined in the Rec. [ITU-T H.264][H.264] standard.

## H.264 IS-04 Sources, Flows and Senders
### Sources
### Flows
### Senders
#### RTP transport
##### SDP format-specific parameters
#### Other transports
#### Multiplexed Flows

## H.264 IS-04 Receivers
### RTP transport
### Other transports
### Multiplexed Flows

## H.264 IS-05 Senders and Receivers
### RTP transport
### Other transports
### Parameter Sets

## H.264 IS-11 Senders and Receivers
### RTP transport
### Other transports

## Controllers

[BCP-004-01]: https://specs.amwa.tv/bcp-004-01/ "AMWA BCP-004-01 NMOS Receiver Capabilities"
[H.264]: https://www.itu.int/rec/T-REC-H.264 "Advanced video coding for generic audiovisual services"
[H.222.0]: https://www.itu.int/rec/T-REC-H.222.0 "Generic coding of moving pictures and associated audio information: Systems"
[RFC-2119]: https://tools.ietf.org/html/rfc2119 "Key words for use in RFCs"
[RFC-6184]: https://tools.ietf.org/html/rfc6184 "RTP Payload Format for H.264 Video"
[IS-04]: https://specs.amwa.tv/is-04/ "AMWA IS-04 NMOS Discovery and Registration Specification"
[IS-05]: https://specs.amwa.tv/is-05/ "AMWA IS-05 NMOS Device Connection Management Specification"
[NMOS Parameter Registers]: https://specs.amwa.tv/nmos-parameter-registers/ "Common parameter values for AMWA NMOS Specifications"
[TR-10-11]: https://vsf.tv/download/technical_recommendations/VSF_TR-10-11_2023-??-??.pdf ""
[TR-10-??]: https://vsf.tv/download/technical_recommendations/VSF_TR-10-??_2023-??-??.pdf ""
[VSF]: https://vsf.tv/ "Video Services Forum"
[SMPTE]: https://www.smpte.org/ "Society of Media Professionals, Technologists and Engineers"
[ST-2110-22]: https://ieeexplore.ieee.org/document/9893780 "ST 2110-22:2022 - SMPTE Standard - Professional Media Over Managed IP Networks: Constant Bit-Rate Compressed Video"

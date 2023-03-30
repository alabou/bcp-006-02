# AMWA BCP-006-02: NMOS With H.264

> WORK IN PROGRESS ... DOCUMENT UNDER CONSTRUCTION

{:.no_toc}

- A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

_(c) AMWA 2023, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

## Introduction

H.264 is a video compression technology standardized in Rec. [ITU-T H.264][H.264] | ISO/IEC 14496-10.
A companion RTP payload format specification was developed through the IETF Payloads working group, IETF [RFC 6184][RFC-6184] for the transport of an H.264 bitstream over RTP. 

The BCP-006-02 specification includes support for H.264 bitstreams that are compliant with the clauses of the main document and annexes A, B, C, D and E of the Rec. [ITU-T H.264][H.264] specification. It excludes support for bitstreams that are compliant with other annexes of the specification.
> Annex F (deprecated), Annex G (scalable video coding), Annex H (multiview video coding), Annex I (multiview and depth video coding) and Annex J (multiview and depth video with enhanced non-base view coding) are not supported.

The Rec. [ITU-T H.222.0][H.222.0] | ISO/IEC 13818-1 specification and associated amendments describe the embedding of an H.264 stream in an MPEG2-TS transport stream. An RTP payload format specification for MPEG2-TS transport stream was developed through the IETF Payloads working group, IETF [RFC 2250][RFC-2250] for transport over RTP. Other normative documents describe the requirements for the streaming of an MPEG2-TS transport stream over other non-RTP transports.

The [Society of Media Professionals, Technologists and Engineers][SMPTE] developed Standard [ST 2110-22][ST-2110-22] of the ST 2110 suite of protocols, which cover the end-to-end application use of constant bit rate compression for video over managed IP networks.

> Note that the definition of constant bit rate of ST 2110-22 is very strict. "The video compression or the packetization of the video compression shall produce a constant number of bytes per frame. The packetization shall produce a constant number of RTP packets per frame." This definition of constant bit rate is hereafter described as strict-CBR, using the H.264 definition of constant bit rate for CBR.

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

Nodes capable of transmitting H.264 video streams MUST have Source, Flow and Sender resources in the IS-04 Node API.

### Sources

The Source resource MUST indicate `urn:x-nmos:format:video` for the `format` attribute. 
- Source resources MAY be associated with many Flows at the same time. The Source is therefore unaffected by the use of H.264 compression.

### Flows

The Flow resource MUST indicate `video/H264` in the `media_type` attribute, and `urn:x-nmos:format:video` for the `format` attribute. This has been permitted since IS-04 v1.1. 
- H.264 Flow resources MAY be associated with many Senders at the same time through the Senders' `flow_id` attributes. The H.264 Flow is therefore unaffected by the use of a specific transport. 
- H.264 Flow resources MAY be associated with many Flows at the same time through the Flows' `parents` attributes. The H.264 Flow is therefore unaffected by being the parent of some other Flows.

For Nodes implementing IS-04 v1.3 or higher, the following additional requirements on the Flow resource apply.

In addition to those attributes defined in IS-04 for all coded video Flows, the following attributes defined in the [Flow Attributes Register](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/) of the [NMOS Parameter Registers][] are used for H.264.

These attributes provide information for Controllers and Users to evaluate stream compatibility between Senders and Receivers.

- [Components](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#components)
  The Flow resource MUST indicate the color (sub-)sampling, width, height and depth of the associated uncompressed picture using the `components` attribute. The `components` array values MUST correspond to the stream's active parameter sets. A Flow MUST track the stream's current active parameter sets.

Informative note: ST 2110-22 does not require the `sampling` or `depth` SDP parameters. RFC 6184 does not define any such SDP parameters. The `sampling` and `depth` of the associated uncompressed picture could be derived from the H.264 active parameter sets by a Receiver.

- [Profile](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#profile)
  The Flow resource MUST indicate the H.264 profile, which defines algorithmic features and limits that MUST be supported by all decoders conforming to that profile. The stream's active parameter sets MUST be compliant with the `profile` attribute of the Flow. The permitted `profile` values are strings, defined as per ITU-T  Rec. H.264 Annex A

  - "BaselineConstrained"
  - "Baseline" (Default if not specified in the SDP transport file)
  - "Main"
  - "Extended"
  - "High"
  - "HighProgressive"
  - "HighConstrained"
  - "High10"
  - "High10Progressive"
  - "High-422"
  - "HighPredictive-444"
  - "High10Intra"
  - "HighIntra-422"
  - "HighIntra-444"
  - "CAVLCIntra-444"

Informative note: The names of the profiles in string form have been derived from the names used at Annex A of the H.264 standard with whitespace omitted, the sampling mode always positioned at the end of the string, preceded by a '-' and the terms High and Baseline positioned at the beginning of the string.

The profile strings in this specification are included in the [NMOS Parameter Registers][]. Additional strings may be added there in the future.

- [Level](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#level)
  The Flow resource MUST indicate the H.264 level, which defines a set of limits on the values that may be taken by the syntax elements of an H.264 bitstream. The stream's active parameter sets MUST be compliant with the `level` attribute of the Flow. The permitted `level` values are strings, defined as per ITU-T Rec. H.264 Annex A

  - "1" (Default if not specified in the SDP transport file)
  - "1b", "1.1", "1.2", "1.3"
  - "2", "2.1", "2.2"
  - "3", "3.1", "3.2"
  - "4", "4.1", "4.2"
  - "5", "5.1", "5.2"
  - "6", "6.1", "6.2"

Informative note: The names of the levels in string form have been derived from the names used at Annex A of the H.264 standard.

The level strings in this specification are included in the [NMOS Parameter Registers][]. Additional strings may be added there in the future.

The Flow's `profile` and `level` attributes map to the `profile-level-id` parameter of the SDP transport file. See section SDP format-specific parameters.

The Flow's `profile` and `level` attributes map to the members profile_idc, level_idc and constraint_set\<N\>_flag of the AVC_video_descriptor of an MPEG2-TS transport stream. See section Multiplexed Flows.

Informative note: The Flow's `profile` and `level` attributes are always required. The SDP transport file `profile-level-id` parameter MAY be omitted when matching the default value.

- [Bit Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#bit-rate)
  The Flow resource MUST indicate the target encoding bit rate (kilobits/second) of the H.264 bitstream. The stream's active parameter sets MUST be compliant with the `bit_rate` attribute of the Flow. The `bit_rate` integer value is expressed in units of 1000 bits per second, rounding up.

  Informative note: The H.264 bitstream is not required to transport hypothetical reference decoder (HRD) parameters such that an H.264 decoder may not know the actual target bit rate of a stream. There are bit rate limits imposed by the level of the coded bitstream. IS-11 may be used to constraint the Sender to a target bit rate compatible with the Receiver Capabilities.

- [Constant Bit Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#constant-bit-rate)
  The Flow resource MUST indicate if it operates in constant bit rate (CBR) mode or variable bit rate mode (VBR or other). When operating in constant bit rate mode the `bit_rate` corresponds to the constant encoding bit rate. Otherwise it corresponds to the maximum encoding bit rate. Since the default value of this attribute is `false`, a Flow MAY omit this attribute when using a variable bit rate mode.

Informative note: The maximum bit rate information relates to the codec profile / level limits and the HRD buffering model. The CBR versus VBR mode of operation of the encoder provide essential clues about the coded bitstream produced.

Informative note: For streams compliant with ST 2110-22, the constant bit rate mode is more appropriately described as a strict-CBR mode where "the video compression or the packetization of the video compression shall produce a constant number of bytes per frame. The packetization shall produce a constant number of RTP packets per frame." For other streams, not compliant with ST 2110-22, it is the constant bit rate definition of the H.264 specification that prevails.

Examples Flow resources are provided in [Examples](../examples/).

### Senders
#### RTP transport
##### SDP format-specific parameters

This section applies to a Sender with RTP transport directly associated to an H.264 Flow through the Sender's `flow_id` attribute.

Informative note: When an H.264 Flow is not directly associated with a Sender but with another Flow through the Flow's parents attribute, it does not have to provide format-specific attributes in the form of an SDP transport file. A Sender has such requirement only for the Flow directly associated with it.

The SDP file at the `manifest_href` MUST comply with the requirements of RFC 6184 in the [Usage in Declarative Session Descriptions](https://datatracker.ietf.org/doc/html/rfc6184#section-8.2.3) mode of operation. The SDP Offer/Answer Model described in RFC 6184 is not supported. The `fmtp` source attribute as specified in Section 6.3 of [RFC 5576][RFC-5576] is not supported.

Additionally, the SDP transport file needs to convey, so far as the defined format-specific parameters allow, the same information about the stream as conveyed by the Source, Flow and Sender attributes defined by this specification and IS-04, unless such information is conveyed through in-band parameter sets.

Therefore:
- The `profile-level-id` format-specific parameters MUST be included with the correct value unless it corresponds to the default value. 

- The `packetization-mode` format-specific parameters MUST be included with the correct value unless it corresponds to the default value.

- The `sprop-interleaving-depth`, `sprop-deint-buf-req`, `sprop-init-buf-time` and `sprop-max-don-diff` format-specific parameters SHOULD be included with the correct value if the `packetization-mode` equals one of the interleaved modes unless they correspond to their default value.

- The `sprop-parameter-sets` MUST always be included if the Sender `parameter_sets_transport_mode` property is `out_of_band` and SHOULD be included if the property is `in_and_out_of_band`.

The stream's active parameter sets MUST be compliant with the format-specific parameters, except `sprop-parameter-sets`, declared in the "fmtp=" attribute of an SDP transport file.

If the Sender meets the traffic shaping and delivery timing requirements specified for ST 2110-22, the SDP transport file MUST also comply with the provisions of ST 2110-22.

If the Sender meets the traffic shaping and delivery timing requirements specified for IPMX, the SDP transport file MUST also comply with the provisions of IPMX.

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

[H.264]: https://www.itu.int/rec/T-REC-H.264 "Advanced video coding for generic audiovisual services"
[H.222.0]: https://www.itu.int/rec/T-REC-H.222.0 "Generic coding of moving pictures and associated audio information: Systems"
[RFC-2119]: https://tools.ietf.org/html/rfc2119 "Key words for use in RFCs"
[RFC-6184]: https://tools.ietf.org/html/rfc6184 "RTP Payload Format for H.264 Video"
[RFC-2250]: https://tools.ietf.org/html/rfc2250 "RTP Payload Format for MPEG1/MPEG2 Video"
[RFC-5576]: https://tools.ietf.org/html/rfc5576 "Source-Specific Media Attributes in the Session Description Protocol (SDP)"
[IS-04]: https://specs.amwa.tv/is-04/ "AMWA IS-04 NMOS Discovery and Registration Specification"
[IS-05]: https://specs.amwa.tv/is-05/ "AMWA IS-05 NMOS Device Connection Management Specification"
[NMOS Parameter Registers]: https://specs.amwa.tv/nmos-parameter-registers/ "Common parameter values for AMWA NMOS Specifications"
[TR-10-11]: https://vsf.tv/download/technical_recommendations/VSF_TR-10-11_2023-??-??.pdf "ADD TEXT"
[TR-10-??]: https://vsf.tv/download/technical_recommendations/VSF_TR-10-??_2023-??-??.pdf "ADD TEXT"
[VSF]: https://vsf.tv/ "Video Services Forum"
[SMPTE]: https://www.smpte.org/ "Society of Media Professionals, Technologists and Engineers"
[ST-2110-22]: https://ieeexplore.ieee.org/document/9893780 "ST 2110-22:2022 - SMPTE Standard - Professional Media Over Managed IP Networks: Constant Bit-Rate Compressed Video"

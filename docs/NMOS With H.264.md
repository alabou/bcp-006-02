# AMWA BCP-006-02: NMOS With H.264
{:.no_toc}

> WORK IN PROGRESS ... DOCUMENT UNDER CONSTRUCTION

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

- [Profile](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#profile)

  The Flow resource MUST indicate the H.264 profile, which defines algorithmic features and limits that MUST be supported by all decoders conforming to that profile. The stream's active parameter sets MUST be compliant with the `profile` attribute of the Flow. The permitted `profile` values are strings, defined as per Rec. ITU-T H.264 Annex A

  - "BaselineConstrained"
  - "Baseline"
  - "Main"
  - "Extended"
  - "High", "High-422"
  - "HighProgressive"
  - "HighConstrained"
  - "High10"
  - "High10Progressive"
  - "HighPredictive-444"
  - "High10Intra"
  - "HighIntra-422", "HighIntra-444"
  - "CAVLCIntra-444"

  Informative note: The names of the profiles in string form have been derived from the names used at Annex A of the H.264 standard with whitespace omitted, the sampling mode always positioned at the end of the string, preceded by a '-' and the terms High and Baseline positioned at the beginning of the string.

  The profile strings in this specification are included in the [NMOS Parameter Registers][]. Additional strings may be added there in the future.

- [Level](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#level)
  
  The Flow resource MUST indicate the H.264 level, which defines a set of limits on the values that may be taken by the syntax elements of an H.264 bitstream. The stream's active parameter sets MUST be compliant with the `level` attribute of the Flow. The permitted `level` values are strings, defined as per Rec. ITU-T H.264 Annex A

  - "1"
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

Informative note: The Flow's `profile` and `level` attributes are always required. The SDP transport file `profile-level-id` parameter may be omitted when matching the default value.

- [Bit Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#bit-rate)
  
  The Flow resource MUST indicate the target encoding bit rate (kilobits/second) of the H.264 bitstream. The stream's active parameter sets MUST be compliant with the `bit_rate` attribute of the Flow. The `bit_rate` integer value is expressed in units of 1000 bits per second, rounding up.

  Informative note: The H.264 bitstream is not required to transport hypothetical reference decoder (HRD) parameters such that an H.264 decoder may not know the actual target bit rate of a stream. There are bit rate limits imposed by the level of the coded bitstream. IS-11 may be used to constraint the Sender to a target bit rate compatible with the Receiver Capabilities.

- [Constant Bit Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#constant-bit-rate)
  
  The Flow resource MUST indicate if it operates in constant bit rate (CBR) mode or variable bit rate mode (VBR or other). When operating in constant bit rate mode the `bit_rate` corresponds to the constant encoding bit rate. Otherwise it corresponds to the maximum encoding bit rate. Since the default value of this attribute is `false`, a Flow MAY omit this attribute when using a variable bit rate mode.

Informative note: The maximum bit rate information relates to the codec profile / level limits and the HRD buffering model. The CBR versus VBR mode of operation of the encoder provide essential clues about the coded bitstream produced.

Informative note: For streams compliant with ST 2110-22, the constant bit rate mode is more appropriately described as a strict-CBR mode where "the video compression or the packetization of the video compression shall produce a constant number of bytes per frame. The packetization shall produce a constant number of RTP packets per frame." For other streams, not compliant with ST 2110-22, it is the constant bit rate definition of the H.264 specification that prevails.

The H.264 encoder associated with the Flow MUST produces an H.264 bitstream that is compliant with the `profile` and `level` attributes.

Examples Flow resources are provided in [Examples](../examples/).

### Senders

This section applies to a Sender directly associated with an H.264 Flow through the Sender's `flow_id` attribute.

Informative note: When an H.264 Flow is not directly associated with a Sender but with a multiplexed Flow through the Flow's parents attribute, it does not have to provide Sender's format-specific attributes. A Sender has such requirement only when the H.264 Flow is directly associated with it. See the Sender's format specific (multiplexed) specification for details.

Sender resources provide no indication of media type or format, since this is described by the associated Flow resource.

#### RTP transport

For Nodes transmitting H.264 using the RTP payload mapping defined by RFC 6184, the Sender resource MUST indicate `urn:x-nmos:transport:rtp` or one of its subclassifications for the `transport` attribute.

For Nodes implementing IS-04 v1.3 or higher, the following additional requirements on the Sender resource apply.

In addition to those attributes defined in IS-04 for Senders, the following attributes defined in the [Sender Attributes Register](https://specs.amwa.tv/nmos-parameter-registers/branches/main/sender-attributes/) of the NMOS Parameter Registers are used for H.264.

- [Bit Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/sender-attributes/#bit-rate)
  
  The Sender resource SHOULD indicate the target bit rate (kilobits/second) including the transport overhead of the H.264 stream. The value is for the IP packets, so for the RTP payload format per RFC 6184, that includes the RTP, UDP and IP packet headers and the payload. The `bit_rate` integer value is expressed in units of 1000 bits per second, rounding up. The Sender's transport `bit_rate` value MUST derive from the Flow's encoding `bit_rate` value and according to the Flow's `constant_bit_rate` attribute it corresponds to a constant bit rate or a maximum bit rate. 

  If the Sender meets the traffic shaping and delivery timing requirements specified for ST 2110-22 it MUST indicate the transport `bit_rate`.

  Informative note: This definition is consistent with the definition of the bit rate attribute (b=) required by ST 2110-22 in the SDP media description. There is no such SDP attribute in RFC 6184.

- [Packet Transmission Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/sender-attributes/#packet-transmission-mode)
  
  A Sender using the non-interleaved or interleaved packetization modes MUST include the `packet_transmission_mode` attribute and set it to either `non_interleaved_nal_units` or `interleaved_nal_units`. The `packet_transmission_mode` attribute maps the the RFC 6184 `packetization-mode` parameter with `single_nal_unit` corresponding to value 0, `non_interleaved_nal_units` to value 1 and `interleaved_nal_units` to value 2. Since the default value of this attribute is `single_nal_unit`, the Sender MAY omit this attribute when using that mode. 

- [ST 2110-21 Sender Type](https://specs.amwa.tv/nmos-parameter-registers/branches/main/sender-attributes/#st-2110-21-sender-type)
  
  If the Sender complies with the traffic shaping and delivery timing requirements for ST 2110-22, it MUST include the `st2110_21_sender_type` attribute.

- [Parameter Sets Flow Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#parameter-sets-flow-mode)
  
  A Sender operating in strict-Flow mode MUST set the `parameter_sets_flow_mode` attribute to `strict` or if operating in static-Flow mode it MUST set the `parameter_sets_flow_mode` attribute to `static`. Otherwise it MAY omit or set the `parameter_sets_flow_mode` attribute to `dynamic`. If unspecified the default value is `dynamic`. See the "Parameter Sets" section for more details.

- [Parameter Sets Transport Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#parameter-sets-transport-mode)
  
  A Sender operating with out-of-band parameter sets MUST set the `parameter_sets_transport_mode` attribute to either `out_of_band` or `in_and_out_of_band`. Otherwise it MAY omit or set the `parameter_sets_transport_mode` attribute to `in_band`. If unspecified the default value is `in_band`. See the "Parameter Sets" section for more details.

  An example Sender resource is provided in the [Examples](../examples/).

##### SDP format-specific parameters

This section applies to a Sender with RTP transport directly associated with an H.264 Flow through the Sender's `flow_id` attribute.

Informative note: When an H.264 Flow is not directly associated with a Sender but with another Flow through the Flow's parents attribute, it does not have to provide format-specific attributes in the form of an SDP transport file. A Sender has such requirement only for the Flow directly associated with it.

The SDP file at the `manifest_href` MUST comply with the requirements of RFC 6184 in the [Usage in Declarative Session Descriptions](https://datatracker.ietf.org/doc/html/rfc6184#section-8.2.3) mode of operation. The SDP Offer/Answer Model described in RFC 6184 is not supported. The `fmtp` source attribute as specified in Section 6.3 of [RFC 5576][RFC-5576] is not supported.

Additionally, the SDP transport file needs to convey, so far as the defined format-specific parameters allow, the same information about the stream as conveyed by the Source, Flow and Sender attributes defined by this specification and IS-04, unless such information is conveyed through in-band parameter sets.

Therefore:
- The `profile-level-id` format-specific parameters MUST be included with the correct value unless it corresponds to the default value. "Baseline" is the default profile value and "1" is the default level value.

- The `packetization-mode` format-specific parameters MUST be included with the correct value unless it corresponds to the default value.

- The `sprop-interleaving-depth`, `sprop-deint-buf-req`, `sprop-init-buf-time` and `sprop-max-don-diff` format-specific parameters SHOULD be included with the correct value if the `packetization-mode` equals one of the interleaved modes unless they correspond to their default value.

- The `sprop-parameter-sets` MUST always be included if the Sender `parameter_sets_transport_mode` attribute is `out_of_band` and SHOULD be included if the attribute is `in_and_out_of_band`.

The stream's active parameter sets MUST be compliant with the format-specific parameters, except `sprop-parameter-sets`, declared in the "fmtp=" attribute of an SDP transport file.

If the Sender meets the traffic shaping and delivery timing requirements specified for ST 2110-22, the SDP transport file MUST also comply with the provisions of ST 2110-22.

Informative note: ST 2110-22 does not require the `sampling` or `depth` SDP parameters. RFC 6184 does not define any such SDP parameters. The `sampling` and `depth` of the associated uncompressed picture could be derived from the H.264 active parameter sets by a Receiver.

If the Sender meets the traffic shaping and delivery timing requirements specified for IPMX, the SDP transport file MUST also comply with the provisions of IPMX.

An example SDP file is provided in the [Examples](../examples/).

#### Other transports

For Nodes transmitting H.264 using other transports, the Sender resource MUST indicate the associated `urn:x-nmos:transport:` label of the transport or one of its subclassifications for the `transport` attribute.

Sender resources provide no indication of media type or format, since this is described by the associated Flow resource.

The `manifest_href` attribute MAY be `null` if an SDP transport file is not supported by the transport. Otherwise the SDP transport file MUST comply the transport specific requirements. There is no SDP format-specific parameters requirements for transports other than RTP.

- [Parameter Sets Flow Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#parameter-sets-flow-mode)
  
  A Sender operating in strict-Flow mode MUST set the `parameter_sets_flow_mode` attribute to `strict` or if operating in static-Flow mode it MUST set the `parameter_sets_flow_mode` attribute to `static`. Otherwise it MAY omit or set the `parameter_sets_flow_mode` attribute to `dynamic`. If unspecified the default value is `dynamic`. See the "Parameter Sets" section for more details.

- [Parameter Sets Transport Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#parameter-sets-transport-mode)
  
  A Sender operating with out-of-band parameter sets MUST set the `parameter_sets_transport_mode` attribute to either `out_of_band` or `in_and_out_of_band`. Otherwise it MAY omit or set the `parameter_sets_transport_mode` attribute to `in_band`. If unspecified the default value is `in_band`. See the "Parameter Sets" section for more details.

Informative note: The out of band mechanism used to transmit parameter sets is transport specific and out of the scope of this specification.

## H.264 IS-04 Receivers

Nodes capable of receiving H.264 video streams MUST have Receiver resources in the IS-04 Node API.

This section applies to a Receiver directly or indirectly associated with an H.264 stream.

Informative note: When an H.264 stream is directly associated with a Receiver, the Receiver has `format` set to `urn:x-nmos:format:video` and `media_types` of the `caps` attribute contains `video/H264`. When an H.264 stream is part of a multiplexed stream and is indirectly associated with a Receiver, the Receiver has `format` set to `urn:x-nmos:format:mux`, `media_types` of the `caps` attribute does not contains `video/H264` and `constraint_sets` of the `caps` attribute contains `video/H264`.

For a Receiver directly associated with an H.264 stream, the Receiver resource MUST indicate `urn:x-nmos:format:video` for the `format` attribute and MUST list `video/H264` in the `media_types` array within the `caps` object. This has been permitted since IS-04 v1.1.

For a Receiver indirectly associated with an H.264 stream part a multiplexed stream, the Receiver resource MUST indicate `urn:x-nmos:format:mux` for the `format` attribute, MUST list the mux media type in the `media_types` array within the `caps` object and MUST list a constraint set indicating support for the media type `video/H264` in the `constraint_sets` array within the `caps` object.

If the Receiver has limitations on or preferences regarding the H.264 video streams that it supports, the Receiver resource MUST indicate constraints in accordance with the [BCP-004-01][] Receiver Capabilities specification. The Receiver SHOULD express its constraints as precisely as possible, to allow a Controller to determine with a high level of confidence the Receiver's compatibility with the available streams. It is not always practical for the constraints to indicate every type of stream that a Receiver can or cannot consume successfully; however, they SHOULD describe as many of its commonly used operating points as practical and any preferences among them.

The `constraint_sets` parameter within the `caps` object MUST be used to describe combinations of frame rates, width and height, and other parameters which the receiver can support, using the parameter constraints defined in the [Capabilities register](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/) of the NMOS Parameter Registers.

When the H.264 decoder has no restrictions on the value of some parameter, the Receiver can indicate that the parameter is unconstrained, as described in BCP-004-01. 

The following parameter constraints can be used to express limits or preferences specifically defined by Rec. ITU-T H.264 for H.264 decoders:

- [Profile](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#profile)

  Some H.264 profiles are superset of other profiles. The H.264 specification describe the relationship among the profiles. From the point of view of the H.264 specification, supporting such superset profile is required to also be supporting the associated subset profiles. To assist a Controller not having knowledge of the H.264 profiles relationship, the Receiver Capabilities SHOULD enumerate subset profiles in addition to the superset profile.

- [Level](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#level)

  Some H.264 levels are superset of other levels. The H.264 specification describe the relationship among the levels. From the point of view of the H.264 specification, supporting such superset level is required to also be supporting the associated subset levels. To assist a Controller not having knowledge of the H.264 levels relationship, the Receiver Capabilities SHOULD enumerate all the subset levels in addition to the superset level.

- [Bit Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#format-bit-rate)
- [Constant Bit Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#format-constant-bit-rate)

A Receiver MUST be able to decode bitstreams conforming to the profiles and levels declared in the Receiver Capabilities. A Receiver MAY have preferences and more optimal profiles and levels that MAY be declared through Receiver Capabilities. A preferred constraint set MAY indicate such preferences while another constraint set MAY indicate full support of some profiles and levels. A Receiver MAY further constrain the support of a coded bitstream compliant with a profile and level using other constraints in its Receiver Capabilities.

Other existing parameter constraints, such as the following, are also appropriate to express limitations on supported H.264 video streams:

- [Frame Width](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#frame-width)
- [Frame Height](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#frame-height)
- [Grain Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#grain-rate)
- [Color Sampling](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#color-sampling)
- [Component Depth](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#component-depth)
- [Colorspace](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#colorspace)
- [Transfer Characteristic](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#transfer-characteristic)
- [ST 2110-21 Sender Type](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#st-2110-21-sender-type)

An example Receiver resource is provided in the [Examples](../examples/).

### RTP transport

For Nodes consuming H.264 using the RTP payload mapping defined by RFC 6184, the Receiver resource MUST indicate `urn:x-nmos:transport:rtp` or one of its subclassifications for the `transport` attribute.

The following parameter constraints can be used to express limits or preferences specifically defined for H.264 decoders:

- [Bit Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#transport-bit-rate)

- [Packet Transmission Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#packet-transmission-mode)

  A Receiver based on RTP transport declares the `packet_transmission_mode` capability to indicate the NAL units packetization modes that it supports. RFC 6184 require the `single_nal_unit` packetization mode to be supported by all the H.264 Receivers. As such a constrained Receiver MUST have a `packet_transmission_mode` capability enumerating the "single_nal_unit" value and optionally it MAY also enumerate the "non_interleaved_nal_units" and "interleaved_nal_units" values.

- [Parameter Sets Flow Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#parameter-sets-flow-mode)
  
  A Receiver declares the `parameter_sets_flow_mode` capability to indicate that it supports bitstreams using parameter sets associated with strictly one (`strict`), one (`static`) or multiple (`dynamic`) Flows. Considering that an active parameter set is associated with a specific Flow, this capability indicates that a Receiver is capable or not of decoding an H.264 bitstream where the associated Flow properties change dynamically. 
  
  A Receiver supporting the `dynamic` mode MUST also support the `strict` and `static` modes. Such Receiver SHOULD have `strict`, `static` and `dynamic` values enumerated in the Receiver Capability in order to allow Senders operating in any `parameter_sets_flow_mode`.

- [Parameter Sets Transport Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#parameter-sets-transport-mode)

  A Receiver declares the `parameter_sets_transport_mode` capability to indicate that it supports bitstreams using parameter sets provided either only out-of-band, only in-band or in-and-out-of-band. The in-band parameter sets MAY update, augment or duplicate the parameter sets received out-of-band. A Receiver declaring the `in_band` or `in_and_out_of_band` capabilities MUST be capable of decoding in-band parameter sets that update, augment or duplicate the parameter sets received out-of-band. A Receiver declaring the `out_of_band` capability MUST be capable of decoding in-band parameter sets that duplicate the parameter sets received out-of-band.

  All the parameter sets used by the bitstream MUST be compliant with the `profile-level-id` parameter explicitely or implicitely declared in the stream's associated SDP transport file. The parameter sets MAY be specified out-of-band using the `sprop-parameter-sets` parameter of an SDP transport file, in-band through the H.264 bitstream or in-and-out-of-band usign both mechanisms. See the "Parameter Sets" section for more details.

  A Receiver supporting `in_and_out_of_band` MUST also support the `in_band` and `out_of_band` modes. Such Receiver SHOULD have all "in_band", "out_of_band" and "in_and_out_of_band" values enumerated in the Receiver Capabilities in order to allow Senders operating in any `parameter_sets_transport_mode`.

For Nodes consuming H.264 using the RTP payload mapping defined by RFC 2250, the Receiver resource MUST indicate `urn:x-nmos:transport:rtp` or one of its subclassifications for the `transport` attribute.

The following parameter constraints can be used to express limits or preferences specifically defined for H.264 decoders:

- [Parameter Sets Flow Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#parameter-sets-flow-mode)
  
  A Receiver declares the `parameter_sets_flow_mode` capability to indicate that it supports bitstreams using parameter sets associated with strictly one (`strict`), one (`static`) or multiple (`dynamic`) Flows. Considering that an active parameter set is associated with a specific Flow, this capability indicates that a Receiver is capable or not of decoding an H.264 bitstream where the associated Flow properties change dynamically. 
  
  A Receiver supporting the `dynamic` mode MUST also support the `strict` and `static` modes. Such Receiver SHOULD have `strict`, `static` and `dynamic` values enumerated in the Receiver Capability in order to allow Senders operating in any `parameter_sets_flow_mode`.

- [Parameter Sets Transport Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#parameter-sets-transport-mode)

  A Receiver declares the `parameter_sets_transport_mode` capability to indicate that it supports bitstreams using parameter sets provided either only out-of-band, only in-band or in-and-out-of-band. The in-band parameter sets MAY update, augment or duplicate the parameter sets received out-of-band. A Receiver declaring the `in_band` or `in_and_out_of_band` capabilities MUST be capable of decoding in-band parameter sets that update, augment or duplicate the parameter sets received out-of-band. A Receiver declaring the `out_of_band` capability MUST be capable of decoding in-band parameter sets that duplicate the parameter sets received out-of-band.

  All the parameter sets used by the bitstream MUST be compliant with the profile and level explicitely or implicitely declared using an out-of-band transport specific mechanism or in the members profile_idc, level_idc and constraint_set\<N\>_flag of the AVC_video_descriptor of an MPEG2-TS transport stream. The parameter sets MAY be specified in-band through the H.264 bitstream, out-of-band or in-and-out-of-band usign an unspecified out-of-band mechanisms. See the "Parameter Sets" section for more details.

  A Receiver supporting `in_and_out_of_band` MUST also support the `in_band` and `out_of_band` modes. Such Receiver SHOULD have all "in_band", "out_of_band" and "in_and_out_of_band" values enumerated in the Receiver Capabilities in order to allow Senders operating in any `parameter_sets_transport_mode`.

### Other transports

For Nodes consuming H.264 using other transports, the Receiver resource MUST indicate the associated `urn:x-nmos:transport:` label of the transport or one of its subclassifications for the `transport` attribute.

The following parameter constraints can be used to express limits or preferences specifically defined for H.264 decoders:

- [Parameter Sets Flow Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#parameter-sets-flow-mode)
  
  A Receiver declares the `parameter_sets_flow_mode` capability to indicate that it supports bitstreams using parameter sets associated with strictly one (`strict`), one (`static`) or multiple (`dynamic`) Flows. Considering that an active parameter set is associated with a specific Flow, this capability indicates that a Receiver is capable or not of decoding an H.264 bitstream where the associated Flow properties change dynamically. 

  A Receiver supporting the `dynamic` mode MUST also support the `strict` and `static` modes. Such Receiver SHOULD have `strict`, `static` and `dynamic` values enumerated in the Receiver Capability in order to allow Senders operating in any `parameter_sets_flow_mode`.

- [Parameter Sets Transport Mode](https://specs.amwa.tv/nmos-parameter-registers/branches/main/capabilities/#parameter-sets-transport-mode)

  A Receiver declares the `parameter_sets_transport_mode` capability to indicate that it supports bitstreams using parameter sets provided either only out-of-band, only in-band or in-and-out-of-band. The in-band parameter sets MAY update, augment or duplicate the parameter sets received out-of-band. A Receiver declaring the `in_band` or `in_and_out_of_band` capabilities MUST be capable of decoding in-band parameter sets that update, augment or duplicate the parameter sets received out-of-band. A Receiver declaring the `out_of_band` capability MUST be capable of decoding in-band parameter sets that duplicate the parameter sets received out-of-band.

  Informative note: The out of band mechanism used to transmit parameter sets is transport specific and out of the scope of this specification.

  All the parameter sets used by the bitstream MUST be compliant with the profile and level explicitely or implicitely declared using an out-of-band transport specific mechanism or in the members profile_idc, level_idc and constraint_set\<N\>_flag of the AVC_video_descriptor of an MPEG2-TS transport stream. The parameter sets MAY be specified in-band through the H.264 bitstream, out-of-band or in-and-out-of-band usign an unspecified out-of-band mechanisms. See the "Parameter Sets" section for more details.

  A Receiver supporting `in_and_out_of_band` MUST also support the `in_band` and `out_of_band` modes. Such Receiver SHOULD have all "in_band", "out_of_band" and "in_and_out_of_band" values enumerated in the Receiver Capabilities in order to allow Senders operating in any `parameter_sets_transport_mode`.

## H.264 IS-05 Senders and Receivers
### RTP transport

  Connection Management using IS-05 proceeds in exactly the same manner as for any other stream format carried within RTP.

  If IS-04 Sender `manifest_href` is not `null`, the SDP transport file at the **/transportfile** endpoint on an IS-05 Sender MUST comply with the same requirements described for the SDP transport file at the IS-04 Sender `manifest_href`.

  A `PATCH` request on the **/staged** endpoint of an IS-05 Receiver can contain an SDP transport file in the `transport_file` attribute. The SDP transport file for a H.264 stream is expected to comply with RFC 6184 and, if appropriate, ST 2110-22 or IPMX. It need not comply with the additional requirements specified for SDP transport files at Senders.

  If the Receiver is not capable of consuming the stream described by a `PATCH` on the **/staged** endpoint, it SHOULD reject the request. If it is unable to assess the stream compatibility because some parameters are not included `PATCH` request, it MAY accept the request and postpone stream compatibility assessment.
  
### Other transports

  Connection Management using IS-05 proceeds in exactly the same manner as for any other stream format carried within other transports.

  If IS-04 Sender `manifest_href` is not `null`, the SDP transport file at the **/transportfile** endpoint on an IS-05 Sender MUST comply with the same requirements described for the SDP transport file at the IS-04 Sender `manifest_href`.

  If the Receiver is not capable of consuming the stream described by a `PATCH` on the **/staged** endpoint, it SHOULD reject the request. If it is unable to assess the stream compatibility because some parameters are not included `PATCH` request, it MAY accept the request and postpone stream compatibility assessment.

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
[BCP-004-01]: https://specs.amwa.tv/bcp-004-01/ "AMWA BCP-004-01 NMOS Receiver Capabilities"

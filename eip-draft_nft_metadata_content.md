---
eip: <to be assigned>
title: Flexible NFT Content
description: Currently ERC721 and ERC1155 only specify the "image" field in metadata for images. This proposal adds specifications for a more flexible "content" field type.
author: Iain Nash <iain@iain.in>
discussions-to: https://www.nftstandards.wtf
status: Draft
type: Standards Track
category: ERC
created: 2022-12-26
---

## Simple Summary
Define an extension to the `EIP1155`, `EIP721` metadata JSON standard supporting metadata to allow for embedding rich media of any content type as an additional file to the NFT. "Content" and File attachments of NFTs.
    
## Abstract
While the NFT standard provides a way of identifying the tokens through an image, NFT users wish to attach more forms of rich media. This is a solved problem by using files and mime type.
We propose adding:<br><br>
 A `content` metadata field queried by clients allowing a file to be associated as the canonical media for the NFT file. The canonical file can be a full essay, original audio or video file or files that are too specific or cumbersome to include in the current NFT `image` field. Applications that can render this rich content can do so, but the larger intention is that archivists and users can understand the context and original work that the NFT is.

This sort of extension has been already realized for many NFT projects using custom fields such as `animationURL`, `contentURL`, `media` in metadata JSON files. If this file format and key were standardized:


1. off-chain "exotic" media types: including lossless OGG files, TIFF files, PDF files, high-quality uncompressed HD files, markdown, or even solidity code files

2. The ability to automatically index and make users aware of content stored as PDFs or Markdown while building a on-chain essay aggregation service with the specified `mime-type` field.

3. Cross-chain content verification hashes: content stored in IPFS / Arweave can be verified with a SHA256 hash in the metadata as a standard and allow for automated verification of the URI contents. This mirrors what `manifold` uses with `image_details` and the original ZORA cryptomedia specification.

This greatly expands the utility of the `NFT` ability to make unique and broad forms of data in standard files more clear and usable.
    
## Motivation
    
The ERC721/1155 standard defines a standard file format for `metadata.json`. We propose an optional extension key to the `metadata.json` file that includes a custom `content` key. This key points to an JSON object including the fields: `mime`, `uri`, `alt`, and `sha256`.<br>
The original `image` field should be retained as an image that best describes a preview of the `content` field of the NFT.<br>
While every project has its own metadata requirements, this EIP aims to unleash the creativity of NFT creators, by providing a common base to describe NFT content information.<br>

## Specification
    
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.<br>

Smart contracts implementing this ERC MUST implement the metadata JSON keys as a prescribed JSON object:

`content`

Within the `content` field, `uri` and `mime` are required. The `uri` field MUST conform to the RFC 3986 URI standard. The `mime` field MUST conform to the RFC 6838 Media Types standard using types defined by IANA. `alt` and `hash` are recommended but not required fields. `alt` can be any valid json string and `hash` is defined as an RFC 6234 `SHA-256` hash encoded as a lowercase hex string.
<br>
If `uri` is provided, `hash` and `mime` fields are strongly recommended but not required. 
<br>
It is up to the parser to validate this JSON structure since any keys not defined by the NFT specification are not defined and can be used by previous NFT collections. At Dec. 26th 2022, only NNN 721-standard NFT collections utilized the `content` key.
<br>

## Rationale
This design is inspired from the [IANA Media Types (formerly known as MIME types)](https://www.iana.org/assignments/media-types/media-types.xhtml), the same way an operating system recognizes supported formats with the available installed software, wallet or marketplace clients can check if they "support" a given NFT. The document does not intend to describe all possible NFT formats but provide an effective way to identify the NFT type.

Creators are free to imagine new content types that provide additional on-chain or off-chain metadata content. This method can be used for linking large content types such as AI models, on-chain SVG that may break normal gas limits, or even code to the NFT. Most platforms SHOULD show these links as source full-resolution files, download links, or re-encode the media and validate it can render on users clients instead of directly attempting to render or display this media online.

## Backwards Compatibility

This EIP does not introduce any backward compatibility with existing NFT standards. It extends these standards by adding the possibility of having additional metadata.

NFT creators should keep the existing ERC721 (and ERC1155) metadata URI schemes on top of new content types to compliment this standard and provide metadata associated with the content.
    
This standard is compatible with the metadata format in the ERC721 standard and by extension, the ERC1155 standard as well.


## Common questions

1. Why not use an on-chain getter function?
    There has been a lack of recent NFT projects using custom getter functions over data-uri generation techniques. We see this metadata format relatively flexible and still compatible with the given url generation schemes on the solidity side.

2. Should more specific media types to render be defined?
    The goal of this field is not to provide direct rendering support but rich media support for aware applications and archivists wishing to study, display, or save the original canonical media associated with the non-fungible token.

3. How are translations handled for the `alt` text?

## Reference Implementation

See the example in the Specification section.

## Security Considerations

Clients that decide to support and implement executable content (binary, javascript...) must ensure that proper security measures are in place before running any code on the client.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

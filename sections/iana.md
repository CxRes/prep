# IANA Considerations

The change controller for the following registrations is: "IETF (iesg@ietf.org) - Internet Engineering Task Force".

## Header Field Registration

IANA has registered the following entry in the "[Hypertext Transfer Protocol (HTTP) Field Name Registry](https://www.iana.org/assignments/http-fields/)" defined by [HTTP]:

| Header Field Names | Status       | Reference          |
|-
| =Accept-Events=    | Provisional  | {{header-fields}}  |
| =Events=           | Provisional  | {{header-fields}}  |

## Notifications Protocol Registry

The =Accept-Events= and =Events= header fields identify specific protocols for notifications. The "HTTP Notifications Protocol Registry", created and maintained by IANA at <https://www.iana.org/assignments/http-parameters/>, registers identifiers for notification protocols.

Notifications protocol registrations MUST include the following fields:

+ Identifier
+ Name
+ Pointer to specification text

Initial registrations are given below:

| Identifier | Name                          | Reference      |
|-
| PREP       | Per Resource Events Protocol  | This document  |

Values to be added to this namespace require IETF Review (see {{RFC8126, Section 4.8}}). Identified protocols MUST conform to the purpose of sending notifications as defined in {{header-fields}} of this document.

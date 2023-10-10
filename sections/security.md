# Security Considerations

Since the {{&protocol}} uses HTTP to transmit notifications, it follows that the security and privacy considerations that apply to HTTP also apply to PREP. Considerations relevant to HTTP semantics and its use for transferring information over the Internet are discussed in {{Section 17 of HTTP}}, and considerations related to HTTP/1.1 message syntax and parsing are discussed in {{Section 11 of -HTTP1}}.

The following is a non-exhaustive list of security and privacy considerations that become especially pertinent due to the manner in which PREP uses HTTP for transmitting notifications:

## Browser Fingerprinting

The =Accept-Events= header field provides an extra vector that can aid unique identification of user agent. Follow the advice in {{HTTP, Section 17.13}} to minimize the risk of fingerprinting.

## Denial-of-Service Attacks

Resources that serve notifications, by virtue of keeping the response stream open for an extended period of time are more susceptible to Denial-of-Service attacks because the effort required to request notifications from the same resource is tiny compared to the time, memory, and bandwidth consumed by attempting to serve the notifications. Servers ought to ignore, coalesce, or reject egregious notification request, such as repeated notification requests to resource from the same origin.

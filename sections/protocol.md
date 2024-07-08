# Protocol {#protocol}

## Event Fields {#event-fields}

The {{&protocol}} reuses existing HTTP fields ({{HTTP, Section 5}}) as event fields. Any HTTP field MAY be used as an event field. For the limited context of notifications using the {{&protocol}}, an event field with the same name as an HTTP field MUST have identical semantics to that HTTP field, unless otherwise specified.

This specification restricts =Accept-Events= and =Events= as Structured header fields* {{HTTP-SF}}. From this it follows:

+ An event field whose value is anything except a bare Item, that is, an Item without Parameters, MUST be specified inside an Inner List.
+ Unless otherwise specified, an event field that is not already defined as a Structured Field, therefore, MUST be handled as a Retrofit Structured Field {{HTTP-Retrofit}} when such handling is defined.
+ An event field that is not already defined as a Structured Field but cannot be handled as a Retrofit Structured Field either, MUST be explicitly specified by the implementation.

## Methods {#methods}

For the {{&protocol}}, `HEAD` ({{HTTP, Section 9.3.2}}) and `GET` ({{HTTP, Section 9.3.1}}) are the only methods in response to which notifications are advertised.

A resource server MUST NOT send the =Accept-Events= header field with `prep` as a protocol in response to a request with any method other than `HEAD` or `GET`.

For the {{&protocol}}, `GET` ({{HTTP, Section 9.3.1}}) is the only method by which notifications are requested and for which notifications response is defined.

An application client MUST NOT send the =Accept-Events= header field with `prep` as a protocol in a request with any method other than `GET`.

A resource server MUST NOT send the =Events= header field with the parameter `protocol` with a value of `prep` in response to a request with any method other than `GET`.

A resource server MUST NOT send the =Events= header field except in response to a `GET` request.

## Status Codes {#status-codes}

The {{&protocol}} reuses existing HTTP status codes ({{HTTP, Section 15}}) to describe the result of the request for notifications and the semantics of notifications in the response.

In response to a request where =Accept-Events= header field indicates `prep` as the preferred protocol, a resource server that supports notifications using the {{&protocol}} MUST communicate the status code for the notifications response using the `status` parameter in the =Events= header field.

For the limited context of notifications using the {{&protocol}}, the status code communicated using the `status` parameter in the =Events= header field MUST have identical semantics to the corresponding HTTP status code, unless otherwise specified.

# Header Fields {#header-fields}

The {{&protocol}} introduces new header fields. These header fields are not specific to the {{&protocol}}. They can be used by other protocols that augment resources with the ability to send notifications.

Protocols MUST ensure that the semantics be so defined that these header fields are safely ignored by recipients that do not recognize them ({{HTTP, Section 5.1}}).

*[=Accept-Events=]: #accept-events-header (((Accept-Events))) `Accept-Events`

## Accept-Events {#accept-events-header}

The =Accept-Events= header field can be used by application clients to specify their preferred protocol for receiving notifications. For example, the =Accept-Events= header field can be used to indicate that the request is specifically limited to a small set of desired protocols, as in the case for notifications with the {{&protocol}}.

When sent by a resource server in a response, the =Accept-Events= header field provides information about which notification protocols are preferred in a subsequent request to the same resource.

An application client MAY generate a request for notifications regardless of having received an =Accept-Events= header field. The information only provides advice for the sake of improving performance and reducing unnecessary network transfers.

Conversely, an application client MUST NOT assume that receiving an =Accept-Events= header field means that future requests will return notifications. The content might change, the server might only support notifications requests at certain times or under certain conditions, or a different intermediary might process the next request.

### Validity {#accept-events-header-validity}

A recipient MUST ignore the =Accept-Events= header field received with a request method that is unrecognized or for which notifications response is not defined for a particular notifications protocol.

A recipient MUST ignore the =Accept-Events= header field received in response to a request method that is unrecognized or for which notifications discovery and/or response is not defined for a particular notifications protocol.

A recipient MUST ignore the =Accept-Events= header field that it does not understand.

A recipient MUST ignore the protocols specified in the =Accept-Events= header field that they are not aware of.

### Syntax {#accept-events-header-syntax}

=Accept-Events= is a List structured ({{HTTP-SF, Section 3.1}}) header field. Its members MUST be of type string that identifies a notification protocol. A protocol identifier MAY be followed with zero or more parameters defined by the given protocol, which MAY be followed by a `q` parameter.

The `q` parameter assigns a relative "weight" to the sender's preference for a notification protocol with semantics as defined in {{HTTP, Section 12.4.2}}. Senders using weights SHOULD send `q` last (after all protocol parameters). Recipients SHOULD process any parameter named `q` as weight, regardless of parameter ordering.

Note: Use of the `q` parameter name to negotiate notification protocols would interfere with any parameter having the same name. Hence, protocol parameters named `q` are disallowed.

*[=Events=]: #events-header (((Events))) `Events`

## Events {#events-header}

The =Events= header field is sent by a resource server to provide event fields in response to a request for notifications.

Where the =Accept-Events= header field sent in the request is ignored, a resource server MUST NOT send the =Events= header field in a response.

Conversely, a resource server MUST send the =Events= header field in a response, if the =Accept-Events= header field sent in the request is not ignored.

### Validity {#events-header-validity}

If the =Events= header field is sent in response to a request that does not contain the =Accept-Events= header field, the recipient MUST treat the response as invalid.

If the response contains a =Events= header field that the recipient does not understand or the =Events= header field specifies a `protocol` that the recipient does not understand, the recipient MUST NOT process the response. A proxy that receives such a message SHOULD forward it downstream.

### Syntax {#events-header-syntax}

=Events= is a Dictionary structured ({{HTTP-SF, Section 3.1}}) header field. It MUST contain one member with the key `protocol` whose value identifies the notification protocol used in the response. It MAY contain other members that are defined by the given notification protocol.

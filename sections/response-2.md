# Notifications Response {#notifications-response}

<!--

In response to a `GET` request with a =Accept-Events= header field with `prep` as the preferred notifications protocol, a resource server providing notifications:

+ MUST respond with a status code identical to the one that would have been sent with the response had notifications not been requested.
+ MUST include the message body that would have been transmitted had notifications not been requested, unless the `Prefer` header field {{RFC7240}} indicates a preference of `return=minimal` ({{RFC7240 Section 4.2}}).

-->

## Headers {#notifications-response-headers}

A resource server providing notifications using the {{&protocol}}:

+ MUST include the following header fields in the response:

  + `Date` ({{HTTP, Section 6.6.1}}).
  + `Content-Type` ({{HTTP, Section 8.3}}) with the media type set to `multipart/mixed` ({{RFC2046, Section 5.1.3}}).
  + =Events= which MUST include the following event fields:
    + `protocol` set to the value `prep`.
    + `status` set to the `200 (OK)` ({{HTTP, Section 15.3.1}}) status code.
    + `expires` with an integer value in seconds indicating an interval after `Date` when notifications will no longer be sent and the response stream is closed.

    NOTE: Since caching is meaningless in the context of notifications, this specifications repurposes the deprecated `Expires` header field as an event field to specify when the notifications response ends.

+ SHOULD include the following header fields in the response:

  + `Vary` ({{HTTP, Section 12.5.5}}) which MUST include the following values:
    + =Accept-Events=.
    + `Last-Event-ID`, if the request for PREP notifications included the `Last-Event-ID` header field that was not ignored by the resource server.
  + `Last-Modified` ({{HTTP, Section 8.8.2}}).

+ MAY include the following header fields in the response:

  + =Accept-Events= which MUST list `prep` as one of the available notification protocols. Associated with the `prep` list item, the resource server:
    + MUST include an `accept` event field with at least one acceptable media-type for notifications.

## Body {#notifications-response-body}

A resource server sending PREP Notifications MUST transmit a multipart message body ({{RFC2046, Section 5.1}}) with a media type of `multipart/mixed` ({{RFC2046, Section 5.1.3}}) with two body parts in the order specified below:

1. Base Response: the message body that would have been sent had notifications not been requested.
2. Notifications Response: the multipart response body with body parts containing notifications as defined in {{notifications-part-body}}.

### Base Response {#base-response-part-body}

By default, the {{&protocol}} requires a resource server to transmit the base response body as first body part of multipart response body.
This behaviour is desirable in most scenarios as we short-circuit an extra round trip that would otherwise be needed to fetch the current representation of the resource before fetching notifications and eliminates the need to co-ordinate the two responses.

#### Opt out {#base-response-opt-out}

An application client requesting only notifications using the {{&protocol}} needs to explicitly opt out of receiving the base response as described in {{request}}, {{<<request}}.

If the request for PREP notifications includes the `Last-Event-ID` header field ({{SSE}}, [Section 9.2.4](SSE#the-last-event-id-header)) which matches the `Event-ID` of the last event on the resource or wildcard character "*", a resource server SHOULD NOT send the content of the base response.

The resource server MUST send still the first body part of the multipart response body, even if this part is empty. The resource server MAY send the headers corresponding to the representation body that would have been sent in the base response, and only omit the representation body of base response.

### Notifications {#notifications-part-body}

A resource server MUST transmit PREP notifications in the second body part of the multipart response as yet another embedded multipart message body ({{RFC2046, Section 5.1}}), with a media type of `multipart/digest` ({{RFC2046, Section 5.1.5}}) that MAY include zero or more body parts. Each body part of the notifications multipart message body MAY contain at most one notification.

{:aside}
> **Implementation Guidance**
>
> While not strictly necessary, it is strongly encouraged that resource servers send notifications in a manner such that the boundary delimiter ({{RFC2046, Section 5.1.2}}) is always at the end of a chunk ({{-HTTP1, Section 7.1}}) or data frame ({{-HTTP2, Section 6.1}}) as shown in the examples below. This way an application client does not have to wait until the next chunk or frame (which might be a while or in cases of error never arrive) to be certain that the immediate message is complete.

## Termination {#notifications-response-termination}

A resource server MUST end the notification response in any one of the following scenarios:

+ Once the time specified in the `expires` parameter of the =Events= header field has elapsed.
+ Immediately after sending a notification upon a `DELETE` request on the resource that results in a response with `200 (OK)` ({{HTTP, Section 15.3.1}}) or `204 (No Content)` ({{HTTP, Section 15.3.5}}) status codes.

A resource server MUST properly terminate the multipart responses as defined in {{RFC2046, Section 5.1.2}}, before closing the response stream.

{:aside}
> **Implementation Guidance**
>
> When a user navigates away from a website or an application using PREP notifications, application clients are strongly encouraged to properly close the response stream to ensure that servers do not keep sending notifications.

### Examples

#### Response with Current Representation and Notifications

~~~
{::include examples/notifications/full-response.http}
~~~
{: sourcecode-name="full-response.http" #full-response-example title="Notification Response with Representation included"}

#### Response with Notifications Only

~~~
{::include examples/notifications/basic-response.http}
~~~
{: sourcecode-name="basic-response.http" #basic-response-example title="Response with Notifications Only"}

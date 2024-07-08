# Request {#request}

The {{&protocol}} extends the content negotiation mechanism provided by HTTP allowing application clients to negotiate notifications independent of the base response.

In order to receive notifications using the {{&protocol}} from a resource, an application client sends a `GET` request to resource server, which:

+ MUST include the =Accept-Events= header field, which:
  + MUST list `prep` as a preferred notification protocol.
    + MAY include zero or more event fields. For example, application clients MAY specify an `accept` event field to indicate a preferred media-type for notifications.
+ MAY include the `Last-Event-ID` header field ({{SSE}}, [Section 9.2.4](SSE#the-last-event-id-header)) requesting the resource server to not send the base response body and resume notifications from the event occurring immediately after the one specified.

{:aside}
> **Implementation Guidance**
>
> Set the `Last-Event-ID` to a wildcard `*` to explicitly request a resource server to not send the base response body in the response at all.

~~~ http-message
{::include examples/notifications/request.http}
~~~
{: sourcecode-name="notifications-request.http" #notifications-request-example title="Request for Notifications"}

A resource server MUST ignore event fields for `prep` notifications in the =Accept-Events= header that it does not recognize or implement.

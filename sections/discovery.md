# Discovery {#notifications-discovery}

Application clients can engage in reactive content negotiation to discover if a resource server supports notifications using the {{&protocol}} on a given resource.

## Request {#discovery-request}

An application client can discover the ability of a resource server to deliver PREP notifications for a target resource by sending a `HEAD` ({{HTTP, Section 9.3.2}}) request.

## Not Available {#discovery-response-when-notifications-not-available}

In the response to a `HEAD` request, a resource server that does not provide notifications for the target resource using the {{&protocol}} MUST NOT list `PREP` as as one of the available protocols in the =Accept-Events= header field.

~~~ http-message
{::include examples/discovery/request.http}
~~~
{: sourcecode-name="discovery-request.http" #discovery-request-example title="Discovery Request"}

## Available {#discovery-response-when-notifications-available}

In response to a `HEAD` request, a resource server that serves notifications for the target resource using the {{&protocol}} SHOULD include the =Accept-Events= header field, which MUST list `PREP` as one of the available protocols.

Associated with `PREP` list item, the resource server MUST include an `accept` event field with at least one acceptable media-type for notifications.

~~~ http-message
{::include examples/discovery/response.http}
~~~
{: sourcecode-name="discovery-response" #discovery-response-example title="Discovery Response"}

{:aside}
> **Implementation Guidance**
>
> When resource servers support HTTP/2 {{-HTTP2}} for a resource, they are strongly encouraged to advertise it in the response.
>
> When a resource is accessible from different locations, resource servers are encouraged to advertise these locations in the response.
>
> HTTP Alternative Services {{RFC7838}}, for example, describes a mechanism for advertising these capabilities.

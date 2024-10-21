# Introduction {#introduction}

The {{&protocol}} defines a minimal HTTP-based framework by which clients can securely receive update notifications directly from any resource of interest on the Open Web Platform.

## Motivation {#motivation}

HTTP was originally designed to transfer a static documents within a single request and response. If the document changes, HTTP does not automatically update clients with the new versions. This design was adequate for web pages that were mostly static and written by hand.

But web-applications today are dynamic. They provide (near-)instantaneous updates across multiple clients and servers. The many workarounds developed over the years to provide real-time updates over HTTP have often proven to be inadequate. Web programmers instead resort to implementing custom messaging systems over alternate protocols such as WebSockets, which requires additional layers of code in the form of non-standard JavaScript frameworks to synchronize changes of state.

Per Resource Events is a minimal protocol built on top of HTTP that allows clients to receive notifications directly from a resource of interest. Unlike other HTTP based solutions, {{&protocol}} supports the use of arbitrary media-types for notifications, which can be negotiated just like representations; thus giving implementers a lot of flexibility to customize notifications according to the needs of their application.

Our goal with the {{&protocol}} is to make notification convenient for consumers. The {{&protocol}} allows a client to receive notifications together with the representation, saving on the unnecessary round trip. With a library like PREP Fetch, Per Resource Events may be consumed in JavaScript with just a few lines of code:

~~~
const response = fetch('http://example.com', {
  headers: { 'Accept-Events': '"prep"' }
});
const prepResponse = prepFetch(response);

const representation = await prepResponse.getRepresentation();
// Do something with the representation
// API identical to fetch Response

const notifications = await prepResponse.getNotifications();
for await (const notification of notifications) {
  // do something with a notification
  // API identical to fetch Response
}
~~~
{: #prep-fetch-example title="PREP Fetch Example"}

## How it Works {#how-it-works}

### {{&empty}}
{: anchor="plain-request" numbered="false" toc="exclude"}

Consider an ordinary HTTP `GET` request:

~~~
{::include examples/introduction/plain-request.http}
~~~
{: sourcecode-name="plain-request.http" #plain-request-example title="A sample HTTP `GET` request"}

### {{&empty}}
{: anchor="simple-prep-request" numbered="false" toc="exclude"}

A client application that wishes to receive PREP notifications from a resource simply makes a `GET` request with just one additional =Accept-Events= header.

~~~
{::include examples/introduction/minimal-request.http}
~~~
{: sourcecode-name="minimal-request.http" #minimal-request-example title="A minimal PREP notifications request"}

Additional parameters might be added to the =Accept-Events= header to negotiate the form of notifications as discussed in {{request}}, {{<<request}}.

### {{&empty}}
{: anchor="plain-response" numbered="false" toc="exclude"}

If a server does not implement the {{&protocol}}, the =Accept-Events= header in a `GET` request is simply ignored. The resource returns the current representation thereby preserving backwards compatibility. Let us presume this response is:

~~~
{::include examples/introduction/plain-response.http}
~~~
{: sourcecode-name="plain-response.http" #plain-response-example title="Response to the sample HTTP `GET` request"}

### {{&empty}}
{: anchor="simple-prep-response" numbered="false" toc="exclude"}

However, if the server supports the {{&protocol}}, it sends a multipart response with the current representation followed any notifications.

#### {{&empty}}
{: anchor="simple-prep-response-headers" numbered="false" toc="exclude"}

The response now includes an additional =Events= headers which specifies `prep` as the notifications protocol and a status for the notifications response. As a courtesy, the response also includes the `Vary` header to indicate that response was influenced by the `Accept-Events` header in the request and the =Accept-Events= header itself for reactive negotiation in the future.

The `Content-type` header now indicates a response body of `multipart/mixed` to reflect the two part response. Thus, we have the following response headers:

~~~
{::include examples/introduction/response-headers.http}
~~~
{: sourcecode-name="intro-response-headers.http" #intro-response-headers title="Response with PREP Notifications - Headers"}

#### {{&empty}}
{: anchor="simple-prep-response-current-representation" numbered="false" toc="exclude"}

The first part of this multipart response is the current representation of the resource:

~~~
{::include examples/introduction/response-current-representation.http}
~~~
{: sourcecode-name="intro-response-current-representation.http" #intro-response-current-representation title="Response with PREP Notifications - Current Representation"}

The client can request for this to be skipped by specifying a `Last-Event-Id` header set either to the ID of the previous representation or `*` as described in {{request}}.

#### {{&empty}}
{: anchor="simple-prep-response-notification" numbered="false" toc="exclude"}

The second part of this multipart response is itself a multipart message that contains notifications. Upon a resource event, a notification is transmitted as a part of this multipart message.

By default, notifications are sent in the `message/rfc822` format (which is structurally identical to a HTTP/1.1 message) with some additional semantics as specified in {{rfc822-semantics}}. Alternate formats and semantics might be negotiated using the =Accept-Events= header.

The response stream is closed when the time limit for notification has elapsed or immediately after the resource is deleted as in the example below:

~~~
{::include examples/introduction/response-notifications.http}
~~~
{: sourcecode-name="intro-response-notifications.http" #intro-response-notifications title="Response with PREP Notifications - Notifications"}

#### {{&empty}}
{: anchor="simple-prep-response-complete" numbered="false" toc="exclude"}

Together, the complete response with PREP notifications is:

~~~
{::include examples/introduction/response-headers.http}

{::include examples/introduction/response-current-representation.http}

{::include examples/introduction/response-notifications.http}
~~~
{: sourcecode-name="intro-response.http" #intro-response title="Complete Response"}

## Scope {#scope}

The {{&protocol}} specifies:

+ a mechanism for the discovery of notification capabilities per resource,
+ a mechanism to request notifications from a resource,
+ representation and semantics for the response that transmits notifications,
+ a default representation for notifications and associated semantics when used to transmit notifications.

The {{&protocol}} does not specify:

+ a specific authentication and authorization mechanism to be used with the {{&protocol}}. Implementations are encouraged to use existing approaches for authentication and authorization.
+ representation and semantics for notifications (other than the default case).
  + Implementations are free to use any media-type for notifications, which can be negotiated just like representations.
  + Implementations are also free to define additional semantics for a given media-type, when used to transmit notifications using the {{&protocol}}.

## Audience {#intended-audience}

This specification is for:

+ [Server developers](http://data.europa.eu/esco/occupation/a7c1d23d-aeca-4bee-9a08-5993ed98b135) who wish to enable clients to listen for updates to particular resources.
+ [Application developers](http://data.europa.eu/esco/occupation/c40a2919-48a9-40ea-b506-1f34f693496d) who wish to implement a client to listen for updates to particular resources.

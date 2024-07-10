## Classes of Products {#classes-of-products}

The {{&protocol}} identifies the following Classes of Products for conforming implementations. These products are referenced throughout this specification.

{: #resource-server}Resource Server
: that builds on HTTP origin server ({{HTTP, Section 3.6}}) by defining header fields ({{HTTP, Section 6.3}}) and media types ({{HTTP, Section 8.3.1}}).

{: #application-client}Application Client
: that builds on HTTP user agent ({{HTTP, Section 3.5}}) by defining behaviour in terms of fetching {{FETCH}} across the platform.

*[resource server]: #resource-server
*[Resource server]: #resource-server (((resource server)))
*[resource servers]: #resource-server (((resource server)))
*[Resource servers]: #resource-server (((resource server)))

*[application client]: #application-client
*[Application client]: #application-client (((application client)))
*[application clients]: #application-client (((application client)))
*[Application clients]: #application-client (((application client)))

## Interoperability {#interoperability}

Interoperability occurs between [Application Client—Resource Server](#interoperability-1) as defined by the {{&protocol}}.

{: #interoperability-1}Application Client—Resource Server Interoperability
: Interoperability of implementations for application clients and resource servers is tested by evaluating an implementation's ability to request and respond to HTTP messages that conform to the {{&protocol}}.

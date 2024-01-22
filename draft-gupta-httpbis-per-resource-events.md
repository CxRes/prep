---
title: "Per Resource Events"
abbrev: "PREP"
category: std

docname: draft-gupta-httpbis-per-resource-events-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Applications and Real-Time"
workgroup: "HTTP"
keyword:
  - notifications
  - pubsub
venue:
  group: "HTTP"
  type: "Working Group"
  mail: "ietf-http-wg@w3.org"
  arch: "https://lists.w3.org/Archives/Public/ietf-http-wg/"
  github: "CxRes/prep"
  latest: "https://CxRes.github.io/prep/draft-gupta-httpbis-per-resource-events.html"

author:
 -
    fullname: "Rahul Gupta"
    email: "cxres+ietf@protonmail.com"

autolink-iref-cleanup: true
entity:
  empty: ""
  protocol: Per Resource Events Protocol

normative:
  Delta: RFC3229
  HTTP: RFC9110
  HTTP-SF: I-D.ietf-httpbis-sfbis-03
  HTTP-Retrofit: I-D.ietf-httpbis-retrofit-06
  RFC822:
  RFC2046:
  RFC5789:
  RFC8126:
  RFC9112:
    -: HTTP1
    display: HTTP/1.1
  RFC9113:
    -: HTTP2
    display: HTTP/2
  SSE: W3C.eventsource

informative:
  FETCH:
    target: https://fetch.spec.whatwg.org
    title: Fetch - Living Standard
    date: false
  REST:
    target: https://roy.gbiv.com/pubs/dissertation/top.htm
    title: Architectural Styles and the Design of Network-based Software Architectures
    author:
      -
        ins: R.T. Fielding
        name: Roy T. Fielding
    date:
      month: September
      year: 2000
    refcontent: Doctoral Dissertation, University of California, Irvine
  RFC7838:
  RFC9205:
  WEBSUB: W3C.websub
  WS: W3C.websockets


--- abstract

Per Resource Events is a minimal protocol built on top of HTTP that allows clients to receive notifications directly from any resource of interest. The Per Resource Events Protocol (PREP) is predicated on the idea that the most intuitive source for notifications about changes made to a resource is the resource itself.


--- middle

<!-- Informative Sections -->

{::include-nested sections/introduction.md}

{::include sections/design.md}


<!-- Conformance Section -->

{::include boilerplate/conformance.md}

{::include sections/conformance.md}

{::include sections/terminology.md}


<!-- Normative Sections -->

{::include sections/headers.md}

{::include sections/protocol.md}

{::include-nested sections/discovery.md}

{::include-nested sections/request.md}

{::include sections/response-1.md}

{::include-nested sections/response-2.md}

{::include sections/rfc822.md}

{::include sections/security.md}

{::include sections/iana.md}

--- back

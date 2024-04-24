# Implementation Guidance

A Connect/Express style middleware for parsing the `Accept-Events` header field is available at <https://github.com/CxRes/express-accept-events/> and can be installed from npm as `express-accept-events`. Complementing this is a Connect/Express style middleware that negotiates an appropriate notifications protocol on the resource based on the request made using the `Accept-Event` header field, available at <https://github.com/CxRes/express-negotiate-events/> and can be installed from npm as `express-negotiate-events`.

An early implementation of the Per Resource Events Protocol, also in the form of a Connect/Express style middleware, is available at <https://github.com/CxRes/express-prep/> and can be installed from npm as `express-prep`.

Express Accept Events, Express Negotiate Events and Express PREP are Free and Open Source Software, released under the Mozilla Public License v2.0.

The W3C Solid Community Group is developing a specification called Solid-PREP that defines representation and semantics for PREP notifications sent from LDP Resources hosted on Solid storage. This specification enables Solid servers to incorporate the Per Resource Events as a means for sending notifications. Solid-PREP is available at <https://github.com/solid/solid-prep/> and is released under the W3C Software and Document license - version 2023.

The Node Solid Server, an open source server that implements the Solid Protocol, is in the process of implementing notifications using the Per Resource Events Protocol and Solid-PREP at <https://github.com/nodeSolidServer/node-solid-server/tree/prep>.

The PREP Fetch <https://github.com/CxRes/prep-fetch/> library provides a convinient way to consume fetch {{FETCH}} respsones containing PREP notification. PREP Fetch itself is a thin wrapper on top of Multipart Fetch <https://github.com/CxRes/multipart-fetch/> that streams the parts of a multipart response as a series of Fetch Responses.

PREP Fetch and Multipart Fetch are Free and Open Source Software, released under the Mozilla Public License v2.0.

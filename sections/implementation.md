# Implementation Guidance

A Connect/Express style middleware for parsing the `Accept-Events` header field is available at <https://github.com/CxRes/express-accept-events/> and can be installed from npm as `express-accept-events`. Express-Accept-Events is FOSS, released under the Mozilla Public License v2.0.

An early implementation of the Per Resource Events Protocol, also in the form of a Connect/Express style middleware, is available at <https://github.com/CxRes/express-prep/> and can be installed from npm as `express-prep`. Express-PREP is FOSS, released under the Mozilla Public License v2.0.

The W3C Solid Community Group is developing a specification called Solid-PREP that defines representation and semantics for PREP notifications sent from LDP Resources hosted on Solid PODS. This specification enables Solid servers to incorporate the Per Resource Events for sending notifications. Solid-PREP is available at <https://github.com/solid/solid-prep/> and is released under the W3C Document license - version 2023.

The Node Solid Server, an open source server that implements the Solid Protocol, is in the process of implementing notifications at <https://github.com/nodeSolidServer/node-solid-server/tree/prep>, initially for linked data resources, using the Per Resource Events Protocol.

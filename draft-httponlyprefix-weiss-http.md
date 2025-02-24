---
title: "HttpOnly cookie prefix"
abbrev: "HttpOnlyPrefix"
category: info

docname: draft-httponlyprefix-weiss-http-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: AREA
workgroup: WG Working Group
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: WG
  type: Working Group
  mail: WG@example.com
  arch: https://example.com/WG
  github: USER/REPO
  latest: https://example.com/LATEST

author:
 -
    fullname: Yoav Weiss
    organization: Shopify
    email: yoav@yoav.ws

normative:

informative:


--- abstract

This draft introduces the __HttpOnly and __HttpOnlyHost cookie name prefixes
that ensure the cookie was set with an HttpOnly attribute.


--- middle

# Introduction

There are cases where it's important to distinguish on the server side
between cookies that were set by the server and ones that were set by the
client.
One such case are cookies that are normally *always* set by the server,
unless some unexpected code (an XSS exploit, a malicious extension, a
commit from a confused developer, etc) happens to set them on the client.

This draft add a signal that would enable servers to make such a distinction.

More specifically, it defines the __HttpOnly and __HttpOnlyHost prefixes,
that make sure that a cookie is not set on the client side using script.

## Conventions and Definitions

{::boilerplate bcp14-tagged}

# Cookie Name Prefixes

## The "__HttpOnly-" prefix

If a cookie's name begins with a case-sensitive match for the string __HttpOnly-,
then the cookie will only be set if the cookie is set:

1) Using a `Set-Cookie` HTTP header.
2) With the Secure attribute.
3) With the HttpOnly attribute.

## The "__HttpOnlyHost-" prefix

If a cookie's name begins with a case-sensitive match for the string __HttpOnly-,
then the cookie will only be set if the cookie is set:

1) Using a `Set-Cookie` HTTP header.
2) With the Secure attribute.
3) With the HttpOnly attribute.
4) A Path attribute with a value of /, and no Domain attribute.

# Security Considerations

There are no particular security considerations.
These new prefixes will only limit the ability of non-compliant cookies to be set.
They do not open up new capabilities for server to set cookies where they previously could not.


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.

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
# area: AREA
# workgroup: WG Working Group
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  github: "yoavweiss/httponly_prefix"
  latest: "https://yoavweiss.github.io/httponly_prefix/draft-httponlyprefix-weiss-http.html"

author:
 -
    fullname: Yoav Weiss
    organization: Shopify
    email: yoav@yoav.ws

normative:

  COOKIES:
    title: Cookies HTTP State Management Mechanism
    date: 20 February 2025
    target: https://datatracker.ietf.org/doc/draft-ietf-httpbis-rfc6265bis/

informative:


--- abstract

This draft introduces the __HttpOnly and __HttpOnlyHost cookie name prefixes
that ensure the cookie was set with an HttpOnly attribute.


--- middle

# Introduction

There are cases where it's important to distinguish on the server side
between cookies {{COOKIES}} that were set by the server and
ones that were set by the client.

One such case is cookies that are normally *always* set by the server,
unless some unexpected code (an XSS exploit, a malicious extension, a
commit from a confused developer, etc.) happens to set them on the client.

This draft add a signal that would enable servers to make such a distinction.

More specifically, it defines the __HttpOnly and __HttpOnlyHost prefixes,
that make sure that a cookie is not set on the client side using script.

## Conventions and Definitions

{::boilerplate bcp14-tagged}

# Server Requirements

These requirements apply to cookies received in a `Cookie` request header from the client.

## Cookie Name Prefixes

### The "__HttpOnly-" prefix

If a cookie's name begins with a case-sensitive match for the string __HttpOnly-,
then this indicates that **all** the following are true:

1. The cookie was originally created on the client using a `Set-Cookie` HTTP header sent from this server.
2. The `Set-Cookie` HTTP header included the `Secure` attribute.
3. The `Set-Cookie` HTTP header included the `HttpOnly` attribute.

### The "__HttpOnlyHost-" prefix

If a cookie's name begins with a case-sensitive match for the string __HttpOnlyHost-,
then this indicates that **all** the following are true:

1. The cookie was originally created on the client using a `Set-Cookie` HTTP header sent from this server
2. The `Set-Cookie` HTTP header included the `Secure` attribute.
3. The `Set-Cookie` HTTP header included the `HttpOnly` attribute.
4. The `Set-Cookie` HTTP header included the `Path` attribute with a value of `/`.
5. The `Set-Cookie` HTTP header did not include the `Domain` attribute.

# User Agent Requirements

These requirements apply to cookies received in a `Set-Cookie` response header from the server.

## Cookie Name Prefixes
User agents' requirements for cookie name prefixes differ slightly from servers', as UAs MUST match the prefix string case-insensitively.

### Storage Model
Add the following steps after step 21 of section 5.7 in {{COOKIES}}.

1. If the cookie-name begins with a case-insensitive match for the string "__HttpOnly-",
   1. Abort these steps and ignore the cookie entirely unless all the following conditions are true:
      1. The cookie's `secure-only-flag` is true.
      1. The cookie's `http-only-flag` is true.

1. If the cookie-name begins with a case-insensitive match for the string "__HttpOnly-",
   1. Abort these steps and ignore the cookie entirely unless all the following conditions are true:
      1. The cookie's `secure-only-flag` is true.
      1. The cookie's `http-only-flag` is true.
      1. The cookie's `host-only-flag` is true.
      1. The `cookie-attribute-list` contains an attribute with an `attribute-name` of "Path", and the cookie's path is "/".
      1. The `cookie-attribute-list` does not contain an attribute with an `attribute-name` of "Domain".

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

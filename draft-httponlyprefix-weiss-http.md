---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
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

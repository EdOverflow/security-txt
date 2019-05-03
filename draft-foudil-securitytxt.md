---
title: A Method for Web Security Policies
docname: draft-foudil-securitytxt-06
ipr: trust200902
cat: info
pi:
  sortrefs: 'yes'
  strict: 'yes'
  symrefs: 'yes'
  toc: 'yes'
author:
- ins: E. Foudil
  name: Edwin Foudil
  email: contact@edoverflow.com
- ins: Y. Shafranovich
  name: Yakov Shafranovich
  organization: Nightwatch Cybersecurity
  email: yakov+ietf@nightwatchcybersecurity.com

--- abstract
When security vulnerabilities are discovered by independent
security researchers, they
often lack the channels to report them properly. As a result,
security vulnerabilities may be left unreported. This document defines a format
("security.txt") to help organizations describe the process for security
researchers to follow in order to report security vulnerabilities.

--- middle

# Introduction

## Motivation and Prior Work

Many security researchers encounter situations where they are unable
to report security vulnerabilities to organizations because there is
no course of action laid out or no way indicated to contact the owner of a particular
resource.

As per section 4 of {{?RFC2142}}, there is an existing convention
of using the \<SECURITY@domain\> email address for communications regarding
security vulnerabilities. That convention provides only a single, email-based
channel of communication for security vulnerabilities per domain, and does not provide
a way for domain owners to publish information about their security disclosure
policies.

There are also contact conventions prescribed for Internet Service Providers (ISPs)
in section 2 of {{?RFC3013}}, for Computer Security Incident Response Teams (CSIRTs)
in section 3.2 of {{?RFC2350}} and for site operators in section 5.2 of
{{?RFC2196}}. As per {{?RFC7485}}, there is also contact information provided by
Regional Internet Registries (RIRs) and domain registries for owners of IP
addresses, autonomous system numbers (ASNs) and domain names. However, none of
these address the issue of how security researchers can locate disclosure
policies and contact information for organizations in order to report
security vulnerabilities.

In this document, we define a richer, machine-parsable and more extensible way
for organizations to communicate information about their security disclosure
policies, which is not limited to email and also allows for additional features
such as encryption. This format is designed to help
assist with the security disclosure process by making it easier
for organizations to designate the preferred steps for researchers to take
when trying to reach out to them with security vulnerabilities.

## Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and
"OPTIONAL" in this document are to be interpreted as described in
BCP 14 {{!RFC2119}} {{!RFC8174}} when, and only when, they appear in all
capitals, as shown here.

# Note to Readers

> **Note to the RFC Editor:**  Please remove this section prior
> to publication.

Development of this draft takes place on Github at: https://github.com/securitytxt/security-txt

# The Specification

This document defines a text file to be placed in a known location
that provides information for security researchers to assist
in disclosing security vulnerabilities.

The file is named "security.txt", and this file SHOULD be placed under the
/.well-known/ path ("/.well-known/security.txt") {{!RFC5785}} of a domain name or IP address for web
properties. If it is not possible to place the security.txt file in the /.well-known/ path or setup a redirect, web-based services MAY place the file in the top-level path
of a given web domain or IP address ("/security.txt") as a fallback option (see {{weblocation}}).

For web-based services, the file MUST be accessible via the Hypertext Transfer Protocol (HTTP) {{!RFC1945}} as a resource of Internet Media Type "text/plain" with the default charset parameter set to "utf-8" per section 4.1.3 of {{!RFC2046}}, and it MUST be served with "https" (as per section 2.7.2 of {{!RFC7230}}). For file systems and version control repositories a "security.txt" file SHOULD be placed in the root directory of a particular file system or source code project.

This text file contains multiple directives
with different values. The "directive" is the first part of a field all the way up
to the colon ("Contact:") and follows the syntax defined for "field-name" in section 3.6.8
of {{!RFC5322}}. Directives MUST be case-insensitive (as per section 2.3 of {{!RFC5234}}).
The "value" comes after the directive ("https://example.com/security") and follows the syntax
defined for "unstructured" in section 3.2.5 of {{!RFC5322}}.

A "field" MUST always consist of a directive and a value
("Contact: https://example.com/security"). A security.txt file
can have an unlimited number of fields. It is important to note that
you MUST have a separate line for every field. One MUST NOT chain multiple
values for a single directive unless it is explicitly defined by that particular field. Unless
otherwise indicated in a definition of a particular field,
any directive MAY appear multiple times.

## Scope

A "security.txt" file MUST only apply to the domain in the URI used to retrieve it,
not to any of its subdomains or parent domains. A "security.txt" file that is found
in a file system or version control repository MUST only apply to the folder
or repository in which it is located, and not to any of its parent or sibling folders,
or repositories. However, it will apply to all subfolders.

Some examples appear below:

~~~~~~~~~~
# The following only applies to example.com.
https://example.com/.well-known/security.txt

# This only applies to subdomain.example.com.
https://subdomain.example.com/.well-known/security.txt

# This security.txt file applies to IPv4 address of 192.0.2.0.
https://192.0.2.0/.well-known/security.txt

# This security.txt file applies to IPv6 address of 2001:db8:8:4::2.
https://[2001:db8:8:4::2]/.well-known/security.txt

# This security.txt file applies to the /example/folder1 directory and subfolders.
/example/folder1/security.txt
~~~~~~~~~~

## Comments

Any line beginning with the "#" (%x30) symbol MUST be interpreted as a comment.
The content of the comment may contain any ASCII or Unicode characters in the
%x21-7E and %x80-FFFFF ranges plus the tab (%x09) and space (%x20) characters.

Example:

~~~~~~~~~~
# This is a comment.
~~~~~~~~~~

You MAY use one or more comments as descriptive text immediately
before the field. Parsers SHOULD associate the comments with the
respective field.

## Separate Fields

A separate line is REQUIRED for every new value and field. You MUST
NOT chain everything into a single field unless defined by that field. Every line MUST end either
with a carriage return and line feed characters (CRLF / %x0D %x0A) or just
a line feed character (LF / %x0A).

## Digital signature {#signature}

It is RECOMMENDED that a security.txt file be digitally signed
using an OpenPGP cleartext signature as described in
section 7 of {{!RFC4880}}. When digital signatures are used, it is also
RECOMMENDED that implementors use the "Canonical" directive (as per {{canonical}}),
thus allowing the digital signature to authenticate the location of the file.

When it comes to verifying the key used to generate the signature, it is always
the security researcher's responsibility to make sure the key being
used is indeed one they trust.

## Field Definitions

### Acknowledgments {#acknowledgments}

This directive allows you to link to a page where security
researchers are recognized for their reports. The page being linked to
SHOULD list individuals or organizations that reported security vulnerabilities
and worked with you to remediate the issue. Organizations SHOULD be careful
to limit the vulnerability information being published in order
to prevent future attacks.

If this directive indicates a web URL, then it MUST begin with "https://"
(as per section 2.7.2 of {{!RFC7230}}).

Example:

~~~~~~~~~~
Acknowledgments: https://example.com/hall-of-fame.html
~~~~~~~~~~

Example security acknowledgments page:

~~~~~~~~~~
We would like to thank the following researchers:

(2017-04-15) Frank Denis - Reflected cross-site scripting
(2017-01-02) Alice Quinn  - SQL injection
(2016-12-24) John Buchner - Stored cross-site scripting
(2016-06-10) Anna Richmond - A server configuration issue
~~~~~~~~~~

### Canonical {#canonical}

This directive indicates the canonical URI where the security.txt file is located,
which is usually something like "https://example.com/.well-known/security.txt".
If this directive indicates a web URL, then it MUST begin with "https://"
(as per section 2.7.2 of {{!RFC7230}}). The purpose of this directive is to allow
a digital signature to be applied to the location of the "security.txt" file.

This directive MUST NOT appear more than once.

~~~~~~~~~~
Canonical: https://example.com/.well-known/security.txt
~~~~~~~~~~

### Contact {#contact}

This directive allows you to provide an address that researchers
SHOULD use for reporting security
vulnerabilities. The value MAY be an email address, a phone number and/or a
web page with contact information. The "Contact:" directive MUST
always be present in a security.txt file. If this directive indicates a web URL,
then it MUST begin with "https://" (as per section 2.7.2 of {{!RFC7230}}).
Security email addresses SHOULD use the conventions defined in section
4 of {{!RFC2142}}.

The value MUST follow the URI syntax described in {{!RFC3986}}.
This means that "mailto" and "tel" URI schemes MUST be used
when specifying email addresses and telephone numbers, as defined in {{!RFC6068}}
and {{!RFC3966}}. When the value of this directive is an email address,
it is RECOMMENDED that encryption be used (as per {{encryption}}).

The precedence SHOULD be in listed order. The first field is the preferred
method of contact. In the example below, the email address is
the preferred method of contact.

~~~~~~~~~~
Contact: mailto:security@example.com
Contact: tel:+1-201-555-0123
Contact: https://example.com/security-contact.html
~~~~~~~~~~

### Encryption {#encryption}

This directive allows you to point to an encryption key that
security researchers SHOULD use for encrypted communication. You MUST NOT
directly add your key to the field, instead the value of this field
MUST be a URI pointing to a location where the key can be retrieved from.
If this directive indicates a web URL, then it MUST begin with "https://"
(as per section 2.7.2 of {{!RFC7230}}).

When it comes to verifying the authenticity of the key, it is always the security
researcher's responsibility to make sure the key being specified is indeed one
they trust. Researchers MUST NOT assume that this key is
used to generate the digital signature referenced in {{signature}}.

Example of an OpenPGP key available from a web server:

~~~~~~~~~~
Encryption: https://example.com/pgp-key.txt
~~~~~~~~~~

Example of an OpenPGP key available from an OPENPGPKEY DNS record:

~~~~~~~~~~
Encryption: dns:5d2d37ab76d47d36._openpgpkey.example.com?type=OPENPGPKEY
~~~~~~~~~~

Example of an OpenPGP key being referenced by its fingerprint:

~~~~~~~~~~
Encryption: openpgp4fpr:5f2de5521c63a801ab59ccb603d49de44b29100f
~~~~~~~~~~

### Hiring {#hiring}

The "Hiring" directive is used for linking to the vendor's security-related job positions.
If this directive indicates a web URL, then it MUST begin with "https://"
(as per section 2.7.2 of {{!RFC7230}}).

~~~~~~~~~~
Hiring: https://example.com/jobs.html
~~~~~~~~~~

### Policy {#policy}

This directive allows you to link to where your security policy and/or
disclosure policy is located. This can help security researchers understand
what you are looking for and how to report security vulnerabilities.
If this directive indicates a web URL, then it MUST begin with "https://"
(as per section 2.7.2 of {{!RFC7230}}).

Example:

~~~~~~~~~~
Policy: https://example.com/security-policy.html
~~~~~~~~~~

### Preferred-Languages {#preflang}

This directive can be used to indicate a set of natural languages that
are preferred when submitting security reports. This set MAY list multiple
values, separated by commas. If this directive is included then at least
one value MUST be listed. The values within this set are language tags
(as defined in {{!RFC5646}}). If this directive is absent, security researchers
MAY assume that English is the default language to be used (as per section 4.5
of {{!RFC2277}}).

The order in which they appear MUST NOT be interpreted as an indication of
priority - rather these MUST BE interpreted as all being of equal priority.

This directive MUST NOT appear more than once.

Example (English, Spanish and French):

~~~~~~~~~~
Preferred-Languages: en, es, fr
~~~~~~~~~~

## Example of an unsigned "security.txt" file

~~~~~~~~~~
# Our security address
Contact: mailto:security@example.com

# Our OpenPGP key
Encryption: https://example.com/pgp-key.txt

# Our security policy
Policy: https://example.com/security-policy.html

# Our security acknowledgments page
Acknowledgments: https://example.com/hall-of-fame.html
~~~~~~~~~~

## Example of a signed "security.txt" file

~~~~~~~~~~
----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256

# Canonical URL
Canonical: https://example.com/.well-known/security.txt

# Our security address
Contact: mailto:security@example.com

# Our OpenPGP key
Encryption: https://example.com/pgp-key.txt

# Our security policy
Policy: https://example.com/security-policy.html

# Our security acknowledgments page
Acknowledgments: https://example.com/hall-of-fame.html
-----BEGIN PGP SIGNATURE-----
Version: GnuPG v1

[signature]
-----END PGP SIGNATURE-----
~~~~~~~~~~

# Location of the security.txt file

## Web-based services {#weblocation}

Web-based services SHOULD place the security.txt file under the /.well-known/ path; e.g. https://example.com/.well-known/security.txt
as per {{!RFC5785}}. A security.txt file located under the top-level path SHOULD either redirect (as per section 6.4 of {{!RFC7231}})
to the security.txt file under the /.well-known/ path or be used as a fallback if the ".well-known" path cannot be used.

If retrieval of a "security.txt" file results in a redirect (as per
section 6.4 of {{!RFC7231}}), the implementors MUST NOT follow
redirects that lead to another domain or subdomain
but SHOULD follow redirects within the same domain name
(but not different subdomain on the same domain).

The guidance regarding redirects SHOULD NOT apply to the resource locations that appear within the file.

## Filesystems

File systems SHOULD place the "security.txt" file under the root directory; e.g., "/security.txt", "C:\security.txt".

Example file system:

~~~~~~~~~~
/example-directory-1/
/example-directory-2/
/example-directory-3/
/example-file
/security.txt
~~~~~~~~~~

## Internal hosts

A "security.txt" file SHOULD be placed in the root directory of an internal host.

## Extensibility {#extensibility}

Like many other formats and protocols, this format may need to be extended
over time to fit the ever-changing landscape of the Internet. Therefore,
extensibility is provided via an IANA registry for directives as defined
in {{registry}}. Any directives registered via that process MUST be
considered optional. To encourage extensibility and interoperability,
implementors MUST ignore any fields they do not explicitly support.

In general, implementors SHOULD "be conservative in what you do,
be liberal in what you accept from others" (as per {{?RFC0793}}).

# File Format Description and ABNF Grammar {#abnf}

The expected file format of the security.txt file is plain text (MIME type "text/plain") as defined
in section 4.1.3 of {{!RFC2046}} and is encoded using UTF-8 {{!RFC3629}} in Net-Unicode form {{!RFC5198}}.

The following is an ABNF definition of the security.txt format, using
the conventions defined in {{!RFC5234}}.

~~~~~~~~~~
body                   =  signed / unsigned

signed                 =  sign-header unsigned sign-footer

sign-header            =  <headers and line from section 7 of [RFC4880]>

sign-footer            =  <OpenPGP signature from section 7 of [RFC4880]>

unsigned               =  *line <in any order, but exactly once each: opt-canonical-line-etc, opt-lang-line-etc, contact-line-etc>

line                   =  (field / comment) eol

eol                    =  *WSP [CR] LF

field                  =  ack-field /
                          contact-field /
                          encryption-field /
                          hiring-field /
                          policy-field /
                          ext-field

opt-canonical-line-etc =  [canonical-field eol *line]

opt-lang-line-etc      =  [lang-field eol *line]

contact-line-etc       =  contact-field eol *line

fs                     =  ":"

comment                =  "#" *(WSP / VCHAR / %x80-FFFFF)

ack-field              =  "Acknowledgments" fs SP uri

canonical-field        =  "Canonical" fs SP uri

contact-field          =  "Contact" fs SP uri

lang-tag               =  <Language-Tag from section 2.1 of [RFC5646]>

uri                    =  <URI as per [RFC3986]>

encryption-field       =  "Encryption" fs SP uri

hiring-field           =  "Hiring" fs SP uri

policy-field           =  "Policy" fs SP uri

lang-field             =  "Preferred-Languages" fs SP lang-values

lang-values            =  lang-tag *("," [WSP] lang-tag)

ext-field              =  field-name fs SP unstructured

field-name             =  <imported from section 3.6.8 of [RFC5322]>

unstructured           =  <imported from section 3.2.5 of [RFC5322]>
~~~~~~~~~~

"ext-field" refers to extension fields, which are discussed in {{extensibility}}

# Security Considerations

## Compromised Files and Redirects

An attacker that has compromised a website is able to compromise
the "security.txt" file as well or setup a redirect to their own site.
This can result in security reports not being received by the organization
or sent to the attacker.

To protect against this, organizations SHOULD digitally sign their "security.txt"
files (as per {{signature}}), use the canonical directive to sign the location
of the file (as per {{canonical}}), and regularly monitor the file and
the referenced resources to detect tampering.

Security researchers SHOULD check the "security.txt" file including verifying
the digital signature and checking any available historical records before using the information
contained in the file. If "security.txt" file looks suspicious or compromised,
it SHOULD NOT be used.

To avoid redirect attacks, redirects for these files MUST NOT be followed
if they lead to a different domain (as per {{weblocation}}).

## Incorrect or Stale Information

If information and resources referenced in a "security.txt" file are incorrect
or not kept up to date, this can result in security reports not being received
by the organization or sent to incorrect contacts, thus exposing possible
security issues to third parties.

Organizations SHOULD ensure that information in this file and any referenced
resources such as web pages, email addresses and telephone numbers
are kept current, are accessible, controlled by the organization,
and are kept secure.

## Intentionally Malformed Files, Resources and Reports

It is possible for attackers to generate files that are extraordinarily
large or otherwise malformed in an attempt to discover or exploit weaknesses
in parsing code. Implementors SHOULD make sure that any such code
be robust against large and malformed files. ABNF grammar (as defined in
{{abnf}}) SHOULD be used as a way to verify these files.

Same concerns apply to any other resources referenced within security.txt
files, as well as any security reports received as a result of publishing
this file. Such resources and reports may be hostile, malformed or malicious.

## No Implied Permission for Testing

The presence of a security.txt file can be interpreted by researchers
as providing permission to do security testing against that asset.
This can lead to increased testing against an organization by researchers. On the other hand, a decision not
to publish a security.txt file, can be interpreted by the
organization operating that website to be a way to signal to researchers
that permission to test that particular site or project is denied. This can lead to pushback against
researchers reporting security issues to that organization.

Therefore, implementors MUST NOT assume that presence or absence of
a "security.txt" file grants or denies permission for security testing.
Any such permission MAY be defined in a security or disclosure policy
(as per {{policy}}) or a new directive (as per {{extensibility}}).

## Multi-user Environments

In multi-user / multi-tenant environments, it may possible for a user to take
over the location of the "security.txt" file. Organizations SHOULD reserve
the "security.txt" namespace to ensure no third-party can create a page with
the "security.txt" AND "/.well-known/security.txt" names.

## Protecting Data in Transit

To protect a "security.txt" file from being tampered in transit, implementors MUST use
HTTPS when serving the file itself and any web URLs referenced in it (except as noted in
this specification). Implementors MUST also perform the correct TLS
verification (as per {{!RFC6125}}).

As an additional layer of protection, it is also RECOMMENDED that
organizations digitally sign their "security.txt" file with OpenPGP (as per {{signature}}).
Also, to protect security reports from being tampered with or observed while in transit,
organizations SHOULD specify encryption keys (as per {{encryption}}) unless
HTTPS is being used.

However, the determination of validity of keys being used is out of scope
for this specification. Implementors MUST establish other secure means to
verify these keys.

## Spam and Spurious Reports

Similar to concerns in {{!RFC2142}}, denial of service attacks via spam reports
would become easier once a "security.txt" file is published by
an organization. In addition, there is an increased likelihood of reports
being sent in an automated fashion and/or as result of automated scans without
human triage.

Organizations SHOULD weigh the advantages of publishing this file versus
the possible disadvantages and increased resources required to triage
security reports.

Security researchers SHOULD consult the organization's policy, if available,
before submitting reports in an automated fashion or as resulting from
automated scans.

# IANA Considerations

example.com is used in this document following the uses indicated in
{{?RFC2606}}.

192.0.2.0 and 2001:db8:8:4::2 are used in this document following
the uses indicated in {{?RFC6890}}.

## Well-Known URIs registry

The "Well-Known URIs" registry should be updated with the following additional
values (using the template from {{?RFC5785}}):

URI suffix: security.txt

Change controller: IETF

Specification document(s): this document

## Registry for security.txt Header Fields {#registry}

IANA is requested to create the "security.txt Header Fields" registry in
accordance with {{?RFC8126}}. This registry will contain header fields for
use in security.txt files, defined by this specification.

New registrations or updates MUST be published in accordance with the
"Expert Review" guidelines as described in section 4.5 of
{{?RFC8126}}. Any new field thus registered is considered optional
by this specification unless a new version of this specification is published.

New registrations and updates MUST contain the following information:

   1.  Name of the field being registered or updated
   2.  Short description of the field
   3.  Whether the field can appear more than once
   4.  The document in which the specification of the field is published
   5.  New or updated status, which MUST be one of:
       current:  The field is in current use
       deprecated:  The field is in current use, but its use is discouraged
       historic:  The field is no longer in current use

An update may make a notation on an existing registration indicating
that a registered field is historical or deprecated if appropriate.

The initial registry contains these values:

       Field Name: Acknowledgments
       Description: link to page where security researchers are recognized
       Multiple Appearances: Yes
       Published in: this document
       Status: current

       Field Name: Canonical
       Description: canonical URL for this file
       Multiple Appearances: No
       Published in: this document
       Status: current

       Field Name: Contact
       Description: contact information to use for reporting vulnerabilities
       Multiple Appearances: Yes
       Published in: this document
       Status: current

       Field Name: Encryption
       Description: link to a key to be used for encrypted communication
       Multiple Appearances: Yes
       Published in: this document
       Status: current

       Field Name: Hiring
       Description: link to the vendor's security-related job positions
       Multiple Appearances: Yes
       Published in: this document
       Status: current

       Field Name: Policy
       Description: link to security policy page
       Multiple Appearances: Yes
       Published in: this document
       Status: current

       Field Name: Preferred-Languages
       Description: list of preferred languages for security reports
       Multiple Appearances: No
       Published in: this document
       Status: current

# Contributors

The authors would like to acknowledge the help provided during the
development of this document by Tom Hudson, Jobert Abma,
Gerben Janssen van Doorn, Austin Heap, Stephane Bortzmeyer, Max Smith, Eduardo Vela and Krzysztof Kotowicz.


The authors would also like to acknowledge the feedback provided by multiple members of IETF's SAAG and SEC-DISPATCH lists.

--- back
# Note to Readers

> **Note to the RFC Editor:**  Please remove this section prior
> to publication.

Development of this draft takes place on Github at https://github.com/securitytxt/security-txt

# Document History

> **Note to the RFC Editor:**  Please remove this section prior
> to publication.

## Since draft-foudil-securitytxt-00
- Moved to use IETF's markdown tools for draft updates
- Added table of contents and a fuller list of references
- Moved file to .well-known URI and added IANA registration (#3)
- Added extensibility with an IANA registry for fields (#34)
- Added text explaining relationship to RFC 2142 / security@ email address (#25)
- Scope expanded to include internal hosts, domains, IP addresses and file systems
- Support for digital signatures added (#19)

The full list of changes can be viewed via the IETF document tracker:
https://tools.ietf.org/html/draft-foudil-securitytxt-01

## Since draft-foudil-securitytxt-01
- Added appendix with pointer to Github and document history
- Added external signature file to the well known URI registry (#59)
- Added policy field (#53)
- Added diagram explaining the location of the file on public vs. internal systems
- Added recommendation that external signature files should use HTTPS (#55)
- Added recommendation that organizations should monitor their security.txt files (#14)

The full list of changes can be viewed via the IETF document tracker:
https://tools.ietf.org/html/draft-foudil-securitytxt-02

## Since draft-foudil-securitytxt-02
- Use "mailto" and "tel" (#62)
- Fix typo in the "Example" section (#64)
- Clarified that the root directory is a fallback option (#72)
- Defined content-type for the response (#68)
- Clarify the scope of the security.txt file (#69)
- Cleaning up text based on the NITS tools suggestions (#82)
- Added clarification for newline values
- Clarified the encryption field language, added examples
of DNS-stored encryption keys (#28 and #94)
- Added "Hiring" field

## Since draft-foudil-securitytxt-03
- Added "Hiring" field to the registry section
- Added an encryption example using a PGP fingerprint (#107)
- Added reference to the mailing list (#111)
- Added a section referencing related work (#113)
- Fixes for idnits (#82)
- Changing some references to informative instead of normative
- Adding "Permission" field (#30)
- Fixing remaining ABNF issues (#83)
- Additional editorial changes and edits

## Since draft-foudil-securitytxt-04
- Addressing IETF feedback (#118)
- Case sensitivity clarification (#127)
- Syntax fixes (#133, #135 and #136)
- Removed permission directive (#30)
- Removed signature directive and switched to inline signatures (#93 and #128)
- Adding canonical directive (#100)
- Text and ABNF grammar improvements plus ABNF changes for comments (#123)
- Changed ".security.txt" to "security.txt" to be consistent

## Since draft-foudil-securitytxt-05
- Changing HTTPS to MUST (#55)
- Adding language recommending encryption for email reports (#134)
- Added language handling redirects (#143)
- Expanded security considerations section and fixed typos (#30, #73, #103, #112)

## Since draft-foudil-securitytxt-06
- Fixed ABNF grammar for non-chainable directives (#150)

Full list of changes can be viewed via the IETF document tracker:
https://tools.ietf.org/html/draft-foudil-securitytxt

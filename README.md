# Web Key Directory for "nwie.land"

[Web Key Directories](https://datatracker.ietf.org/doc/draft-koch-openpgp-webkey-service/)
provide an easy way to discover public OpenPGP keys through HTTPS.
## What is OpenPGP?

PGP stands for **P**retty **G**ood **P**rivacy and is, in short, an encryption
software commonly used for encrypting E-mails. OpenPGP is the standard behind
it.

E-mails encrypted with OpenPGP are end-to-end encrypted. This means that no one,
except for the sender and the recipient, is able to read the contents of the
E-mail. This is unlike the transport encryption which is employed by various
free E-mail providers such as Gmail or Yahoo-Mail, where E-mails are encrypted
on their ways between , but not on the servers of the E-mail providers of the
sender and the recipient. However, OpenPGP is agnostic of any E-mail provider
and can be used on services such as Gmail or Yahoo-Mail, too.

Traditionally, in symmetric encryption methods, only one key is used for
encrypting and decrypting information. This creates a problem, because
correspondents need to exchange this key among them secretly before being able
to make use of it. Doing this securely over a long distance, especially over the
internet, is generally impossible.

OpenPGP makes use of asymmetric cryptography: Users of this software create
themselves a pair of two keys: one _public_ key, and one _private_ key (often
also called _secret_ key). As its name suggest, the _private_ key may never be
known to anyone other than its creator. The _public_ key however is safe to be
shared around.

Information encrypted with the _public_ key can only be decrypted with the
corresponding _private_ key. This means anyone in possession of a _public_ key
is able to send secret messages to the holder of the corresponding _private_
key, without ever having to meet in person to exchange the key secretly.

## The Role of the Web Key Directory

When using OpenPGP for sending E-Mails, users commonly face two problems: "What
is the public key of the recipient?" and "Is the public key I have _really_ the
right one?"

Historically, users could upload their public keys to key servers; these are
servers that publicly list public keys and offer searching capabilities for
E-mail addresses. However, not everyone chooses to upload theirs. Also, there
are several different key servers; each with its own list of public keys.  And
since anyone can publish new keys on these servers, and anyone can create key
pairs for arbitrary identities, it is sometimes difficult to trust these.

Some public key owners may choose to publish their key on their private web-site
instead, in a text file, but as a potential sender of a message, I have to find
that one first.

The **Web Key Directory** provides a standardised way for looking for public
OpenPGP keys, and offers a fair amount of trust at the same time. Its function
can be summarised like this:

- Senders check a "well known" URL on the domain of the recipient.
- If a public key is available for that mail address, it can be downloaded via
  HTTPS.
- The downloaded public key can now be used without any further user
  interaction.

When looking up the key, the sender constructs a hash out of the local part
(left of the @-sign) of the recipient's E-mail address and looks for a file named
like that.

Example: for the E-mail address `key-submission@example.org`, the sender will
first hash the local part `key-submission`, resulting in
`bxzcxpxk8h87z1k7bzk86xn5aj47intu`, and then query the URL:
```
https://openpgpkey.example.org/.well-known/openpgpkey/example.org/hu/bxzcxpxk8h87z1k7bzk86xn5aj47intu
```

If the name `openpgpkey.example.org` isn't resolvable, the sender will
additionally query the a second URL:
```
https://example.org/.well-known/openpgpkey/hu/bxzcxpxk8h87z1k7bzk86xn5aj47intu
```

If any of these URLs contains a public OpenPGP key matching the E-Mail address,
the sender may use it for encrypting messages.

Trusting keys that were discovered using a WKD is easier, too, since publishing
a key inside a domain's WKD generally requires the permission to do so by at
least the domain owner. Of course, this trust cannot be ultimate, as the domain
owner and the owner of the E-mail address may be two different people.

## Using the Web Key Directory

Implementations of OpenPGP commonly include a way of querying for a public key
using WKD.

For example [GnuPG](https://gnupg.com/):
```
gpg --locate-key key-submission@example.org
```

E-mail software may automatically fetch public keys and encrypt messages using
WKD. This can be configured for example in
[Thunderbird](https://www.thunderbird.net/).

Some secure email providers like [Posteo](https://posteo.de) and
[Protonmail](https://protonmail.com) also make use of key discovery through WKD.

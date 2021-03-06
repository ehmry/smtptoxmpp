# smtptoxmpp
A small XMPP component to relay emails as XMPP messages.

smtptoxmpp runs as a foreground daemon or with systemd socket activation.

Be warned if an email is sent to an address for which there is no XMPP account, 
it is dropped without error.

## Configuration
smtptoxmpp takes the name of a config file as a single argument; an example follows:

    [xmpp]
    domain = "example.com"
    name = "smtp" # the name of the component would then be smtp.example.com
    secret = "changeme"
    server = "example.com"
    port = 5347
    # smtpregexp and xmppregexp are optional, in this example emails are addressed
    # to the subdomain @xmpp.example.com. The XMPP server only serves @example.com,
    # so inregexp extracts what lies before the ampersat and outregexp appends the
    # extraction with @locahost. The first pair of () corresponds to $1, the second
    # to $2, and so forth.
    smtpregexp = "(.*)@xmpp.example.com"
    xmppregexp = "$1@example.com"

## Serving a sub-domain on the same machine as Postfix
Add this to /etc/postfix/main.cf
> transport_maps = hash:/etc/postfix/transport
Add a line like this to /etc/postfix/transport
> xmpp.example.com       smtp:[localhost]:5225
Then run smtptoxmmp with the -port 5225 option or set systemd to activate smtptoxmpp 
on port 5225.


Licensed under the GNU Affero General Public License.

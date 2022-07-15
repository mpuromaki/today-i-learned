# Connecting to remote OPC-DA server

I have seen multiple automation systems where multiple PCs need
data from single PLC. This issue has been usually fixed by
installing OPC-DA-PLC-Gateway software on all machines
that require the data.

This kind of software is needed on all PLCs that do not
implement server on their own. It means that PC needs gateway
software to translate between OPC-DA and PLCs custom
communication protocol. For example old Codesys PLCs
require Codesys OPC-server and Codesys gateway. Same
thing is on 800xA OPC-DA communication with the AC800M
OPC-server acting as MMS gateway (as far as I know).

The problem with multiple gateways is that each of them
generate load on the PLC. Also the gateway usually
has a list of available variables which are shown
on the OPC-DA server. If you change the PLC software
you need to update these lists. This easily causes 
communication issues between different systems when 
changes are made and not all systems that require 
OPC-files are updated.

## The not so good solution

Sometimes this has been fixed by using DCOM for OPC-DA
communication. While this _should_ work, **here be dragons**.

DCOM is really hard to configure to work correctly.
Also some changes on the latest Microsoft Windows'
make this even harder due to security issues on DCOM.

## The better solution

Just use OPC-tunneling. This way all devices see local
OPC-DA server (the tunnel) which then connects to
the real OPC-server through better means. This could be
whatever, but usually OPC-DA packets are translated to
OPC-UA for transfer between machines. This also means
that it would be easy to connect OPC-UA systems to this
solution in the future.

Also because there is only one gateway, on the main OPC-
DA server, there are only one set of OPC-files to be
updated. Also this usually is done automatically.
Easier to set up. Less issues caused by service visists.

OPC-tunneling software is available from:
- OPC Expert
- Kepware
- Matrikon
- And very likely others as well...

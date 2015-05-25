---
layout: post
title:  "Vyatta Config Sync"
date:   2010-05-11 01:00:00
categories: blog
---

So, finally got around to finishing the configuration sync script for [Vyatta][vyatta-org].

Taking the script I found [here][virtualfoundry-blog] as a starting point, I updated it to work with VC6, and changed the configuration method to support firewall zones, and zone-policies rather than interface based rulebases (new in VC6) and also support syncing of static routes.

In addition, this script uses the vyatta-cfg-cmd-wrapper to build the second script, which is then copied over and remotely executed on the backup node, rather than using ssh -tt – I found that when used in combination with a big ruleset, and a busy network – this method would often result in partially complete or otherwise generally rubbish config on the backup node.

I was going to paste it here, but doesn’t look like code tags in wordpress seem to actually enclose the code as they should do – current version@[github][vyatta-sync-github]

*Disclaimer: This script works on my own Vyatta cluster, but it may not work on yours – no responsibility taken for destroyed configs/servers/lives etc. Manual Configuration is still required on both nodes for the base system, interfaces, VRRP etc, and matching interface names is a requirement for this to work.*

[vyatta-org]: http://vyatta.org/
[virtualfoundry-blog]: http://virtualfoundry.blogspot.com/2009/06/configure-redundant-virtual-firewalls.html
[vyatta-sync-github]: http://github.com/rdark/misc/blob/master/vyatta/vyattaSync.sh

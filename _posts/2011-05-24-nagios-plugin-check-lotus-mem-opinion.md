---
layout: post
title:  "Nagios Plugin - check_lotus_mem_opinion.pl"
date:   2011-05-24 01:00:00
categories: blog
---

Wrote a new nagios plugin the other day. It was bothering me that there are checks out there for lotus/domino memory usage based on bytes used/free, but none that ask poor old domino how it feels about the matter.
At OID `1.3.6.1.4.1.334.72.1.1.9.4.0` it quietly makes it's opinion known but no one has really paid it much attention until now..

Returns either 'Plentiful' (OK), 'Normal' (WARNING) or 'Painful' (CRITICAL).

SNMPv3 is not really supported as I threw the thing together in all of about 5 minutes, but v1 and v2 are - we tend not to use SNMPv3 anyway due to lack of support for 64 bit counters - feel free to fork it on [github][github-check-lotus].

[github-check-lotus]: https://github.com/rdark/nagios/blob/master/scripts/check_lotus_mem_opinion.pl

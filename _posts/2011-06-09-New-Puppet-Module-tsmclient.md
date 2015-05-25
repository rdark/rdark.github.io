---
layout: post
title:  "New Puppet Module - tsmclient"
date:   2011-06-09 01:00:00
categories: blog
---

I wrote a puppet module early last year for managing TSM client installation, but it was far from perfect and still required the manual step of connecting to the server for the first time to store the password.
This evening, a couple of new servers going into production prompted me to re-write the whole thing and make it better.

Our process for adding new nodes to backup now consists of 3 steps.

Add node to backup server:

    reg node $hostname $password userid=none contact="$contact" emailaddress="$emailaddress"

Add node to schedule:

    def assoc $policy_dom $schedule_name $hostname

Add the password to the node manifest under the $tsmpassword variable, and add the tsm::client class to the server.

Done.

Our infrastructure has multiple sites, each with their own TSM server, and those TSM servers listen on different ports.
To automate client configuration, we call on 4 facter modules: tppnetwork, tppsite, tsmserver, and tsmport.

    [rclark@vm242|SITE2 ~]$ facter | egrep '^tpp|^tsm'
    tppnetwork => Net210_LAN
    tppsite => SITE2
    tsmport => 1510
    tsmserver => tsm-site2.internal.example.com

*tppnetwork* is a facter module with that takes the IP address of a node and runs it through a bunch of regexes matching various vlans and networks that we run (including amazon address space). This module is used by lots of other things, not just tsmclient.

*tppsite* is a facter module that takes the output of tppnetwork, and from that determines which site that network belongs to. Also, used by many other things.

*tsmserver* takes the output of tppsite, and from that gives the hostname of the TSM server for that site.

*tsmport* takes the output of tsmserver, and gives the port number that that server is configured to listen on.

So, from these bits of information we have the basic pieces of information that we need to configure our TSM client.

The storage of the password for the first time is done by first detecting the need for a password via an authentication failure - to get this to work without requiring input on STDIN is a bit of a hack - running

    dsmc query session

will force a 'query sess' to run in background mode, then we grep for a line beginning with an authentication failure error code. If this is detected, then running

    dsmc set password $tsmpassword $tsmpassword

will be called, storing the password for the client. This has the added bonus of making mass node password changes easier in future - just change it on the server, and then update the $tsmpassword variable in your pupppet configuration.

`tsm::client` is available on [github][github-tsmclient].

[github-tsmclient]: https://github.com/rdark/puppet-tsmclient

---
layout: post
title:  "SVN::Client and Magical Error Codes"
date:   2012-01-07 01:00:00
categories: blog
---

The perl [svn::client][perl-svnclient] module has a tendency to produce some quite obscure error messages, such as this beauty that cropped up recently, generated when calling the status callback function.

    perl: subversion/libsvn_subr/path.c:114: svn_path_join: Assertion `svn_path_is_canonical(base, pool)

A bit of digging with Data::Dumper revealed that this was caused by a path that was inadvertently postfixed with two sequential forward slashes - which the native C client doesn't seem to mind at all..

[perl-svnclient]: http://search.cpan.org/~mschwern/Alien-SVN-v1.6.12.1/src/subversion/subversion/bindings/swig/perl/native/Client.pm

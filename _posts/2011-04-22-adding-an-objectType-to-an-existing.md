---
layout: post
title:  "Adding an objectType to an Existing LDAP Object using Powershell"
date:   2011-04-22 01:00:00
categories: blog
---

I've recently been spending a bit of time getting to know Powershell a little better, and I have to say I'm quite enamoured with it. While not as flexible or powerful as say, perl, it's definitely a giant leap in the right direction on Microsoft's part.

For managing contacts and contactgroups in nagios, we use several custom objectTypes and attributes held in LDAP (in this case, Active Directory).
For contacts, we have a custom objectClass called `nagiosContact`, which has several mandatory attributes:

* `nagiosContactAlias`
* `nagiosHostNotificationOptions`
* `nagiosHostNotificationPeriod`
* `nagiosHostNotificationCommands`
* `nagiosServiceNotificationOptions`
* `nagiosServiceNotificationPeriod`
* `nagiosServiceNotificationCommands`

This objectClass needs to be added to an existing user object in order for our contact creation scripts to pull the needed information from LDAP.

The usual way of adding attributes to an object in Powershell is via the `Put` method. This works fine when adding existing attributes to an object, but if we want to add an objectClass that includes mandatory attributes it won't work; as you can't add the objectClass without having the mandatory attributes present, but then you can't add the mandatory attributes without first having the objectClass applied to the object. A classic Chicken/Egg scenario.

PutEx is a special method that allows for handling an array of attributes. It takes input in the form: update flag, attribute name, array of values to set/unset. Changes are not written until SetInfo() is called. The chicken and the egg are created together and God saw that it was good.

Update Flags are an integer between 1-4 with the following values

    1 - ADS_PROPERTY_CLEAR -  Remove all value(s) of the attribute.
    2 - ADS_PROPERTY_UPDATE - Replace the current values of the attribute with the ones passed in. This will clear any previously set values.
    3 - ADS_PROPERTY_APPEND - Add the values passed into the set of existing values of the attribute.
    4 - ADS_PROPERTY_DELETE - Delete the values passed in.

This example script shows PutEx in operation, adding our custom objectType and all needed attributes to an existing user object.

{% highlight powershell %}
# modUserNagios.ps1
# Makes an existing user into a nagios contact

# set modify type
[int] $ADS_PROPERTY_CLEAR  = 1
[int] $ADS_PROPERTY_UPDATE = 2
[int] $ADS_PROPERTY_APPEND = 3
[int] $ADS_PROPERTY_DELETE = 4

$objClass = "nagiosContact"
$nagiosContactAlias = "Phoebe Foo"
$nagiosHostNotificationOptions = "d,u,r,s,f"
$nagiosHostNotificationPeriod = "extended_workhours"
$nagiosServiceNotificationOptions = "w,u,c,r"
$nagiosServiceNotificationPeriod = "extended_workhours"
$nagiosHostNotificationCommands = "host-notify-html-email"
$nagiosServiceNotificationCommands = "service-notify-html-email"
$objUser = [adsi] "LDAP://CN=Phoebe Foo,OU=Infrastructure,OU=Users,OU=SiteName,DC=ad,DC=example,DC=com"

$objUser.PutEx($ADS_PROPERTY_APPEND, "objectClass", @("$objClass"))
$objUser.Put("nagiosContactAlias", "$nagiosContactAlias")
$objUser.Put("nagiosHostNotificationOptions", "$nagiosHostNotificationOptions")
$objUser.Put("nagiosHostNotificationPeriod", "$nagiosHostNotificationPeriod")
$objUser.Put("nagiosHostNotificationCommands", "$nagiosHostNotificationCommands")
$objUser.Put("nagiosServiceNotificationOptions", "$nagiosServiceNotificationOptions")
$objUser.Put("nagiosServiceNotificationPeriod", "$nagiosServiceNotificationPeriod")
$objUser.Put("nagiosServiceNotificationCommands", $nagiosServiceNotificationCommands")
$objUser.SetInfo()
{% endhighlight %}


---
title: How to enable assistive devices via the command line on OS X
date: 2016-01-16 13:03:16
tags:
- Tips
- OSX
---

One thing I commonly find myself doing it trying to use AppleScript to run specific tasks on GUI applications, however it is not as simple as just writing the AppleScript application you also need to enable certain applications to allow for this. Below you can see how you can enable this on OS X.

## 10.9+ (Mavericks)

First things first you either need a bundle ID to an application or the path to an executable, the most common of these will be bundle IDs you can get that by using this simple command.

{% codeblock lang:bash line_number:false %}
/usr/libexec/PlistBuddy -c 'Print CFBundleIdentifier' /Applications/<name.app>/Contents/Info.plist
{% endcodeblock %}

{% codeblock lang:bash line_number:false Chess.app bundle ID example %}
$ /usr/libexec/PlistBuddy -c 'Print CFBundleIdentifier' /Applications/Chess.app/Contents/Info.plist
com.apple.Chess
{% endcodeblock %}

Now that we have our bundle ID we can go about enabling it 

{% codeblock lang:bash line_number:false %}
sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db \
"INSERT INTO access VALUES('kTCCServiceAccessibility','<Bundle-ID>',0,1,1,NULL);"
{% endcodeblock %}

{% codeblock lang:bash line_number:false Enable assistive device with Chess.app %}
sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db \
"INSERT INTO access VALUES('kTCCServiceAccessibility','com.apple.Chess',0,1,1,NULL);"
{% endcodeblock %}

If you have a path to a binary remember that the third value given to sqlite3 should be a **1** rather than a **0** to indicate it's a binary. For those who care this field is the *client_type*
{% codeblock lang:bash line_number:false Enable assistive device with /usr/bin/osascript %}
sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db \
"INSERT INTO access VALUES('kTCCServiceAccessibility','/usr/bin/osascript',1,1,1,NULL);"
{% endcodeblock %}

**Update:** For 10.11 apple have added an extra column you simply need to add another NULL value as the last entry like so

{% codeblock lang:bash line_number:false Enable assistive device with Chess.app %}
sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db \
"INSERT INTO access VALUES('kTCCServiceAccessibility','com.apple.Chess',0,1,1,NULL,NULL);"
{% endcodeblock %}

## 10.7/10.8 (Mountain Lion and Lion)

Before mavericks life was simpler (well with regards to assistive device enabling), you simply needed a single command to enable it for all applications.

{% codeblock lang:bash line_number:false %}
touch /private/var/db/.AccessibilityAPIEnabled
{% endcodeblock %}
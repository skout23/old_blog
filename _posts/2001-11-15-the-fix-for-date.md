---
layout: post
title: The Fix for date
category: 
tags: []
---
{% include JB/setup %}

For those that care here is what I did to get the date working.

`$result = mysql_query("SELECT now()");
$date = mysql_result($result,0,"now()");
$query = "insert into journal values ('$date','$title', '$content')";
$result = mysql_db_query ("myportal", $query);
`

Whee now I have a place to jot notes and stuff.

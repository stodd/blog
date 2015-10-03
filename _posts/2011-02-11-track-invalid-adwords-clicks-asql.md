---
layout: post
title:  "Track Invalid Clicks on AdWords With ASQL"
date:   2011-02-11 19:58:00
categories: asql adwords
---
Ever worry that some of your competitors are clicking your ads on your ads multiple times just to get you to go over budget? It isn't unlikely that this is happening to you, especially in a competitive market. Google has introduced some advanced mechanisms to determine if a click is invalid but I am not totally at ease by this. For people that know the IP address of invalid users you can go use IP filtering in the AdWords tools but the problem is that you would have to know the IP address of repeat offenders and if you are using Google Analytics you won't know the IP address of all your visitors.

Well if you have access to you access logs you can use a free tool called ASQL to load up your standard format Apache access logs and search them with SQL statements. You can read more about (and get) ASQL here: [ASQL](http://www.steve.org.uk/Software/asql/)

Once you get ASQL running and your access logs loaded you can run a query like this to get a list of IPs that have clicked your ads multiple times:

    SELECT source, COUNT(source) AS NumOccurences FROM logs WHERE referer LIKE '%adurl%' GROUP BY source HAVING (COUNT(source) > 1)

Great, now we can just enter these IP addresses into Google AdWords and we can not fear having anymore invalid clickers. That's not necessarily true.

Most people have dynamic IP addresses provided by their ISP. If you have a competitor who is clicking your ads from home his IP address may change every once in awhile and will appear to be a new user.

Some ISPs group users into one host. If you block that IP address you could potentially block a lot of users of that ISP in an area.

Google only allows 100 IP addresses into the IP filtering tool. You only can block a certain amount of IP addresses so before doing a 'scorched earth' approach at filtering people you may want to do a little research on each IP address. With a little research you may find that an IP address actually does belong to a competitor office of yours. In that case you definitely would want to block them.
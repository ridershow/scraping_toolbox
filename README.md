# scraping_toolbox

## How Datadome protects website

### Real-time detection: Technical criteria

In the first phase of detection, the DataDome module analyzes the visitor’s technical data. This is a real-time process involving no disk access and no database access.

The analysis relies on massive usage of in-memory cache: in-memory Reverse DNS DataBase, in-memory IP reputation and in-memory counters.

Here are a few of the technical triggers analyzed.

#### UserAgent
With every query, the browser unveils its name: the UserAgent. It’s a purely declarative element, which means it can’t be used for whitelisting. There’s a surprising number of “GoogleBots” crawling through AWS!

On the other hand, using the UserAgent as a blacklisting tool can help block basic bots, amounting to approximately 20% of all bad bot activity. Any web server – Nginx, Varnish or Apache – can define blocking rules based on the UserAgent.

The DataDome algorithm also analyzes UserAgent validity. For example, some bots use UserAgent generators, which sometimes create invalid combinations (like IE11 used on Windows XP). This is a great way to unmask fraudulent activity. Likewise, massive traffic coming from browsers such as IE 5.5 or Netscape is unlikely to be legitimate in 2019.

#### IP Reputation
Many SysAdmins rely on home-made tools or on the famous Linux-based solution Fail2Ban for automated blocking of unwanted IP addresses. However, some companies and ISPs use a single IPs for dozens – if not hundreds – of users, which can lead to the unnecessary blocking of legitimate users.

DataDome has built an in-house IP reputation database, leveraging the billions of hits we analyze each day for all of our customers. This database is constantly updated, so that each and every one of our customers can benefit from the collective experience and knowledge gathered from all the websites and APIs that the DataDome solution protects. 

#### IP owner
The nature of the IP owner (ASN) and range (CIDR blocks) also provides valuable information. Is it an ISP, a host, a company or an organization, and what kind? Where is the IP location, and does it match the normal website audience?

#### Header integrity
Each browser has its own HTTP implementation. This allows us to create a unique fingerprint database to unveil fake browsers that didn’t comply with the perfect fingerprint.

#### JavaScript Challenge 
Our JavaScript Challenge presents every visitor with a JavaScript code that includes different challenges.

Very basic bots probably won’t trigger the JavaScript, which is in itself a detection hint. But we go far beyond this, and are able to use our JavaScript Challege to detect advanced crawling technologies such as PhantomJS and even Chrome Headless.

We are constantly improving our JavaScript Challenges, in order to detect ever more sophisticated crawling bot technologies. 

#### Cookie Challenge 
Based on the same principles as the JavaScript Challenge, the Cookie Challenge sends every visitor a cookie and requests that the client send it back. Legitimate browsers will do this seamlessly, while many bots can’t accept cookies and will fail the test.


### Streaming detection: Statistical criteria

Hits that bypass the real-time technical detection will next be submitted to an analysis of the first seconds of activity, compared to statistical norms.

For the purpose of this analysis, DataDome measures all kinds of metrics in different timeframes. These metrics are then matched against standard patterns corresponding to human behaviors. If a non-standard profile is detected, it is then categorized as a bot.

Here are some of the metrics measured by the DataDome solution:

* Number of hits per IP address: Many bots, especially web scrapers and hacker bots, will crawl thousands of pages in minutes looking for relevant content or safety flaws.
* Sessions per IP address: How many sessions are active for a single IP address in a given timeframe.
* Crawling speed (hot volume per minute): A bot can scrape and store many pages’ worth of content in no time. A unique IP address visiting a large number of pages in little time usually indicates fraudulent activity.
* Recurring hits: bots follow strict and precise rules, in terms of visits, crawl frequency, etc.
* Hits generating 404 errors: bots looking for security flaws generate random URLs, hoping to detect a breach in the architecture of your website. An IP address generating an unusually large amount of 404 pages might be looking for such a flaw.
While it’s rarely possible to make an informed decision based on such patterns alone, they provide essential input to our real-time monitoring algorithms.

### Behavioral detection
The final phase in our detection process is behavioral analysis. At this stage, only the most sophisticated bots have eschewed detection.

This analysis takes a little more time, and is performed asynchronously.

#### Sessions

Using cookies or session reconstitution through machine learning, session analysis provides extremely valuable insights to ensure optimal bot detection. Analyzing sessions allows us to come as close to the user as possible – and find out whether it’s man or machine.

Behavioral analysis at the session level is the most efficient criteria to define blocking patterns, as most legitimate users have a much greater data consumption than bots.

Of course, exceptions exist. Many passionate users can spend countless hours on a single forum thread, or keep track of a product listing for days to follow price evolutions and incoming comments. Used alone, session data are not sufficient.

#### Leveraging Big Data for optimum protection

As bots are becoming increasingly adept at imitating human users, the analysis of behavioural patterns becomes all the more important. To catch even the cleverest bots, we must go a lot further than basic pattern identification.

That’s why the DataDome bot detection solution makes use of Big Data to analyze the visitor’s path on the site.

Once set up, our solution tracks every hit your website receives. It gathers data from each individual user, human or not, and use an in-house blend of AI and machine learning for real-time comparison with our knowledge base of legitimate usage patterns.

### Data Access Landing Page
If our identification is still not conclusive after all three stages of detection, we present the visitor with a Data Access Page.

This page includes a CAPTCHA, an important tool to measure false positives and provide a feedback loop for our algorithm. Our machine learning system continuously adjusts the rules based on the number and characteristics of false positives.

What about CAPTCHA-solving farms and clever bot algorithms that have learnt how to solve them? DataDome’s answer is to continue to track and monitor users who pass the CAPTCHA, in order to analyze their usage patterns and find out whether they’re human or not. This can lead to CAPTCHA invalidation, when we observe fraudulent usage of session authorization.

----

## inspect outgoing HTTP requests of a single application

Obtain root in a terminal with

`sudo -i`

capture the RAW packets

`sudo tcpdump -i any -w /tmp/http.log &`

capture all the raw packets, on all ports, on all interfaces and write them to a file, /tmp/http.log.

Run your application. It obviously helps if you do not run any other applications that use HTTP (web browsers).

Kill tcpdump

`killall tcpdump`
To read the log, use the -A flag and pipe the output toless:

`tcpdump -A -r /tmp/http.log | less`

The -A flag prints out the "payload" or ASCII text in the packets. This will send the output to less, you can page up and down. To exit less, type Q

Some helpful flags (options):

```-i Specify an interface
-i eth0

tcp port xx
tcp port 80

dst 1.2.3.4
specify a destination ip address```

More informations about TCPDUMP here --> https://danielmiessler.com/study/tcpdump/

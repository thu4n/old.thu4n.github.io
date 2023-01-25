---
title: My first time participating in Net Challenge 2022
date: 2022-12-19 20:55:00 +0700
author: thu4n
categories: [Blogging]
tags: [net challenge, competitions]
---
## What is Net Challenge?
![img](/assets/img/other/netChallenge1.JPG){: width="700" height="400" }
_Our team - Xelvis64_
{: .nolineno}
In summary, Net Challenge is a contest about network configuration and troubleshooting hosted by the faculty of Computer Network and Communication in UIT. For more details about it, you can read [here](https://www.uit.edu.vn/net-challenge-2022-chinh-thuc-mo-dang-ky).
Me being a Network student had to participate just for the knowledge alone, *not even for the prize money*. So, my two roommates and one 3rd year student formed a team and registered under the name **Xelvis64**.
And so, the story begins.
## Qualifying round
The first round of the contest was a multiple choice test with around **30 questions** (I'm not sure about this). All the questions were about computer networking and *you were allowed to Google*. We had **60 minutes** to complete and each member of the team was **forbidden of communicating** with each other. It did sound rather hard at first, but with the help of Google, things became a little bit easier.
Only the **top 10 teams** with the **highest scores** would be qualified for the next round. Guess what, our score was terrible but somehow, we ranked **10th** and thus, were qualified!
## Semi-final round
No more trivia questions, it's time for some actual configuration to be done. Each team was handed a set of papers and on them were the challenge for this round. The challenge consisted of **7** tasks and the total time was **120** minutes. We had to do it on the simulation website of contest. The tasks were:

1. VLAN configuration
2. Routing
3. DHCP
4. EtherChannel
5. VLAN Trunking Protocol
6. Spanning Tree Protocol

I was assigned to do task 3 - DHCP which was worth 10 points total. I knew the concept of DHCP as well as the principles behind implementing it but actually setting it up was another story. Since this was probaly my first time to configure DHCP, I followed Google instructions and the result was horrendous. I almost ruined other guys work by doing it wrong, luckily, I didn't save so nothing harmful was done. I managed to get **2 point** for this DHCP configuration task (pretty bad, I know).
One guy in my team got **0 point** for doing VLAN trunking and another guy got like **5 points**. However, the senior in my team was on fire that day and he single-handedly scored **35 points** for the team (20 for VLAN and 15 for Routing). Only the **top 4 teams** would be qualified for the next round and somehow, with our considerably low score of **42**, we placed **4th**.
## Final round
The semi-final was just a simulation website, as for the final, we would be doing the configuration on *actual* physical devices. This time, we had **300** minutes for the completion of the challenge. The round was split into 2 parts.
The first one was **Network Discovery** where we would have to draw a network topology and fill in the informations of the devices used for said topology based on the given IP address table. We did fairly well in this part since our senior guy knew this like the back of his hand.
Onto the second part, the configuration tasks as well as troubleshooting. The tasks were:

1. VLAN configuration
2. VLAN Trunking Protocol
3. OSPF and EIGRP
4. Static routing
5. Dual WAN Configuration
6. Network Address Translation (NAT)
7. Hot Standby Router Protocol (HRSP)
8. Network Automation
9. Troubleshooting

Without a doubt, this round was way harder than the previous one. With a bit of help, I did get through task number **3** and **4** and no messing up this time. Number **1** and **2** were done by my two other teammates. The senior guided us all throughout the tasks, it couldn't be done without him. There was one particular large problem with the NAT configuration. The routers were not real routers, they were Layer 3 Switches and thus, did not support NAT. There were probaly other ways to go around this but the whole team coudn't figure it out. Since NAT implementation wasn't possible, 7-9 could not be done as well for having NAT was required. And, time was up.
I admit it, we did not perform welll at all but in the end, our score was enough for a **third** place. For a first-time participant, I was satisfied with this result. I learned so much more about the networking field and my overall experience with the contest was enjoyable!
![img](/assets/img/other/netChallenge2.JPG){: width="700" height="400" }
_Me and my team (with the judge on the right most)_
{: .nolineno}
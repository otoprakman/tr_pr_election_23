# Data Analysis for 2023 Turkish Presidential Elections

## Introduction

Presidential elections were held in Turkey in May 2023, alongside parliamentary elections, to elect a president for a term of five years. It marks the first time a Turkish presidential election has gone to a run-off.

In the first round, Erdogan and Ogan outperformed expectations to receive 49.5% and 5.2% of the vote respectively. Meanwhile, Kilicdaroglu won 44.9%, while Muharrem Ince (who remained on the ballot despite withdrawing) received 0.4%. Since Erdogan's vote share was 0.5% short of winning outright, he and Kilicdaroglu contested a run-off vote on 28 May. Erdogan was re-elected in the second round with 52.18% of the vote to 47.82% for Kilicdaroglu. 

**(Please consider that this blog post is not written by a Subject-matter expert. All shared information is based on the resources mentioned at Resources section and author’s personal experience. Main purpose of this post is to share personal thoughts regarding the critical elections)**

Election results were disappointment for Kilicdaroglu supporters and Erdogan has all the advantage going into the second round after winning 49.5 percent of votes in the first round. 5 percent of the vote was garnered by Ogan who is a nationalist and at the runoff it was crucial for both leaders Erdogan and especially for Kilicdaroglu. However, there was another tackle for Kilicdaroglu, Kurdish votes who supported Kilicdaroglu in the first round. Kilicdaroglu had to seek the balance of nationalist statements and the level of his tone. He chose nationalist one. How did it affect the second round, let's have a look.

I'll provide some questions and answers by providing some graphs and analysis.

![2023_Turkish_presidential_election_map_first_round](https://github.com/otoprakman/tr_pr_election_23/assets/53580699/43091708-e709-4745-9549-e9cc4a64b2f8)
![2023_Turkish_presidential_election_map_second_round](https://github.com/otoprakman/tr_pr_election_23/assets/53580699/7e51ec94-c526-492f-8a4e-2b4a0d4ab552)
*Election results on map [source: Wikipedia]*

## Testing the call of KK to his supporters to vote.

**Question-1:** *Is there a significant turnout rate difference in cities where KK get the most of the vote?*

![rte_vs_kk](https://github.com/otoprakman/tr_pr_election_23/assets/53580699/ffefda28-db36-415e-8b7e-39ffa4e774cc)

There are 30 cities where KK was leading in the first round and 51 cities where RTE was leading. Here in the plot we compare percentage of turnout changes where candidates were leading in the first round. According to the plot it's clear that turnout rate negatively changed overall, however, cities at where KK was leading in the first round didn't respond well by the call for voting.

**Question-2:** *How candidates votes changed in percentage?*

![count_diff](https://github.com/otoprakman/tr_pr_election_23/assets/53580699/9b2021df-000a-4b5a-8da7-ea2428ae339d)

RTE increased his votes all cities except Batman (-%1.19). RTE increased his votes in Igdir (+%20). KK increased his votes most in Kayseri and decreased most in Agri

**Question-3:** *How KK nationalist speeches after first round responded by voters, was he able to attract Sinan Ogan(SO) voters?*

![so_diff](https://github.com/otoprakman/tr_pr_election_23/assets/53580699/a2a9b793-6d07-4df7-84a2-66ebd6384e4f)

Pearson Correlation Coefficient between KK Diff and SO pct (without Igdir): 0.8306001040759013

High correlation indicates KK was successful at getting SO's votes better than RTE in overall.

**Closer Look At Nationalist Cities**

Let's check parliament election results first. We label cities where total nationalist party votes are above %25.

![nat_so_diff](https://github.com/otoprakman/tr_pr_election_23/assets/53580699/0786c31a-fa18-4103-b25d-aeb7e6fdb11b)

Pearson Correlation Coefficient between KK Diff and SO pct: 0.407891970708202

KK increased his vote in all nationalist cities. It seems nationalist speech was helpful gaining attention of SO votes

**How about Kurdish votes?**

We label cities where YSP(Yesiller Sol Parti) votes are above %25.

![ysp_so_diff](https://github.com/otoprakman/tr_pr_election_23/assets/53580699/7ec222ec-ac86-4358-9fdc-40884ad8cc2b)

Pearson Correlation Coeffcient between KK Diff and SO pct: 0.0073420806258775295

RTE performed better than KK in all YSP-leaning cities regardless of SO vote rates. Overall it indicates voters who preferred SO in first round over KK voted KK in second round. However 

## Conclusion

In this blog post, we introduced one of the common problems in payment analytic and proposed two different approaches. As a future research direction, number of variables can be introduced to the models like, type of merchant, customer profiles, country of the customer, type of the prouct etc. Another methods like multi-armed bandit or more sophisticated tree based algorithms can be used. Sampling bias is a critical problem and should be considered before modelling.

## Resources
1) https://en.wikipedia.org/wiki/2023_Turkish_presidential_election
2) https://www.cfr.org/in-brief/heres-how-read-turkeys-election-results-so-far


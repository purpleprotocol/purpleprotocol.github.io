---
layout: page
title: Voting
wikiPageName: Voting
menu: wiki
---

In order to provide safety against malicious sequencers, a multi-stage voting algorithm is executed by each participating sequencer on their locally stored causal graph of events.

### Candidate sets
A candidate set is formed out of any event which has at least an edge pointing to it but which does not have any edge pointing from it to another event. If two events have an equal timestamp, they will belong to different candidate sets. If there are no concurrent events, a candidate set will contain at most one event. If there are concurrent events which meet the criteria, they will also be contained by the candidate sets.

### Voting criteria
An event votes on a candidate set if it meets the eligibility criteria which is that it directly follows one or more events from the candidate set and that it is followed by at least `(n + 1) / 2` other events from distinct sequencers where `n` is the number of currently active sequencers.

### Advancement criteria
A candidate set is advanced if it meets the proposal requirement which is that there are at least `n + 1` proposals for it. An event proposes for a candidate set if it follows at least  `(n + 1) / 2` events from distinct sequencers which follow an event which has voted on the candidate set.

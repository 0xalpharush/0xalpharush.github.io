---
layout: post
title: Reclaiming the MEV Conversation 
summary: It's time to rethink how we approach solving and discussing MEV.
image: /images/reclaiming-the-mev-conversation.jpg
---
It's time to rethink how we approach solving and discussing MEV.

### The Problem

As cryptocurrencies gain traction and the opportunity to make money beckons, the incentives in these systems undergo stress testing. While some rug pulls, or scams, are obvious, many other projects are simply poorly designed but well marketed. One particular example of value being captured is "miner extractable value (MEV) solutions" that purport to protect users through "clever" economic designs said to incentivize good behavior. Unfortunately, knee-jerk reactions to "unfair" practices like payment-for-order-flow and front-running do little to solve the underlying issues. Instead, the problem is swept aside in favor of rushed regulation and token sales of protocols with empty promises. Let's examine the belly of the beast.

### What is MEV?

Markets can "leak" value as a consequence of their design irrespective of whether that market is a blockchain or a stock exchange. Simply put, there's pennies lying in the road for anyone to grab. In stock exchanges, traders with expensive price feeds and low-latency connections can ["snipe"](https://www.youtube.com/watch?v=yvNJID_7iHI) stocks that no longer reflect the market price and flip them within sub-second time windows; hence, the designation "high frequency trading." In blockchains, sporadic opportunities like liquidations and arbitrage result in a similar race to be "first." This plays out in a variety of ways, but, in general, block producers capture most of the value as they either extract value themselves or accept bribes from users for the service of transaction ordering.

Any system that propagates transactions continually but has limited space (time or storage) will, at times, contain transaction slots that are inherently more valuable than others as there are pennies to pick up, and people are willing to share part of the profit. To address this issue, stock exchanges could switch to batched auctions and mitigate some "MEV" as proposed by Eric Budish. In the instance of blockchains, specifically smart contract platforms, the problem is slightly different since many protocols built on top of the layer-one chains rely on incentives to get users to "poke" the contract and perform certain updates. In fact, the very existence of decentralized exchanges (DEX) would not be possible without profit-motivated arbitrageurs who "update" the market price. 

In pursuit of fabled MEV, "searchers" rapidly fine tuned their extraction and adopted sophisticated methods such as optimizing their hardware, brushing up on convex optimization, writing low-level assembly, and, ultimately, using private relays (primarily, Flashbots). 

### Where are We Headed?

As the tale of long lost treasure spread, a wide variety of actors have entered the space. Many researchers laborered and published proposals with ways to [smooth MEV](https://ethresear.ch/t/committee-driven-mev-smoothing/10408) and [separate proposers from block-builders](https://ethresear.ch/t/proposer-block-builder-separation-friendly-fee-market-designs/9725). Alternative suggestions include various uses of encryption similar to how Osmosis DEX plans to protect trades with threshold encryption. While there has yet to be a catch-all solution, many smart people are working to address this issue. 

For now, the main thing to do is avoid bad<sup>[1](#footnote1)</sup> actors. Discussions around MEV need not devolve into hyperbolic tantrums or pump-my-bags-shilling. Rather, we need a reinvigorated focus on improving the market designs of layer-one blockchains and the smart contract applications on top of them. 

Projects that create permissioned systems and attempt to use their own token as an incentive are fundamentally flawed and not pushing the ecosystem forward. At best, “tokenizing MEV”, puts a band-aid on the problem. At worst, wealth is redistributed from the “average” token holder, enriching others and offering little protection. This mechanism design problem will not be solved with shoddy incentives because the prospect of MEV, by nature, creates a reason to violate expectations and trust for profit. Advertising a “solution” that merely adds another component to block producers’ optimization problem may temporarily patch issues, but it’s not prudent to accept such a solution as permanently removing the economic motivations of block producers.

### Further Reading

I recommended checking out the additional resources I put together [here](https://github.com/0xalpharush/awesome-MEV-resources) and joining the action on [Twitter](https://twitter.com/0xalpharush) as this post assumes a lot of prior context.

<sup><a name="footnote1">1</a>: Not a value judgement.<sup>


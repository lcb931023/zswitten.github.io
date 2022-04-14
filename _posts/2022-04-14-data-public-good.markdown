---
layout: post
title:  "Data as an Undersupplied Public Good"
date:   2022-04-14 00:40:04 -0700
---

*Summary*
The most valuable data in the world is public. Stuff like Wikipedia and Github. So it's underfunded, like all public goods. And it's even more underfunded because its value is hard to measure. Finding cheap ways to measure the incremental effects of a dataset would let us reward dataset creators and unlock massive value.

*Data as a public good // The best things in life are free*

What makes data valuable? Data is valuable because it makes an ML model do better on some task. For private data, this can be estimated by companys' willingness to pay. For instance, your browsing/shopping habits are [allegedly](https://www.invisibly.com/learn-blog/how-much-is-data-worth) worth about $65/year/person because they improve the performance of a what-ads-are-worth-showing-to-you model.

But the most valuable data in the world, the data that powers the biggest, best, models, is *completely public*.

- [OpenAI Codex](https://openai.com/blog/openai-codex/), which powers [Copilot](https://copilot.github.com/), is trained on publicly available source code from Github. Codex can already [write video games](https://andrewmayneblog.wordpress.com/2022/03/17/building-games-and-apps-entirely-through-natural-language-using-openais-davinci-code-model/) essentially from scratch and already has many devotees who say it makes their work faster and better.
- [Alphafold](https://www.deepmind.com/blog/alphafold-a-solution-to-a-50-year-old-grand-challenge-in-biology), which can replicate an entire PhD's worth of work finding the structure of a protein with one inference call, is trained on publicly available data from the [Protein Data Bank](https://www.rcsb.org/).
- [GPT-3](), OpenAI's language model that blew everyone away in 2020 with its ability to do style transfer, open-ended dialogues, and FILLMEIN, was [trained](https://en.wikipedia.org/wiki/GPT-3#Training_and_capabilities) on Common Crawl, Reddit, Wikipedia, and a bunch of books. Again, public domain.
- DALL-E 2, the new AI art generating hotness at time of this writing, is [trained](https://arxiv.org/pdf/2102.12092.pdf) (see Appendix C; this is for DALL-E 1 but in the [paper](https://cdn.openai.com/papers/dall-e-2.pdf) for 2 they say they used the same data) on a combination of [Conceptual Captions](https://ai.google.com/research/ConceptualCaptions/), which is made of up pairs of images and their alt-text, and images from Wikipedia along with their captions.

GPT-3 and Copilot are already earning income, gating access to their models through a paid API. AlphaFold and DALL-E haven't directly made anyone money yet as far as I know, but it's a mortal lock that AlphaFold or its successor will speed up the process of finding boutique molecules to cure diseases and help insert DNA into our genome, and artists are shook about DALL-E hitting their pocketbook which is a good sign for art consumers.

Wikipedia isn't going to see a penny of that, though, unless their yearly donation pledge headers happen to catch Sam Altman in a generous mood. But of course it's not only Wikipedia the organization who is providing the value here. A lot (most?) of the value of Wikipedia qua dataset was created by the people who wrote the entries.

Because of this lack of ownership and attribution, economic theory tells us that large public datasets are *massively underproduced*. To produce the socially optimal amount of data, every time you write a Wikipedia entry or write an answer on Stack Overflow, you should earn royalties equal to the expected improvement of all the models trained on what you wrote, multiplied by the value of that improvement to all the users of those models.

*Measuring the impact of a dataset // Data Nutrition*

How should we think about the value of an individual Wikipedia entry, or of the Wikipedia corpus as a whole?

Machine learning does have a tool for measuring the impact of a dataset: [ablation](https://en.wikipedia.org/wiki/Ablation_(artificial_intelligence). AKA taking out the dataset, training the model, and then comparing performance. As Wikipedia helpfully reminds us, there's an analogy with biological studies where they take out some portion of a fruit fly's brain and see if it still flies.

The catch is that these models cost millions of dollars to train. And as long as the [scaling laws](https://www.gwern.net/Scaling-hypothesis) keep holding, they're only going to get more expensive over time. It's tough enough to make the argument for OpenAI to do full retrains of GPT-3-minus-Wikipedia, GPT-3-minus-Reddit, ..., and of course GPT-3-minus-wikieditorxyz's-contributions for all wiki editors would be prohibitively expensive.

There is another problem as well: even if we could perfectly measure the contribution of each Wikipedia entry to model performance on the training set, the connection between training performance and real-world value seems to be highly nonlinear and hard to predict. We don't know the magical threshold for accuracy in predicting the next token in a sequence that will enable Copilot to write code fluently and to-spec in a way that makes software engineers as shook as artists.

I'd argue that the uncertainty of that translation can be ignored as a form of "ML luck" akin to "moral luck" -- if the magical perplexity threshold is 50, your data brings it halfway from 60 to 50, and then later mine brings it the other half, and causes a phase change, we both deserve the same reward.

Let's leave that complication aside and pretend that test set impact = real world impact. One way to approximate the contribution of some subset of (e.g.) Wikipedia on GPT's performance is to do ablations with progressively less tiny versions of GPT. Hopefully there would be decent correlation between contribution to 1e9-scale GPT, 1e8-scale GPT, and 1e7-scale GPT which would give us confidence that the same would hold for full-scale GPT.

Another approach to measuring the contribution of small portions of a dataset would be to model it directly. You could amass a dataset where the samples are (dataset, supplemental data) pairs, and the labels are the improvement of a menagerie of models when trained on dataset + supplemental dataset compared to without the supplemental dataset. Or, rather than (dataset, supplemental pair), you could use (parametrized model, supplemental dataset), and the label would be the improvement in performance for that specific model.

Food you buy in a store usually comes with a label for Nutrition Facts: how many calories, how much sodium, protein, niacin it has. In this world, datasets would have labels showing how much they contribute to model performance on various tasks. We would talk about information-rich datasets the way we talk about spinach being high in fiber.

*Solving the data shortage*

What would the world look like if datasets had reliable nutrition facts, and model providers like OpenAI paid out royalties to data providers each time they served an inference proportional to the nutrition of the data?

1. Data labeling, along with scraping and other creative forms of data acquisition, become more lucrative and high-status.
2. The pace of development for large data-constrained ML models is accelerated. Profits for large model providers go up as the pie grows even though they pay out much of their new income to data providers.
3. Compute-constrained models benefit from the nutrition fact labels as it's easier to know what to train them on.

And some other consequences that make the picture less rosy:

4. Whatever method we choose for measuring data nutrition will be heavily analyzed and gamed. An arms race will develop as it's only a matter of time before each imperfect metric gets [Goodharted](https://en.wikipedia.org/wiki/Goodhart%27s_law) to death. Some model providers may keep their valuation methods secret to prevent this, publishing only their total payouts or not even that.

How might we bring this world about?

1. Developing powerful, cheap methods for measuring data nutrition would on its own provide model providers with strong incentive to purchase more data on purely selfish grounds. They already do this (Facebook spends tens of millions each year paying people to label data; Google has us all doing Captchas all the time; OpenAI pays Turkers to help with their reinforcement learning projects). This would be more incentive for them to do more of what they're already going.

2. The legal system could be brought to bear, mandating the payment of data royalties. This would have a stronger stimulative effect on data production but would also 

*Notes // Arguments from Authority*

- [Chinchilla paper](https://arxiv.org/pdf/2203.15556.pdf), showing that at current margins models benefit more from extra data than extra size
- Andrew Ng: "So for many practical applications, it’s now more productive to hold the neural network architecture fixed, and instead find ways to improve the data... This is the time to take the things that some individuals have been doing intuitively and make it a systematic engineering discipline." [Link](https://spectrum.ieee.org/andrew-ng-data-centric-ai)
- Andrej Karpathy [blogpost](https://karpathy.github.io/2022/03/14/lecun1989/) on 33 years of progress in ML. Predictions for 2055: "2055 neural nets are basically the same as 2022 neural nets on the macro level, except bigger. Our datasets and models today look like a joke. Both are somewhere around 10,000,000X larger." Models can get bigger off Moore's law but where does the 10,000,000x increase in data come from?
- I definitely didn't invent the data as a public good formulation. In fact I happened to read Anthropic's [latest paper](https://arxiv.org/pdf/2204.05862.pdf) using the phrase while I was editing this essay. Not sure if other people have connected this to public goods being undersupplied. It's kind of hard to Google because of people talking about data FOR public good.
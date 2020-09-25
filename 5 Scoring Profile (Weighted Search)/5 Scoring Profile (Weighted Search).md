### Scoring Profile (Weighted Search)

#### a. Concept

*Scoring* computes a search score for each item in a rank ordered result set. Every item in a search result set is assigned a search score, then ranked highest to lowest.

Azure Cognitive Search uses default scoring to compute an initial score, but you can customize the calculation through a *scoring profile*. Scoring profiles give you greater control over the ranking of items in search results. For example, you might want to boost items based on their revenue potential, promote newer items, or perhaps boost items that have been in inventory too long.

**For more details:** https://docs.microsoft.com/en-us/azure/search/index-add-scoring-profiles

#### b. Create your scoring for index

In azure portal, select "Scoring profiles" 

![image-20200924114622933](https://i.loli.net/2020/09/24/8RciLKgtOmBqau6.png)

Then set weight number for each fields.

![image-20200924114940700](https://i.loli.net/2020/09/24/lWcF9AN8uGboIfk.png)

The final step is check your index definition, make sure these information are listed.

![image-20200924115136090](https://i.loli.net/2020/09/25/pM76tAa8mnVIdlW.png)
# Mode: ofertas - Multi-Offer Comparison

Use a 10-dimension weighted scoring matrix:

| Dimension | Weight | 1-5 Criteria |
|-----------|--------|--------------|
| North Star alignment | 25% | 5=exact target role, 1=unrelated |
| CV match | 15% | 5=90%+ match, 1=<40% match |
| Level | 15% | 1=staff+, 2=senior, 4=mid-senior, 5=mid, 3=junior |
| Estimated comp | 10% | 5=top quartile, 1=below market |
| Growth trajectory | 10% | 5=clear path to next level, 1=dead end |
| Remote quality | 5% | 5=fully remote async, 1=onsite only |
| Company reputation | 5% | 5=top employer, 1=red flags |
| Tech stack modernity | 5% | 5=cutting-edge AI/ML, 1=legacy |
| Time to offer | 5% | 5=fast process, 1=6+ months |
| Cultural signals | 5% | 5=builder culture, 1=bureaucratic |

For each offer, score every dimension and compute the weighted total.

Return:
- ranked comparison
- final recommendation
- time-to-offer considerations

If the offers are not already in context, ask the user for them. They can be JD text, URLs, or references to already evaluated offers.

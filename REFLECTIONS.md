# Reflections

1. Why a silent `robots.txt` failure is worse than a loud one (CM3).

A silent `robots.txt` failure is worse because it looks like a successful scrape while quietly violating a site rule. A loud failure is easier to catch, fix, and explain; it keeps the team honest about provenance and legal risk. In this project, that mattered because the data source was not a clean public API but a scraped job sample, so the trust boundary had to be explicit.

2. France Travail data vs. this jobspy-derived sample — what's the real difference in risk, and does public/private repo status change your answer?

France Travail data is more official and usually more structured, but it is still a real-world production dataset with access and policy constraints. The jobspy sample is more flexible and easier to use locally, but it is also more likely to be noisy, incomplete, and harder to reproduce exactly because it depends on third-party scraping and site structure. Public vs. private repository status matters for the dashboard and reproducibility, but it does not remove the underlying data-quality risk; it mainly changes how visible and shareable the result is.

3. Why free-text-to-skills is a good LLM task and application-tracking is not (CM5).

Free-text-to-skills is a good LLM task because the job is to interpret messy, human-written language and normalize it into a consistent schema. Application tracking is much riskier because the model is being asked to make decisions with real personal and employment consequences, where mistakes can affect people directly. In other words, extraction is a classification and normalization problem, while tracking is a decision-support problem with much higher stakes.

4. Your predicted LLM formatting inconsistency, and whether you found a real instance of it in your own `extracted.json`.

I expected the model to vary formatting across postings, especially in capitalization, punctuation, and how it treated multi-word skills such as “Computer vision,” “Generative AI,” or “CI/CD.” I did find that kind of inconsistency in the generated JSON: the same concept appeared in different styles and casing, which is exactly the kind of noise that makes a KPI definition sensitive to the extraction pipeline. That is why a robust KPI should not rely on brittle string matching alone.

5. Why the critique pass counts as human-in-the-loop, and why it runs on `extracted.json` and not `kpi.json` (CM5).

The critique pass counts as human-in-the-loop because a human checks whether the extracted skills are plausible, consistent, and usable before the aggregate metric is computed. It belongs on `extracted.json` because that is where the raw structured output lives; once the extraction has been reviewed, KPI generation is just an aggregation step. This separation matters because a flawed extraction can produce a clean-looking but misleading KPI.

6. Your own chosen `TARGET_SKILL`, argued before you saw its number, and which of the two KPI values (default vs. yours) you'd actually trust for a real decision (CM2).

I chose Python as my target skill because it is common in technical roles and easy to interpret in a hiring market context. Before seeing the number, I expected Python to be relevant but not dominant in every job family, so I was not surprised that the baseline KPI was only about 16.7%. For a real decision, I would trust the more explicit and domain-specific KPI only when the decision is about software roles; otherwise, a broader market signal like cloud usage is more useful.

7. What a viewer could and couldn't reconstruct from `kpi.json` alone, and what specifically kept it that way (CM3/CM6).

From `kpi.json` alone, a viewer can see the aggregate KPI value, the match count, the date, and the top skills. They cannot reconstruct the exact postings, the raw extracted skill list per posting, or the reasoning behind the extraction, because those details were intentionally kept out of the published file. That is the right design for a dashboard: it exposes the decision signal without leaking the sensitive per-posting detail that would make the project fragile or less reproducible.

8. Who this dashboard is really for, and whether its current framing matches that (CM1).

This dashboard is really for a decision-maker who wants a quick, readable signal about market demand, not for a person who needs a full legal or hiring audit. The current framing is close to that goal because the page is simple and aggregated, but it still needs careful wording so it is not mistaken for a definitive labor-market truth. In other words, the audience is practical and time-limited, not a technical committee doing a deep evidence review.

9. Your second, self-designed KPI and which of the two you'd trust more.

My second KPI was the fraction of postings mentioning any major cloud provider, such as AWS, Azure, or GCP. In this dataset, that metric came out higher than the Python share, which makes sense because cloud skills appear across more job families than a single programming language. I would trust this one more for a real staffing or curriculum decision because it captures broader market demand and is less likely to be distorted by a single skill label or one technology niche.

---



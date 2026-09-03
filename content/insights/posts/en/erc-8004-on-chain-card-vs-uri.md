# On-chain identity vs off-chain information

When you run into an agent, where do you actually find its identity?

Eight months after mainnet, we wanted a narrower question than “how many agents exist.” Where does a readable agent card live — on the registry, in the URI file, or nowhere?

This is what we used:

- **Snapshot date:** 24 August 2026.
- **Universe:** Identity on mainnet only, on eight EVM chains: Ethereum L1, Base, Polygon, BNB Chain, Arbitrum, Celo, Gnosis, X Layer.
- **Stock:** **441,794** agents.
- **“Usable card”:** a non-empty name **and** a description of at least 10 characters. A name alone is a label, not a card.

## On-chain information

| On-chain field | Agents | % of 441,794 |
| --- | ---: | ---: |
| Has a chain profile | 441,794 | 100% |
| Non-empty name | 441,794 | 100% |
| Description ≥ 10 characters (**usable card**) | **205,564** | **46.5%** |
| Name without a usable description | 236,230 | 53.5% |
| Empty description | 234,253 | 53.0% |
| Description of 1–9 characters | 1,977 | 0.4% |
| Non-empty image | 172,868 | 39.1% |
| Name + description + image | 171,269 | 38.8% |
| Services, skills, or tags on the chain profile | **0** | 0% |

## Off-chain information (URI file)

| Off-chain URI | Agents | % |
| --- | ---: | ---: |
| Declares an `agentURI` | 391,235 | 88.6% |
| Parsed URI profile | **371,473** | **84.1%** |
| Usable URI profile (name + description ≥ 10) | **352,017** | **79.7%** |
| URI profile with an image | 306,056 | 69.3% |
| URI profile with **services** | **181,132** | **41.0%** |
| Document stored behind the pointer | 387,211 | 87.6% |

## Off-chain information (feedback)

| Feedback-origin profile | Agents |
| --- | ---: |
| Any feedback-origin profile | **9,463 (2.1%)** |
| Usable name + description | 5,456 (1.2%) |

## Three origins, one gap

| Bucket | Agents | % |
| --- | ---: | ---: |
| Usable on-chain card | 205,564 | 46.5% |
| URI profile (any fill) | 371,473 | 84.1% |
| Feedback profile (any fill) | 9,463 | 2.1% |
| Usable chain **and** URI profile | 204,323 | 46.2% |
| All three origins | 3,983 | 0.9% |
| Usable chain only (no URI, no feedback profile) | 1,240 | 0.3% |
| No usable chain, URI profile only | 163,910 | 37.1% |
| No usable chain, but URI or feedback profile | **169,389** | **38.3%** |
| No usable chain, no URI profile, no feedback profile | **66,841** | **15.1%** |

## Same standard, different cards

| Chain | Agents | % of stock | On-chain usable | URI profile | URI usable | Feedback profile |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| BNB Chain | 296,616 | 67.1 | **52.6%** | **94.7%** | 93.8% | 0.2% |
| Base | 67,701 | 15.3 | 42.1% | 64.9% | 57.6% | **8.9%** |
| Ethereum L1 | 50,397 | 11.4 | **21.2%** | 40.4% | 29.8% | 0.9% |
| X Layer | 11,170 | 2.5 | **0.3%** | 97.0% | 42.3% | 0.0% |
| Celo | 9,779 | 2.2 | **90.1%** | 98.0% | 96.7% | **14.5%** |
| Gnosis | 4,115 | 0.9 | 6.4% | 99.5% | 96.9% | **24.1%** |
| Arbitrum | 1,387 | 0.3 | 86.7% | 97.4% | 91.1% | 2.1% |
| Polygon | 629 | 0.1 | 13.5% | 82.7% | 67.7% | 14.1% |

## Analysis

The previous note asked where ERC-8004 identities and reputation live. This one is narrower: when you meet an ERC-8004 agent, **where is its identity actually stored?**

**What the standard designed.** Identity is an ERC-721 plus a pointer. From the start it was meant to be dual: some of the identity on the contract, the rest in an external file where the agent has more room for skills, services, and endpoints. Feedback can even attach a third pointer, which sometimes holds the same document and sometimes another.

The numbers match that design: **100%** of agents have an on-chain name, and more than **84.1%** have a parsed URI profile.

**What the data show.** As before, you have to go one layer down or you stay in the spec.

1. **The registry is a directory, not a catalog.** An on-chain name for 441,794 agents sounds like transparency. In detail, more than half of those names have no description long enough to read, and **no** chain profile in this index carries services, skills, or tags.

   If you only look at the on-chain record, about one in two agents will look like a dummy or get discarded for lack of information. Measuring the quality of the ~50% that do write enough is a later note. The headline already is that metadata quality is an alarm.

   *Stay on the NFT and you learn the agent exists. You do not learn what it does.*

2. **`agentURI` is a pointer — for better and for worse.** An off-chain pointer gives the contract a freedom it does not have. It also puts the burden on whoever hosts it: keep it up, and keep it possible to check that the file is still there and still current.

   That already shows. About **88.6%** of agents declare an external pointer. **79.7%** have a usable name plus description in that file. Ask for an image as well and it drops to **69.3%**. Ask whether the agent declares what it does (`services`) and only **41.0%** do.

   *Great power, great responsibility.*

3. **Chain culture does more work than the spec.** Celo writes a usable on-chain card for **90.1%** of its agents. X Layer does it for **0.3%** and still sets a URI on **97%**. Ethereum L1, which opened the year as the only venue, now has the weakest dual coverage: **21.2%** usable on-chain and **40.4%** with a URI profile. BNB, which holds **67%** of the stock, keeps the split the standard imagined: a half-filled NFT and an almost complete URI file.

   In one ERC-8004 ecosystem, each chain’s habits — and often the requirements of its main marketplaces — show up in how local agents register.

   *We copy the habits of whoever we follow.*

## Conclusion

On-chain identity is not mature enough, on its own, to let you know the agent you want to interact with.

That is a discoverability problem. An agent that only reads the registry hits a hard wall, and ends up needing tools that fetch the off-chain `agentURI`.

It also pushes agent marketplaces to keep their own processes for reading `agentURI` and filling in services, so agents can actually find each other.

And a third fact: any tool that wants to analyze or verify agents has to read off-chain to complement what each agent claims to have, and keep extra processes to check that the file is still current and reachable.

Fold in the earlier map of ERC-8004 growth and the standard has already moved through stages: first a mint surface, then a reputation surface, and now a **documentation surface that lives off-chain**. Read only the registry and you undercount what agents say they are. Read “441k agents” as 441k complete cards and you overcount what the chain knows by itself.

## What this does not show

- That a parsed profile is live, honest, or unique. Present is not reachable, and it is not a quality score.
- That the 15.1% “with no card” do not exist: all of them have an on-chain name. What is missing is a usable description or a parsed URI/feedback profile.
- Clones, templates, or richness. That is a different reading.
- Validation. The contract is not on mainnet.
- Every ERC-8004 deploy — only the eight mainnets in this index.

## How these numbers were cut

Snapshot **24 August 2026**, ~04:08 UTC. Identity on mainnet, eight EVM chains: Ethereum L1, Base, Polygon, BNB Chain, Arbitrum, Celo, Gnosis, and X Layer. Testnets are out.

A “usable card” is a non-empty name **and** a description of at least 10 characters. Origins are counted from profile rows (`chain`, parsed URI, feedback), not from a merged view. A document stored behind the pointer means bytes were saved; it is not a liveness check.

Related read, not republished here: eight months of ERC-8004 on mainnet.

Data indexed by Global Score Agent. This is a reading of ERC-8004 identity documents, not a product scorecard.

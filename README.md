# cloud-itonami-lei-zsn2lwnpyw6ismruc664

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Origin Energy Limited.**

Archives the publicly published legal/policy text of **Origin Energy Limited**, with source-url and retrieval-date provenance, per ADR-2607110300. Read-only reference/archive repository. Part of the worldwide-scope extension (batch AU-ME-UTIL-1, 2026-07-19).

## Company identity

- **Legal name**: Origin Energy Limited
- **LEI (ISO 17442)**: [ZSN2LWNPYW6ISMRUC664](https://search.gleif.org/#/record/ZSN2LWNPYW6ISMRUC664) (GLEIF entity-verified, AU)
- **Jurisdiction**: AU
- **Website**: https://www.originenergy.com.au
- **Ticker**: ORG (ASX)
- **ISIC Rev.5**: 3510

## Contents

- `facts.edn` — verified public-registry facts about this entity, each value
  carrying the URL it was read from and when. Generated; see below.
- `scripts/verify-facts.cljs` — re-fetches every URL `facts.edn` cites and
  fails if the live registry no longer says what is recorded here.
- `80-data/public/tos.journal.edn` — EDN quad-log of the archived legal text.
- `NOTICE` — copyright/attribution statement.
- `blueprint.edn` — machine-readable company identity record.

## Verified registry facts

`facts.edn` holds 11 entities read from 9 GLEIF URLs: the LEI record itself;
the Local Operating Unit that maintains it and GLEIF's accreditation of that
LOU; the Australian registration authority (`RA000014`, the Australian
Securities and Investment Commission) that corroborates `ACN 000 051 696`; the
ISO 20275 legal form `R4KK`, *Public Company limited by shares*; both parent
reporting exceptions; a summary of instrument identifiers and one of direct
children; and the two entities GLEIF records as directly consolidated by this
one. Every value carries `:source/url`, `:source/http-status` and
`:source/retrieved-at`; the shape is tx-data, so it loads with
`(d/transact conn (edn/read-string (slurp "facts.edn")))` and joins on
`:company/lei`.

Check it against the live registry — a bare `git clone` is enough, no workspace
and no dependency resolution:

```bash
nbb scripts/verify-facts.cljs           # check
nbb scripts/verify-facts.cljs --write   # re-fetch and rewrite facts.edn
```

The exit codes are three rather than two, because a check that could not run
must not be indistinguishable from one that ran and found nothing: `0` every
cited URL answered and every fact still matches, `1` a citation broke or a fact
drifted, `3` the check could not be performed and is refusing to report a pass
(`facts.edn` missing or unreadable, or every request failing at the transport
level — a machine with no egress must not publish a green check).

### On this entity's parents

Origin Energy Limited reports no parent at either consolidation level, and
`facts.edn` says so explicitly rather than by omission: GLEIF publishes, per
level, *either* a parent *or* a reporting exception explaining why there is
none, and 404s whichever does not apply. Here both exception endpoints answer,
with category `DIRECT_ACCOUNTING_CONSOLIDATION_PARENT` /
`ULTIMATE_ACCOUNTING_CONSOLIDATION_PARENT` and reason `NO_KNOWN_PERSON`, and
both are recorded. The verifier asserts that either/or on every run and fails if
a level ever answers on neither side — the one combination under which this file
would fall silent about who consolidates this company, in a way a reader could
not tell apart from a company that genuinely has no parent.

The same reasoning governs the two counts. `:securities/isin-count` is `0`
here, and that is a measured zero: the `isins` page was fetched and reported an
empty collection, so GLEIF maps no instrument identifiers to this LEI — it is
not a question nobody asked. `:relationship/direct-child-count` is `2`, and
both children are recorded below it as their own entities rather than left as a
number.

## Design rationale

See ADR-2607110300 and the worldwide-scope extension ledger in `com-junkawasaki/root` (`90-docs/adr/`).

# Trade — Mechanics

> **Topic** — resource trade and the international market: routes, influence, ECS, contracts.
> **Scope** — covered: the trade system. Not covered: convoy warfare specifics (future `Navy.md` / `Logistics.md`), autonomy mechanics ([[hoi4-autonomy]], future `Puppet.md`), fuel economics (`Fuel.md`).
>
> **Sources & version anchors**
> - [w] game wiki *Trade* (rev 98827, 99% translated) and *International market* (rev 83405, 60% translated — untranslated English text was used directly), via the Chinese mirror, snapshot 2026-09-10.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Trade exchanges strategic resources for the use of civilian factories; the trade law sets aside a share of domestic extraction for the world market whether or not deals exist, and puppets add resources beyond their master's own laws. [w:Trade §出口到市场的资源]
- A deal needs a non-hostile partner with trade influence ≥ 0, market access, and goods; the exporter does not approve it; the importer pays at least 1 civ factory and can buy up to 8 resource units per factory, paying the full factory even when fewer units are available; puppet imports buy 80 units per factory from tight puppets (collaboration governments, reichskommissariats) down to 16–10 for looser autonomy levels. [w:Trade §进行贸易]
- Deals cancel automatically when the importer runs out of free factories; resources also need a working route (land, or convoy plus naval access), otherwise nothing is delivered while the factories stay consumed. [w:Trade §进行贸易]
- Under scarcity, allocation follows trade-influence shares; influence factors: base 150, overlord→puppet +400, opinion between −100 and +100, distance −0.02 per 7.114 km, existing deals +2/month up to +50, ideology support up to +50, anti-monopoly −100, and an operative's Control Trade mission at +0.5/day stacking ×0.5 up to +50. [w:Trade §贸易影响力]
- Sea trade uses convoys that enemies can raid; active routes are visible in the resource map mode, and naval access levels exclude ports from route search. [w:Trade §运输货物]
- International market: the buyer allocates civ factories whose output becomes Economic Capacity Surplus (ECS) for the seller; ECS boosts the seller's construction lines up to a default 25% per line (~22.5%–27.5% across trade laws) and drains as it is used. [w:International market §Economic Capacity Surplus]
- Market access is mutual, guaranteed between faction members and subjects/overlords; war or embargo removes it and cancels all contracts. [w:International market §Market Access]
- Buying: at most 15 civ factories per contract; deliveries arrive every 30 days by convoy and can be intercepted; the seller reviews requests by default and prefers short contracts. [w:International market §Purchasing]
- Selling: only stockpiled equipment; low/normal/high prices are 0.75× / 1× / 1.25×, where normal is 2× production cost in ECS; a purchase request reserves the equipment; editing offers does not change ongoing purchases. [w:International market §Selling]
- Subsidies from focuses, decisions and events pay the seller +100% ECS, are usually restricted to an equipment type and a specific seller, and drain as they are used. [w:International market §Subsidies]

## Engine view

- Resources are defined in `common/resources/00_resources.txt`: `cic` is the resources needed to trade for 1 civ factory (default 0.125 → 8 units per factory, capped at 1) and `convoys` is the amount one convoy carries (default 0.1 → 10). [m:Resources modding.md → CIC, Convoys] [v:common/resources/00_resources.txt @1.18.3] [w:Trade §参考资料]
- Trade defines: `NTrade.ALLOW_TRADE_CUT_OFF = 0`, `NTrade.BASE_TRADE_FACTOR = 150`, `NTrade.PUPPET_MASTER_TRADE_FACTOR = 400`, `NTrade.RELATION_TRADE_FACTOR = 1`, `NTrade.DISTANCE_TRADE_FACTOR = −0.02`, `NTrade.MONTH_TRADE_FACTOR = 2`, `NTrade.MAX_MONTH_TRADE_FACTOR = 50`, `NTrade.ANTI_MONOPOLY_TRADE_FACTOR = −100`, `NTrade.ANTI_MONOPOLY_TRADE_FACTOR_THRESHOLD = 0.5`, `NTrade.BASE_LAND_TRADE_RANGE = 1000`. [v:common/defines/00_defines.lua → NTrade.* @1.18.3] [w:Trade §参考资料]
- Map scale is `NGame.MAP_SCALE_PIXEL_TO_KM = 7.114`; operative trade influence uses `NOperatives.OPERATIVE_BASE_CONTROL_TRADE_DRIFT = 0.5`, `NOperatives.CONTROL_TRADE_STACKING_FACTOR = 0.5`, `NOperatives.CONTROL_TRADE_MAX_INFLUENCE = 50`. [v:common/defines/00_defines.lua → NGame.MAP_SCALE_PIXEL_TO_KM, NOperatives.OPERATIVE_BASE_CONTROL_TRADE_DRIFT, NOperatives.CONTROL_TRADE_STACKING_FACTOR, NOperatives.CONTROL_TRADE_MAX_INFLUENCE @1.18.3] [w:Trade §参考资料]
- A land route exists while it can be traced through non-hostile states and stays within `sqrt(ln(2 + statesA + statesB)) × 1000 × 7.114 km`. [w:Trade §运输货物]
- Without a land route a port pair is chosen around the capitals (search expands until 10 port states are found) and scored as `F × (T − Σ_regions P×A) + 10 × (Ls + Ld)`, where A is 1 normally, 1000 for blocked sea zones and 100 for avoid; route length is weighted by naval access and higher port levels maximize the score. [w:Trade §运输货物]
- International market defines: `NMarket.PURCHASE_CONTRACT_DELIVERY_TOTAL_DAYS = 30`, `NMarket.IC_TO_CIC_FACTOR = 2.0`, `NMarket.MAX_CIV_FACTORIES_PER_CONTRACT = 15`, `NMarket.LOW_PRICE_LEVEL_FACTOR = 0.75`, `NMarket.HIGH_PRICE_LEVEL_FACTOR = 1.25`, `NMarket.PURCHASE_CONTRACT_SUBSIDY_BONUS_SPEED_FACTOR = 1.0`; market-access automation defaults to `NMarket.REQUEST_AUTOMATION_AUTO_ACCEPT_MARKET_ACCESS_DEFAULT` and `NMarket.REQUEST_AUTOMATION_AUTO_SEND_MARKET_ACCESS_DEFAULT` true while `NMarket.REQUEST_AUTOMATION_AUTO_ACCEPT_PURCHASE_DEFAULT` is false. [v:common/defines/00_defines.lua → NMarket.* @1.18.3] [w:International market §参考资料]
- The ECS construction boost default is `NProduction.CIC_BANK_SPEED_BOOST_FACTOR = 0.25`. [v:common/defines/00_defines.lua → NProduction.CIC_BANK_SPEED_BOOST_FACTOR @1.18.3] [w:International market §Economic Capacity Surplus]

## Modding surfaces

- Trade laws are ideas of the `trade_laws` category inside `common/ideas/_economic.txt` → [[hoi4-idea]], Idea modding.md. [v:common/ideas/_economic.txt → trade_laws @1.18.3] [m:Idea modding.md]
- Trade modifiers: `trade_opinion_factor`, `faction_trade_opinion_factor`, `trade_cost_for_target_factor`, `overlord_trade_cost_factor`, `extra_trade_to_overlord_factor`, `trade_laws_cost_factor`, `control_trade_mission_factor`, `autonomy_gain_trade`. [m:List of modifiers.md → trade_opinion_factor, trade_cost_for_target_factor, overlord_trade_cost_factor, trade_laws_cost_factor, control_trade_mission_factor]
- Adding or removing resources, their `cic`/`convoys` values and UI hooks → Resources modding.md. [m:Resources modding.md → CIC, Convoys, Interface]
- Convoy capacity comes from convoy equipment stats → Equipment modding.md, [[hoi4-tech]]. [m:Equipment modding.md]
- Resource deposits are placed in state history / map files → State modding.md, Resources modding.md, [[hoi4-map]]. [m:State modding.md] [m:Resources modding.md]
- Trade law pricing/cost modifiers can be applied from ideas (`economy_cost_factor`, `trade_laws_cost_factor`) — see the filename-order pitfall for law overrides in Idea modding.md. [m:Idea modding.md → law = yes; file order]

## Unknowns

- The anti-monopoly condition wording is garbled in the mirror translation; only the define pair (`NTrade.ANTI_MONOPOLY_TRADE_FACTOR = −100`, threshold 0.5) is certain.
- Script effects for market subsidies or contracts are not present in the snapshot used here (unsourced).
- The puppet import discount (80 vs 16–10 per factory) is wiki prose; the underlying autonomy-side modifiers were not traced to definitions in this pass.
- Trade-influence contribution of ideology support ("up to +50") has no define anchor found in the sources used (unsourced number).

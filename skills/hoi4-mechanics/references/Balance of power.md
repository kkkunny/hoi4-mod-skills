# Balance of power — Mechanics

> **Topic** — balance of power (BoP): two-sided meters, bands, and their modifier payloads.
> **Scope** — covered: the BoP system and its shipped examples. Not covered: focus/decision content that moves the meters (see [[hoi4-focus]] / [[hoi4-decision]]), and the modding write-up (`Balance of power modding.md`).
>
> **Sources & version anchors**
> - [w] game wiki *Balance_of_power* (rev 103368, 100% translated), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- A balance of power has two sides (left/right) with ranges from −100% to 100%; each side has multiple bands, and a middle range belongs to neither side; a BoP can only be replaced by another BoP of the same side (Italy: Mussolini's side can be replaced by Balbo's, not by the communists). [w:Balance of power]
- BoP state is managed by focuses, events and decisions that apply country or state modifiers; the page gives no automatic drift values. [w:Balance of power]
- Italy's BoP: left sides are the Grand Council, the Papacy, and the Communist faction; right sides are Mussolini, Balbo, Grandi, Victor Emmanuel III, Umberto II and the Socialist faction; bands are −100…−90, −90…−60, −60…−30, −30…−10 (left) and 10…30, 30…60, 60…90, 90…100 (right) with a −10…10 middle. [w:Balance of power §意大利]
- Italian band payload examples: Grand Council gives advisor cost −25…−10% and war-support/stability penalties, and low bands can trigger the Italian civil war or leader-deposition events; Papacy gives stability/PP/neutral-party drift; Mussolini gives PP/stability/war support up to +15/+5/+5% at the top band; Balbo trades construction speed for factory output and army/air XP; Grandi cuts law costs and boosts construction; the king/Umberto give stability, army XP, surrender limit, drift defence and annexation discounts; the Socialists cut puppet costs and raise compliance. [w:Balance of power §意大利]
- Other shipped examples: Ethiopia (Mesafint vs Mekwanint, with disintegration/rebellion events at fixed bands), Switzerland (Federal Council vs Cantons; advisor costs down to −100%), Finland (public trust vs the leader ladder), Denmark (Welfare vs War) and Czechoslovakia (army readiness bands with stability/resource effects). [w:Balance of power §埃塞俄比亚 §瑞士 §芬兰 §丹麦 §捷克斯洛伐克]

## Engine view

- No NDefines names are cited by the page (the system is data-driven): the state is defined by BoP definitions plus focuses/events/decisions applying modifiers, with country and state scopes and scripted flags (Ethiopia uses `ETH_mesafint_power_flag`-style flags). [w:Balance of power]
- Band payloads are ordinary modifiers (advisor costs, PP/stability/war support gains, intel, construction, XP gains, surrender limit, annexation cost, initial compliance…); values are the content's, not engine constants. [w:Balance of power]

## Modding surfaces

- BoP definitions and arguments → Balance of power modding.md, [[hoi4-modifier]]; moves are driven by focuses/decisions/events → [[hoi4-focus]], [[hoi4-decision]], [[hoi4-event]]. [m:Balance of power modding.md]
- The modifiers inside bands are standard modifier keys → List of modifiers.md. [m:List of modifiers.md → political_advisor_cost_factor, war_support_factor, stability_factor]

## Unknowns

- The page lists no automatic drift numbers and no NDefines; drift is entirely script-driven (unsourced engine constants).
- Several page bugs are flagged by the page itself: shown vs actual state modifiers for Ethiopia's bands, and the Denmark band labels printed as negative numbers — reproduced as printed here, not resolved.
- Only illustrative band values are transcribed; the full Italian/Ethiopian tables remain on the page.

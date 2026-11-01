# Great Circle Tycoon: play-through review

Artifact reviewed: https://claude.ai/artifact/VBbvZ4xzHAN9nbwuUP63G5 (version 1793510065-1bb4, 649 KB, single page).

How it was tested: the page was driven headless in Chromium on a 390x844 phone viewport and a 1366x820 desktop viewport, in light and dark mode. Solo games were started in North America, Europe and "Anywhere"; routes were built by hand for every mode; a hub was built; sheets, menus, zoom and tap hit-testing were exercised. The simulation core (lines 484 to 3577 of the page) was also run in Node for 8 to 12 game years under passive play, Autopilot (balanced and bold) in four regions, and a borrow-and-build strategy. No JavaScript errors were seen in any run. Screenshots: `gct-review-phone.png`, `gct-review-desktop.png`.

## What already works well

- Close-up rendering is excellent: distinct vehicle silhouettes per mode, rail ties, dashed sea lanes, great-circle air routes, hub diamonds, label collision avoidance.
- Dark mode is complete and legible, including canvas colours.
- Sheets are information-dense but well organised (route, city, company, finance, autopilot).
- Hit-testing in dense Europe at region zoom resolved six adjacent cities (Brussels, Lille, Rotterdam, Essen, Cologne, Amsterdam) correctly.
- 60 fps at normal scale on the phone viewport; the long game (424 routes, 1,692 vehicles) still held 57 fps at region zoom.

## Gameplay findings, ranked

1. **Free takeover immunity on day one (exploit).** Buying 21% of your own company at the start costs about $1.3M (1% = $62k). The "Own 51% of your own company" goal then pays $2.0M. Net +$0.7M, and no rival can ever reach 51%. Eight passive years afterwards: no threat at all. Fix: pay the goal only after, say, year 2, or scale the bonus to the price paid, or start the player at 51% and make the threat come from dilution (rights issues) instead.
2. **The shipping rival dwarfs everything.** `spawnRival` gives the ship persona $20M cash and a $42M ship budget; others get $5M and $2.4M to $14M. Oceanic is worth ~$50M on day one, $250M by year 8, ~$1B by year 11, while the best Autopilot player reaches ~$100M in 6.5 years. Oceanic then buys the player's float (44% to 50% in passive runs). Fix: start all rivals at parity with the player and let them grow, or give the ship persona one feeder ship.
3. **Sea revenue is paid per kilometre.** Goods Hamburg to Seoul pay $1,133 per tonne versus $60 per tonne for 1,000 km by rail. One container ship on that lane estimates $13.6M to $17.7M a year on a $42M outlay. Real container freight is nearer $50 to $150 per tonne. Fix: a distance-decay fare for sea (and long-haul air), for example `price * d / (1 + d / 4000)`.
4. **Road spam is the dominant strategy.** Autopilot bold reached 356 routes and 1,235 vehicles by year 9 with $324M road revenue and $0 air revenue; it opened its first rail line in year 7 to 11 and never flew. Players get the same signal. Fix: per-route admin overhead that grows with route count, or road terminal congestion, so bigger vehicles and modes pay off mid-game.
5. **The early game has a wall and the UI points at the wrong side of it.** Starter route earns ~$600k a year. The opportunity rings and the top-six "$/yr" chips are scaled by raw profit, so they point at $42M routes (Orlando to Seoul, $9.35M a year) when the player has $4M plus a $10M credit line. The planner then says "You're $38.0M short. Try fewer vehicles." with one vehicle selected. Fix: scale rings by profit on routes you can afford now (cash + credit), dim the rest, and replace the dead-end message with "Needs $42M. You can reach it by ...".
6. **Leg times expose the time scale.** A 12,000 km flight shows "9.0 days each way"; Orlando to New York by train "8.2 days". Show "round trips per month" or hide the figure.
7. **Air is weak as a rival persona and as a player mode.** Skyline ends year 9 at $70M and locked out by Ironhorse in several runs. Worth a pass on airport cost ($4M each) and air pax pricing.
8. **Goal tiers.** "Build a rail line" and "Launch a shipping line" sit in Getting Started but Autopilot needs 7 to 11 years to hit them. Either move them up a tier or add a cheap nudge ("Cheapest rail you can afford today: Orlando to Miami, $7.5M").
9. **Sea pick shows zero rings.** From Hamburg, every profitable sea destination was off-screen, so the hint "Bigger rings earn more" sat over an empty map. Pan or zoom to include the top three ring cities when entering pick-B.

## Visual and UX findings, ranked

1. **Toast flood.** With Autopilot on at fastest speed, 21 toasts arrived in 12 seconds. Each covers the top third of the phone map. Batch autopilot messages ("Autopilot opened 4 routes this month"), send them to the news feed, and keep toasts for goals, threats and money events.
2. **Sheets hide the thing they describe on phones.** Opening the planner for Orlando to Seoul left Greenland in the visible strip of map. Pan the camera so the origin (or the route midpoint) sits in the uncovered area when a sheet opens.
3. **Start zoom is too far out on phones.** The starter route is a 20 px sliver on a hemisphere view; vehicles are invisible. Fit the camera to the starter route with padding so the first thing the player sees is their coaches moving.
4. **Floating income labels pile up.** "+$8.5k" over "+$15.5k" over "Dallas" at region zoom. Merge floaters per screen cell rather than per city, cap to one per city per 1.5 s, and skip them when more than ~40 vehicles are on screen.
5. **Vehicles bury labels and lines at scale.** By year 9, trucks cover LOS ANGELES and CHICAGO entirely. Add vehicle boxes to the label collision set, and switch to a level of detail: below a zoom threshold draw routes only, or one marker per route.
6. **Performance at world zoom in the long game.** 19 fps on the phone viewport with 1,692 vehicles (headless, software rendering, so real devices will do better, but the trend holds). The level-of-detail change above fixes this too; caching the route layer per mode into an offscreen canvas when the camera is still would also help.
7. **HUD collision.** At "326 routes · 1144 veh." the money ("$14.7M") overlaps the subtitle. Give `.hud-cash` a min width or let the money shrink with `clamp()`, and hide the vehicle count below 400 px.
8. **Rival legend.** Shows three dots for four rivals, and the dots are generic swatches, not the actual rivals. Render the real rival swatches with their short names. Also, slot colours r1 (olive) and r6 (dark gold) read the same in dashed lines; r4 (brown) sits close to road orange.
9. **Copy.** Welcome text says "by road, rail and air" but the game has sea. Company name truncates to "CROSSWIND T..." on phones; prefer the short name in the HUD.
10. **Routes list at scale.** 356 flat rows, 660 ms to open, no filter. Group by mode or region, show top 20 by profit with "show all", and add a search box.

## Numbers behind the findings

| Scenario (Node, seed fixed) | Year 1 value | Year 6 | Year 12 | Notes |
|---|---|---|---|---|
| Passive, North America | $6.5M | $8.6M | n/a | Rivals bought 26% of float by year 8 |
| Passive, Oceania | $6.5M | $8.8M | lost in year 8 | Oceanic reached 50% |
| Autopilot balanced, NA | $6.8M | $84M | $708M | $100M goal at 6.5 y; 423 routes |
| Autopilot bold, NA | $8.0M | $85M | $872M | 578 routes, $674M loan |
| Oceanic (ship rival), typical | $48M to $58M | $148M to $293M | $789M to $1.28B | Starts with $62M of assets |

Self-share exploit: 21% for $1.30M, goal bonus $2.00M, immune thereafter (tested 8 years).
Sea unit price: $1,133 per tonne of goods Hamburg to Seoul; rail $60 per tonne per 1,000 km; road passengers $60 per 800 km.
Perf (phone viewport, headless): 60 fps at year 3 (24 routes); 57 fps region zoom and 19 fps world zoom at year 9 (424 routes, 1,692 vehicles).
Toasts: 21 in 12 s at speed 4 with Autopilot on.

# Great Circle Tycoon changes

## Balance fixes and new vehicle art (artifact version 14)

Built on top of version 13, which added Easy, Normal and Hard difficulty, the business cycle,
buyout bids and head-office costs. That version's head-office cost already covers route sprawl,
so no second overhead was added.

### Gameplay
- **Buybacks wait for the share market.** You can no longer buy your own shares before rivals can
  trade them (18 months on Easy, sooner on Normal and Hard). The "Own 51% of your company" bonus is
  now $1.5M. Before, 21% of your own shares cost $1.3M on day one and the goal paid $2M back.
- **Rivals start even.** Every rival starts with the same cash. A shipping line opens with a small
  ship where one pays, otherwise one container ship financed partly by a bank loan. Its starting
  net worth fell from about $62M to about $18M.
- **Fixed: ship rivals could start with no route.** Their starter search checked ship range
  against straight-line distance, but opening a route checks the real sea distance.
- **Long sea voyages are priced like real freight.** The rate per km falls with distance
  (`km / (1 + km / 10000)`). Hamburg to Seoul goods paid $1,133 a tonne; the best long-haul
  container lanes now return about 28% a year instead of 40%+.
- **Autopilot builds bigger things.** It weighs each candidate's return by the size of its profit
  and only shortlists routes it can afford today, so it opens rail lines, ships and flights
  instead of only trucks.
- **Fixed: two rivals could share a colour.** The fourth rival reused slot 1. Old saves are repaired on load.

### Interface
- The start view frames your first route so you see your vehicles moving.
- Opening a route, city or planner sheet on a phone pans the map so its subject stays in view.
- Build rings are sized to what you can afford now. Routes you can't pay for yet show as faint
  dashed rings with "needs $X". The "you're short" message no longer says "try fewer vehicles"
  when you have one.
- Picking a start city zooms out to show the best destinations if they are off screen.
- Trip times read as round trips a month, not "9 days each way".
- Routine toasts are paced to one every 2.2 s and rival moves to one every 3 s. Autopilot moves
  are grouped into one message. Everything is still in Finance > News.
- The routes list has mode filters, a city search, and shows the top 25 first.
- Goals suggest your best first rail line and shipping line, with a button that opens the planner.
- Money floaters merge by screen area and draw beneath city names.
- The legend shows the real rivals' colours. Rival colours no longer clash with mode colours.
- The HUD keeps the cash readable on narrow phones.

### Vehicles
- New top-down art for 20 vehicle types: buses, coaches, vans, semis, road trains, regional,
  high-speed, maglev and freight trains, turboprops, jets, widebodies, freighters, supersonic and
  blended-wing aircraft, feeder and mega container ships, tankers, bulk carriers and ferries.
  See `gct-vehicles.png`.
- Planes cast a ground shadow. Ships leave a wake. Rival vehicles use the rival's colour.
- Each kind is painted once per colour into a sprite and stamped with rotation, so drawing costs
  about 2 to 5 ms a frame even in a 1,300-vehicle late game.
- Far out, or on a crowded screen, vehicles become dots so routes and names stay readable.

### Tested
- 12 seeded 10-year Autopilot games on Normal, against version 13 with the same seeds:

| | Version 13 | This version |
|---|---|---|
| Player games lost | 0 of 12 | 0 of 12 |
| Median player value | $108M | $129M |
| Median strongest rival | $434M | $168M |
| Median shipping rival | $434M | $136M |
| Games using rail, sea or air | 2 of 12 | 9 of 12 |

- Headless play-through on phone and desktop in light and dark mode, with no console errors.
- Saves from the original version and from version 13 load cleanly.

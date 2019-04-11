# Dynamic Auto-Scaling

## How Dynamic Threshold Works

### Flow
```
┌────────────────────────┐                                                            ┌──────────────────────┐
│                        │                                                            │ Score < 50 for Scale │
│     Determine TPS      │                                                            │         DOWN         │
│  seasonality & trend   │                                                            │ Score > 50 for Scale │
│                        │                                                            │          UP          │
└────────────────────────┘                                                            └──────────────────────┘
             │                                                                                    ▲
             │                                     ┌────────────────┐                             │
             │                                     │ Use predicted  │                             │
             │                                 ┌──▶│     trend      │──────┐                      │
             ▼                                 │   └────────────────┘      │                      │
┌────────────────────────┐                     │                           ▼                      │
│                        │                     │              ┌─────────────────────────┐         │
│     Calculate TPS      │                    No              │      Reward function:   │         │
│ upper-lower bound for  │────▶  Anomaly?   ───┤              │     R(action) based on  │─────────┘
│       every 30m        │                     │              │        SLA metrics      │
│                        │                     │              └─────────────────────────┘
└────────────────────────┘                   Yes                           ▲
                                               │   ┌────────────────┐      │
                                               │   │  Use detected  │      │
                                               └──▶│trend w/ cycles │──────┘
                                                   └────────────────┘


```
### Steps
- Define a reward function __R(action)__ for application, using by default
`R(action) = M(latency) < SLA ? (action==UP?-1,1), (action==UP?1,-1)`:
  - Use a static criteria for `M(latency)`
  - Use dymanic criteria for `M(latency)`: 3 sigma of standard deviations
  - Use Min of above two
- Build model for historic `M(tps)` with detected seasnality, and trending.
- Every *PT* mins, Foremast predicts `M(tps)` for *PT* mins using derived model
- If real value of `M(tps)` falls in predicted range, use predicted trending as `Tr(tps)`.
Otherwise, observe *N* data points to determine anomaly trending as `Tr(tps)`
- Determine scaling recommendation:
  - If `Tr(tps)` is UP, return a score > 50 when __R(UP)__ == 1
  - If `Tr(tps)` is DOWN, return a score < 50 when __R(DOWN)__ == 1

### Reward Function
The default reward function R(action) considers two conditions:
1. SLA metrics
2. Resource allocation

If a SLA metrics is not provided, *Latency* will be used.
The rule is simple - reward function will
reward lower resource allocation as long as SLA metrics is not vialated.

## Samples

```
10.00  ┤                             ╭╮       ╭╮
 9.00  ┤                     ╭╮      ││       ││
 8.00  ┤                     ││     ╭╯│╭╮     │╰╮
 7.00  ┤                     ││    ╭╯ │││     │ │
 6.00  ┤                   ╭╮││    │  │││╭╮   │ │
 5.00  ┤                   ││││╭╮  │  ││││╰╮  │ │
 4.00  ┤                   ││││││╭╮│  ││││ │╭╮│ │
 3.00  ┤                   │││╰╯││││  ││││ ││││ │
 2.00  ┼─╮  ╭─────╮ ╭╮   ╭─╯╰╯  ╰╯││  ││╰╯ ╰╯││ ╰╮╭╮╭──╮  ╭─╮╭╮  ╭─╮╭╮ ╭─╮ ╭─╮╭──╮╭╮╭─╮    ╭╮╭╮╭╮
 1.00  ┤ ╰──╯     ╰─╯╰───╯        ╰╯  ╰╯     ╰╯  ╰╯╰╯  ╰──╯ ╰╯╰──╯ ╰╯╰─╯ ╰─╯ ╰╯  ╰╯╰╯ ╰────╯╰╯╰╯╰

```

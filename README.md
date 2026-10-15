# Data for "Reroute, wait or stock up? Designing inventory and capacity reservations for maritime disruption onset"

This repository contains the data behind the article, from the retrieved traffic observations to the numbers shown in every table and figure. It contains data only. Quantities are in units of mean weekly demand and prices in units of the purchase price of one quantity unit unless a column name states another unit.

## Citation

Please cite the article (reference to be added on publication) and, for the traffic data, the IMF as stated under Sources and terms of use.

## Contents

| Folder | What it holds |
|---|---|
| `portwatch/` | IMF PortWatch daily chokepoint and port observations as retrieved, and the query log |
| `derived/` | Weekly series, reference medians and regime definitions used to build the disruption scenarios |
| `costs/` | Cost parameters, public sources, Drewry World Container Index values and conversion assumptions |
| `results/` | Designs, new path tests, historical replays, simple rules, sensitivity runs, relative cost analysis, optimizer comparison and the reduced DP reference |
| `figure_data/` | The plotted values of Figs. 1 to 9 and Supplementary Figs. S1 to S4 |

## Where the article's numbers come from

| Article item | File |
|---|---|
| Fig. 1, Section 3.1 | `derived/weekly_chokepoints.csv`, `derived/weekly_ukraine_ports.csv`, `derived/weekly_gulf_port_calls.csv`, `derived/gulf_port_comparison.csv` |
| Table 2 | `costs/parameters.csv`, `costs/calibrated_parameters_by_cargo_value.csv`, `costs/sources.csv`, `costs/wci_assessments.csv`, `costs/conversion_assumptions.csv` |
| Table 5, SI Section S11 | `results/new_path_tests.csv` |
| Table 6, Fig. 2, Fig. 5, SI replay tables, SI Fig. S2 | `results/replay_windows.csv`, `results/replay_weekly.csv` |
| Fig. 3, SI Section S6 | `results/simple_rule_members.csv`, `results/simple_rule_selections.csv` |
| Fig. 4, SI Tables S9 to S11, SI Fig. S4 | `results/transition_aware_runs.csv` |
| Fig. 6 | `results/replay_windows.csv`, `results/new_path_tests.csv` |
| Section 4.6, SI Section S4.1 (Tables S12 and S13) | `results/cargo_value_comparison.csv`, `results/simple_rule_selections.csv`, `results/transition_aware_runs.csv` |
| Section 4.7, Table 7, Figs. 7 and 8, SI Section S2, SI Fig. S1 | `results/threshold/` |
| Section 4.8, SI Section S7, SI Fig. S3 | `results/optimizer_benchmark_summary.csv`, `results/optimizer_benchmark_curves.csv` |
| Section 4.8, Fig. 9, SI Section S8 | `results/dp_reference.csv` |
| Limitations, mapping and demand checks | `results/replay_windows_robustness.csv` |
| Steady state plans of all policies, SI Section S3 | `results/steady_state_designs.csv` |

## Conventions

Cases are Ukraine, Gulf and Europe. The setting `ample` allows reserved land bridge and second channel capacity up to 1.0, and `scarce` caps both at 0.25; Europe has a single setting, left blank. A plan is the vector (SS, e, theta, b, j) of safety stock, weekly emergency capacity, emergency cover target, weekly land bridge capacity and weekly second channel capacity. Europe has no land bridge or second channel, so its plans have three entries (SS, e, theta). A transition aware design consists of a plan before onset and a crisis plan.

Policy abbreviations follow Table 4 of the article. TA is the transition aware design, SSA the steady state adaptive policy, ZD the zero-delay steady state policy, SR the static robust design with ex post regime data and SN the static normal-period design.

Reliability columns follow Section 3.2.4. `R_on` is onset reliability, the share of onset paths on which at least 95% of demand is filled over the 26 weeks starting with the onset week, with 95% Wilson interval `R_on_lo` to `R_on_hi` over 1,000 test paths. `R_no` is reliability on 300 paths without onset over 52 weeks. `cost`, `cost_on` and `cost_no` are the expected economic cost per unit of demand and its means over onset and no-onset paths. `fill_on` is the mean fill rate over the 26 weeks after onset and `emerg_on` the share of units received from emergency procurement on onset paths. Columns prefixed `design_` are estimates on the design sample (300 onset and 100 no-onset paths) and the others are test values.

## Files

### portwatch

`chokepoints_daily.csv` holds daily vessel transits at Bab el Mandeb (listed as Bab el-Mandeb), Suez, Hormuz and the Cape of Good Hope from 1 January 2019 to 4 October 2026, retrieved on 11 October 2026 (`n_container` is the series used). A repeat download on 12 October 2026 matched it record by record. `ports_daily.csv` holds daily port calls and estimated import and export container volume for the Ukrainian ports (Odesa, listed as Odessa, Chornomorsk, Pivdennyi, Izmail and Reni), Constanța (listed as Constantza) and the Gulf ports (Jebel Ali, Khalifa Port, Khor Fakkan, Fujairah, Sohar), retrieved on 12 October 2026. `retrieval_log.csv` lists every query URL with its retrieval time.

### derived

`weekly_chokepoints.csv` gives complete Monday-to-Sunday weeks with the container transit count, the ratio to the reference median (`ratio_*`), the ratio capped at 1 (`s_*`), that ratio rounded to the nearest 0.25 for the single period network calculation of SI Section S9 (`s_rounded_*`), the availability under the band mapping with s0 = 0.05 and s1 = 0.75 (`a_band_*`) and the regime. `reference_medians.csv` gives the medians over 3 January 2022 to 29 October 2023 and `regimes.csv` the regime boundaries. `weekly_ukraine_ports.csv` gives weekly container calls at Odesa, Chornomorsk and Pivdennyi with their ratio and band-mapped availability (s1 = 0.69). `weekly_gulf_port_calls.csv` and `gulf_port_comparison.csv` give the Gulf port calls inside and outside Hormuz. `regime_mean_ratio.csv` gives the mean capped ratio by regime.

### costs

`parameters.csv` reproduces Table 2. `calibrated_parameters_by_cargo_value.csv` gives the four calibrated parameters at cargo values of 27,250, 54,500 and 109,000 USD per TEU. `sources.csv` lists the public carrier notices, news reports, index pages and the cargo value estimate that were examined, with their publication and effective dates, lanes, charges and how each was used; sources marked as not used were examined and excluded for the stated reason. `wci_assessments.csv` gives the Drewry World Container Index Shanghai to Rotterdam assessments read from archived copies of the Drewry page in the Internet Archive, with the archive URL of each; the 20 assessments flagged `used_in_calibration_mean` (5 January to 9 November 2023) give the mean of 1,495.9 USD per 40 ft container. `conversion_assumptions.csv` states assumptions A1 to A7.

### results

`steady_state_designs.csv` gives the plan selected for each regime and each lead-time estimate, with design and test values. `new_path_tests.csv` gives the test results of all policies in the main setting. `transition_aware_runs.csv` gives every transition aware design with its weight `pi`, target `rho`, signal lag and contracting delay, cargo value, plans and the corresponding SSA and SR values; the run labels identify the main runs, the reliability frontiers and the sensitivity runs.

`replay_windows.csv` gives fill and cost on the observed availability path over 500 demand paths for each policy and window. Windows are `red_sea_onset` (4 December 2023 to 26 February 2024), `red_sea_established` (4 March 2024 to 23 February 2026), `hormuz_onset` (2 March to 25 May 2026), `hormuz_after_announcement` (22 June to 28 September 2026), `full` (25 April 2022 to 28 September 2026) and, for Ukraine, `normal`, `onset`, `established` and `full`. `fill_p05` is the 5th percentile of the window fill rate across demand paths and `P_fill95` the share of demand paths with window fill of at least 95%. Policies labeled with a lag and delay use the main plans under that signal lag and contracting delay, policies labeled `exit on announcement` return to the plan before onset in the week of 22 June 2026, and the signal aware ordering policy uses the normal-regime plan with lead times estimated from signals and no plan switching. `replay_windows_robustness.csv` repeats the replay with the proportional and binary mappings and with a demand coefficient of variation of 0.4. `replay_weekly.csv` gives the weekly series behind Figs. 2 and 5: aggregate fill, its 5th and 95th percentiles across demand paths, stocks, backlog, cargo held outside the strait, capacities in force and receipts by source.

`simple_rule_members.csv` gives every member of the six rule families at each cargo value with design and test values, flagging the member selected at equal no-onset cost and the cheapest member reaching 0.9. Family codes are `stock` (stock before onset), `bridge` (land bridge reservation before onset), `stock+bridge` (both), `standing` (crisis reservations always in force), `uniform` (the same stock increment in every regime) and `static` (one plan with stock and land bridge reservation). `simple_rule_selections.csv` gives the selected members with test and replay values and the TA and SSA references; `cargo_value_comparison.csv` summarizes the comparison across cargo values.

`threshold/plan_outcomes_design_paths.csv` gives, for each of the 6,750 crisis regime plans of the ample Gulf grid, the outcomes on the 400 design paths that do not depend on cost parameters (reliability `R`, fill, diverted, land bridge and stored quantities, supply by channel) and the cost components at the central costs; costs at other charge ratios follow by linear repricing. `threshold/sweep.csv` gives the selected plan and its supply shares at each computed diversion ratio `cdiv` (a fraction of purchase value) for the central costs and the four storage and land bridge fee conditions; `cls` is the supply structure. `threshold/switches.csv` gives the switch intervals and crossings of Table 7, `threshold/structure_best_objective.csv` the best objective within each structure, `threshold/reoptimized_policies.csv` and `threshold/reoptimized_simple_rules.csv` the policies and rules optimized again at the seven ratios of Fig. 8, with `best_by_design` marking the best simple rule.

`optimizer_benchmark_summary.csv` and `optimizer_benchmark_curves.csv` give the comparison of seven optimizers over 8 seeds, the curves as the best objective after each 10 distinct plans. `dp_reference.csv` gives the reduced DP comparison: operating cost per week, gaps to the reference in operating and total cost, fill and 13-week onset fill, with the selected parametric plans and model parameters.

## Sources and terms of use

Traffic data: Source: International Monetary Fund, PortWatch, https://portwatch.imf.org/. The files in `portwatch/` are unchanged records. The files in `derived/` are weekly aggregates and ratios computed from them as described above and in Section 3.1 of the article. The IMF provides these data as is; the IMF terms of use apply to the PortWatch records and their derivatives.

Cost sources: the carrier notices, news reports, the Drewry page and the cargo value estimate are copyrighted by their publishers and are not reproduced here. `costs/sources.csv` and `costs/wci_assessments.csv` give their URLs, dates and archive links and the values taken from them.

All other files were produced for the article and are released under the Creative Commons Attribution 4.0 International License (CC BY 4.0).

## Integrity

`SHA256SUMS.txt` lists the SHA-256 checksum of every file.

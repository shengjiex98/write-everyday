# Time Series Characterization of Gaming Workload for Runtime Power Management.

## Ideas

- Dynamic voltage and frequency scaling (DVFS) has not been extensivley studied for video game applications.
- Tested PID controllers but had questionable results. Only a small subspace of PID controllers lead to good game play and power savings.
- Used Least Mean Squares (LMS) and Autoregressive Moving Average (ARMA) models to *learn* parameters, which makes them applicable to games not a priori known, including closed source games.

## Summary of results

Based on testing with Quake II (open-source, software-rendered) as well as modern, closed-source and hardware-rendered games:
- **PID** based predictions worked poorly for all games
- **LMS** worked well for Quake II but not hardware-rendered games.
- **AR & ARMA** models worked well for all games with comparable performance to hand-tuned predictors.

By predicting workload based on intercepting API calls to graphics libraries, it is possible to apply this scheme to closed-source games as opposed to needing to alter the game's source code.

Plan to do future work with a more holistic approach to power management i.e. CPU, GPU as well as other components in mobile devices such as wireless modules.

## Background

DVFS relies on predicting future workload, which poses a problem for gaming due to its interactive nature (as compared to video).

Proportional-integral-derivative (PID) methods predict future workloads based on previous frams. But the gain values need to be hand tuned. This can be problemetic when encountering unseen games, close-sourced games, or hardware rendered games.
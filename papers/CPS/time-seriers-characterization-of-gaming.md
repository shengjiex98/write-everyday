# Time Series Characterization of Gaming Workload for Runtime Power Management.

## Ideas

- Dynamic voltage and frequency scaling (DVFS) has not been extensivley studied for video game applications.
- Tested PID controllers but had questionable results. Only a small subspace of PID controllers lead to good game play and power savings.
- Used Least Mean Squares (LMS) and Autoregressive Moving Average (ARMA) models to *learn* parameters, which makes them applicable to games not a priori known, including closed source games.
# Counterparty Credit Risk Jobs

A job spec is configuration data which defines all choices about how pricing and simulation are to be performed. Multiple job specs may exist in the system reflecting the different configurations required for different types of jobs. For example, all counterparty credit risk (CCR) production jobs will reference a single job spec which defines the appropriate models, calibrations and time buckets for CCR production requirements, whilst value at risk (VaR) will use a different job spec.

A job spec is bi-temporal in nature, for example over time the business may wish to amend the simulation settings defined in the CCR production job spec, without changing the ID of this job spec.

Under normal operation, a set of job specs would be created at system inception to cover all the different types of production jobs required. The creation of new job specs is expected to be a relatively infrequent event, required only when new business needs emerge or when a bespoke run is required.

A job is a specific instance that will be sent to the compute framework. It associates a job spec with a specific anchor timestamp and trade timestamp. These determine the precise bi-temporal version of market/reference data and trades respectively.

All choices over how pricing and simulation are performed should be inherited from the job spec, using the appropriate bi-temporal version of the job spec as of the anchor timestamp. It should not be bi-temporal in nature as a specific job should only be run once.

Under normal operation, many jobs will be created each day. The anchor timestamp will be constant across many jobs, as market data will be held constant throughout the day, only updating periodically (e.g. at end of day). The trade timestamp may be different every job as new trades will need to be picked up with each job to satisfy intra-day requirements.

The compute framework reads the info contained in a job and triggers all required simulation and pricing tasks as specified in the job. The compute framework has no business logic embedded; it is completely metadata driven and simply performs the tasks specified in the job and stores the output for use in aggregation.

A underlying market factor (UMF) represents a market factor taken as a whole, for example the USD libor curve or a volatility surface. The economic properties of a UMF are a system-wide definition – for example, the fact that USD libor is an interest rate is not a configuration choice but a fact. However, each job spec may specify a different way to model that particular UMF through specification of the calibration set. Also FX spot is another underlying market factor that is used for FX derivatives, such as https://finpricing.com/lib/EqCallable.html

A market data path represents a possible evolution of market data through time. Generally, all paths start at the same place with the real world market data, but evolve differently to each other over time. Future market data points on a path may be generated either through a simulation model (Monte Carlo paths), through application of pre-specified ‘shocks’ to each market data point, or may be real world values if the path is being generated retrospectively (e.g. for back testing).

The ARGO NCG is the core component of ARGO Monitoring Engine and it is responsible for the generation of the Nagios configuration based on information from main sources of truth - topology database (e.g. GOCDB, XML feed) and ARGO POEM. NCG enriches topology information with extra attributes and credential information in needed by Nagios to successfully run probes. Finally, NCG configures Nagios to publish metric results to AMS via component argo-nagios-ams-publisher.

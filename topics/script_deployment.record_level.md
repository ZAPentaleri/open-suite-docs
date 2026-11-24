# Record-level deployment
<!-- id: topic:script_deployment.record_level -->

Part of: [Script deployment](script_deployment.md)

See also: [Script records](script_record.md), [Deployment levels](script_deployment.levels.md),
[Deployment record](script_deployment.deployment_record.md).

Record-level deployment is the standard route for every script type that has entry points. It takes
three steps:

1. Upload the script file.
2. Create a [script record](script_record.md). The script record holds the script's internal ID,
   whether it is active, and its other details.
3. Create a script deployment. The deployment configures how and when the script runs; what it can
   configure depends on the script type.

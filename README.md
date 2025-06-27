# cu

An attempt at bringing postgres upgrades into the CI cycle

## Why "cu"?

Continuous upgrades.

## What's the plan?

1. Create and containerise two trivial apps that connects to PostgreSQL and run through two different predefined sets of SQL statements
  - one to create the initial state of the database and
  - one to fake live running
2. Create a configuration file specifying source and target PostgreSQL versions
3. Create checks to ensure source and target are identical
4. Use bencher to record performance of migration
5. Create an independent benchmark not using migration database
6. Use bencher to compare independent benchmark on source and target
7. Test using drone

## Process flow

1. Change source and / or target in configuration file
2. Commit initiates drone GH action
3. Pull source docker image and run independent benchmark - time benchmark with bencher
4. Stop and remove container
5. Pull target docker image and run independent benchmark - time benchmark with bencher
6. Stop and remove container
7. Start source and run first app - time with bencher
8. Run second app on source, start target and set up pgactive - time with bencher
9. Note when caught up (say less than 0.5s lag) - time with bencher
10. When second app is complete (and target is caught up) compare source and target (time with bencher), any disparity should be a failure
11. Clean up?

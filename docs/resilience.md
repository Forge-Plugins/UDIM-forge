# Fault testing and recovery

UDIM Forge 0.36 adds controlled resilience checks around real conversion and worker recovery. These checks operate exclusively on owned disposable fixture folders. No disk is ejected, dismounted, formatted, filled, disabled or reconfigured. No operating-system sleep or power action is triggered.

## Covered failures

- An upcoming copied input is deleted while a conversion is running. Completed files stay in the durable ledger; restoring the input permits resume without rewriting verified completed files.
- ENOSPC / disk-full and permission-denied errors are injected into fixture writes or free-space queries. No real drive is filled and real filesystem permissions are not changed.
- A child worker exits immediately after writing its durable completion marker and before publication. Reopening the queue recovers the prepared output from staging.
- Interrupted main, grading, resize, ORM and production tasks are copied with their outputs/staging, restored after directory relocation, and resumed with exact saved settings.
- Cancellation, modified completion hashes, source changes after review, altered transfer files, stale export approval and path escapes are checked.

These are controlled simulations and real child-process exits on the development host. They do not certify physical drive failure, power loss, sleep/hibernate behavior, another Windows installation or other hardware. Separate clean-machine and hardware acceptance remains required.

## When a real task stops

Open Tasks and read Details / failed files. Restore or relink missing inputs, correct access/space problems and use the operation's Resume or Retry action. Existing results are verified before reuse. Do not manually edit completion ledgers or commit records. For relocation use the reviewed input relinker or Copy whole task, then open the transferred task explicitly.

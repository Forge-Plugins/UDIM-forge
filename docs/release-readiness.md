# Commercial release checklist

## Implemented delivery controls

UDIM Forge provides project/session recovery, durable jobs, selected exact-setting retries, reviewed input relinking, export preflight, final output verification, local diagnostics and bounded cache controls. New encoded-output checks also report channel min/max, exact 0/1 percentages and tangent-space normal-length advisories during the existing streamed verification pass. Boundary counts alone are not proof of clipping; artistic intent and color/data interpretation still need review.

## Release verification

Each local release requires the unit suite, frozen runtime acceptance, a portable first-run profile with Python/Conda removed from PATH, real export/resume, installer validation and install/upgrade/uninstall checks. These run on the development Windows host. An isolated profile is not a clean Windows virtual machine and does not certify other GPUs or machines.

Before broad customer rollout, run the same checks on a separate supported Windows installation with no development tools; test standard-user installation, Unicode/long paths, offline use, disk-full/access-denied failures, malformed images/models, interrupted jobs and update rollback. Include an AMD/Intel GPU and a lower-memory machine. Keep results and known limitations per build. This environment does not currently supply a clean VM or those additional machines.

## Performance baselines

The local benchmark and memory panel already report synthetic processing times, GPU capabilities and the application/worker budget. Compare like-for-like inputs, storage, cache state, resolution and rendering settings. Release checks may also record timings and peak application-job memory on the owner's EXR/Smaug/HDRI fixtures. Measurements are host-specific; a timing alone is not an optimization or a general performance guarantee. Native import/decode/TX timeouts and cooperative cancellation remain enabled.

## Seller decisions still required

Gumroad is the selected purchase channel, but the product and Product ID have not been created. No public application download or GitHub Release is enabled. Create the paid product, then configure the real product identifier and authorized update delivery.

Finalize the commercial license (allowed seats, studio use, updates), support policy/contact, price and refund terms for the intended sales regions before publishing. These are business/legal choices; this checklist does not silently impose new customer terms. GitHub remains documentation/changelog/issues only. Code signing remains deferred at the owner's request; obtaining a signing certificate and integrating Authenticode is still a launch task.

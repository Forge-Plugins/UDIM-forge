# Licensing and updates

UDIM Forge is commercial software developed by Forge-Plugins. GitHub hosts documentation, changelog and issue tracking only. Application builds, keys and public GitHub release downloads are not provided there.

## Gumroad setup status

Gumroad is the selected store. The product has not been created yet; no Product ID or purchase link is configured. Version 0.14.1 prepares the connector and license UI, but activation is disabled until the publisher supplies the real Product ID in a new build. License enforcement remains disabled in this internal build. No product was created or uploaded by the application.

When creating the product, add a **License key** block to the paid product content. Gumroad displays the **Product ID** in that block while editing. Supply that public ID and the product page URL to Forge-Plugins development; do not share a seller API token or password. Product URL and Product ID are different values. See [Gumroad license keys documentation](https://gumroad.com/help/article/76-license-keys).

## In the application

Open **Help & Support > License / Gumroad**. Once connected, paste the purchase key and explicitly choose **Verify / recheck saved key**. Opening the dialog or starting UDIM Forge does not contact Gumroad. The request sends the Product ID and key over HTTPS to Gumroad's fixed verification endpoint; redirects are rejected and responses/timeouts are bounded. Seller API tokens are never embedded in the application.

Verification rejects another product, incomplete purchase status, refunds, chargebacks, unresolved disputes, a reported disabled/test license and memberships whose end date has passed. Verification uses `increment_uses_count=false`; repeated checks do not count as additional devices. This is purchase verification, not a seat-management system.

The local saved key is encrypted with Windows user protection (DPAPI), bound to the workstation and configured product, and excluded from projects/profiles. The license entry is masked. **Forget local key** removes that local record without revoking the purchase. A saved verification receipt is not a signed offline entitlement and is not used to gate this internal build. Network failure is distinct from an invalid key and retains the previous local record. Device limits, offline grace period and automatic renewal policy require a later commercial configuration.

Publisher-signed offline licenses remain available separately. These are different from Gumroad-generated keys.

## Obtaining paid updates

**My Gumroad library** opens the customer's account library in their browser. Customers without an account can use the **View content** link in their purchase receipt. Updated ZIP/MSI packages should be added to the paid product content after the publisher creates the product; the app does not infer or expose a public file URL. See [Gumroad purchase access](https://gumroad.com/help/article/199-how-do-i-access-my-purchase).

The existing private signed-manifest updater is separate. Gumroad purchase verification does not provide an authenticated automatic binary-update feed. No feed is configured, and nothing is downloaded or installed automatically. Keep previous packages for rollback, close UDIM Forge before replacing an executable, and back up projects before upgrades. Existing settings remain in their established local folders.

## Support

Use the [support guide](support.md). Never attach purchase keys, credentials or private project data to public issues. Diagnostic exports stay local until you share them.

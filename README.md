# CRA Tools 3.0

Android app with two workflows: **Compare & Explain** and **Simulate Adjustment**.

The existing Android wrapper, package (com.ionut.cratools), launcher icon, T1
line catalog, multiple-image intake and analysis concepts are retained.

Publication keeps the repository's existing archive workflow.
CRA_Tools_Android_Project.zip contains the editable Android project in
CRA_Tools_Android/ and the validation tools at the archive root.
GitHub Actions extracts this versioned archive before testing and building.

## Use

1. Choose one of the two cards and select tax year and jurisdiction.
2. Photograph or upload all needed return pages. Camera and multi-image import
   are separate actions. Bundled ML Kit Latin OCR runs locally.
3. Review and correct each line and amount, then mark it Verified.
   Conflicting pages never silently overwrite a reviewed value.
4. Compare returns, or enter one or more new line amounts and simulate.

**Fictional example** loads numbers without names, SINs or account identifiers.
All missing values remain missing; enter zero explicitly when applicable.

## Current official information

The **Official sources** panel retrieves current public references directly from
CRA/Canada.ca and Justice Canada over HTTPS. It shows access timestamps,
available page modification dates and legislation consolidation/amendment dates.
Open each source in the browser for complete current text. Current-year rates
and return-change pages are separate from the selected historical year.

Simulation requires online verification in the current session, at most 24 hours
old. Reviewed 2025 T1 form hashes must match, the top-up reference must still
identify the implemented year/rate/threshold, and legislation must match the
reviewed amendment version. Changed/unavailable sources stop numerical estimates
until the calculation rules are reviewed. Website prose is never executed as
code or automatically converted into tax rules. The application does not claim
to encode all legislation or to apply current-year rates to historical returns.

Native reference checks request fixed public URLs without taxpayer data.
WebView network requests are blocked; official native HTTPS requests validate
hosts and redirects. Offline comparison of confirmed document values is available.

## Calculation scope

Validated rules cover **selected 2025 federal T1 calculations**. This is not a
complete tax preparation engine. Other years and unmapped dependencies report
**dependency not yet mapped**. All money uses integer CAD cents.

- Observed differences, calculated consequences and possible explanations are
  distinct. A possible explanation is not CRA's confirmed reason.
- Multiple allowable federal credit-base changes can propagate through 33500,
  33800, 35000, 42900 and 42000 for the confirmed simple full-year case.
  The 2025 rate is 14.5%. Positive top-up/donation interactions, special taxes,
  foreign credits and minimum-tax cases stop the final estimate.
- Non-refundable reductions are capped at tax otherwise payable.
- The simple Quebec case includes the federal abatement at 16.5% of basic federal
  tax. Quebec provincial income tax remains separate.
- Outside Quebec, credit-change final results require a separately verified
  post-adjustment line 42800, including an explicitly supplied unchanged value.
  Provincial tax is never calculated or silently assumed unchanged.
- Accepted payment or total-payable/total-credit changes can be reconciled
  arithmetically to the federal T1 refund or balance.
- Selected income/deduction changes propagate to supported income totals and
  2025 gross federal bracket tax. Income-tested credits and other remaining
  dependencies are not mapped, so no final refund estimate is produced.
  Zero old benefit repayment alone does not prove zero revised repayment.
- Unknown lines are editable but have no invented calculation rule.
- Overlapping component/total edits and contradictory final amounts block the
  final calculation.
- Outcome means the federal return calculation, not the CRA account balance.
  Interest, penalties, offsets, later payments, benefits, carry-forwards and
  linked returns are outside the result.

Claim eligibility and limits must be verified before scenario entry. The scope
confirmation states a condition; it does not determine residency or eligibility.

## References

- [T1 form and prior years](https://www.canada.ca/en/revenue-agency/services/forms-publications/forms/5000-r.html)
- [2025 federal T1](https://www.canada.ca/content/dam/cra-arc/formspubs/pbg/5000-r/5000-r-25e.txt)
- [2025 Quebec federal T1](https://www.canada.ca/content/dam/cra-arc/formspubs/pbg/5005-r/5005-r-25e.txt)
- [Top-up tax credit](https://www.canada.ca/en/revenue-agency/services/tax/individuals/topics/about-your-tax-return/tax-return/completing-a-tax-return/deductions-credits-expenses/line-34990-top-up-tax-credit.html)
- [Quebec abatement](https://www.canada.ca/en/revenue-agency/services/tax/individuals/topics/about-your-tax-return/tax-return/completing-a-tax-return/deductions-credits-expenses/line-44000-refundable-quebec-abatement.html)
- [Current-year rates](https://www.canada.ca/en/revenue-agency/services/tax/individuals/tax-rates-brackets/current-year.html)
- [Income Tax Act](https://laws-lois.justice.gc.ca/eng/acts/I-3.3/)
- [Income Tax Regulations](https://laws-lois.justice.gc.ca/eng/regulations/C.R.C.,_c._945/)

Initial review: 2026-10-16. Reviewed legislation amendment date: 2026-06-18;
consolidation current to 2026-09-30. Runtime retrieves official versions again.

## Privacy

Amounts, imported photos and OCR evidence stay in memory during the session.
No database, browser storage, account login or return upload is implemented.
Private camera cache is cleared after import, session clearing, Activity
destruction and the next startup. Original photos in the user's gallery remain.
Android backup is disabled and recent-app previews are protected.
Activity/process recreation clears the working session instead of storing data.
Material 3 native theme and token-based WebView UI support light/dark modes.

## Build and validation

Requires JDK 17, Gradle 8.9 and Android SDK 35.

    npm ci
    npm test
    npx playwright install chromium
    npm run test:ui
    cd CRA_Tools_Android
    gradle assembleDebug lintDebug assembleDebugAndroidTest
    gradle connectedDebugAndroidTest

CI runs engine/source-guard scenarios, mobile/desktop UI tests, Android lint,
compilation, real bundled OCR on an emulator, and APK signature verification.
Return fixtures are fictional. Browser tests mock the native bridge and do not
establish real OCR accuracy or live reference availability; native tests exercise
ML Kit itself. Final debug-signed APK: **CRA-Tools-APK** workflow artifact.
Local output: CRA_Tools_Android/app/build/outputs/apk/debug/app-debug.apk.

An older APK signed with a different temporary debug key may require uninstalling
the previous app before installation.

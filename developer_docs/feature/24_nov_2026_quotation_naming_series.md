# Quotation Naming Series

## Requirement

Keep the Quotation naming series hard-coded to the calendar year 2026:

```text
SAL-QTN-2026-#####
```

Do not use the dynamic `YYYY` token. The series must remain on 2026 until the business changes it in March.

This requirement was agreed with Kishorbhai on 23 November 2026.

## March change

In March, change the naming series manually to the 2026-27 series:

```text
SAL-QTN-2627-#####
```

The 26-27 prefix will create a sequence separate from the earlier `SAL-QTN-2026-#####` numbering.

## Implementation

Frappe automatically appends the five-digit counter to a Naming Series field. Therefore, the Property Setter stores only this prefix:

```text
SAL-QTN-2026-
```

The customization is maintained in the Sbargon app through:

- `sbargon/fixtures/property_setter.json`
- The filtered `Property Setter` fixture configuration in `sbargon/hooks.py`

The fixture changes the `options` property of the `Quotation.naming_series` field.

## Data-safety rule

This change applies only when selecting the series for new Quotations. It must not rename, renumber, or otherwise update any existing Quotation.

Saved Quotations retain their original names and stored naming-series values. Opening or refreshing a saved Quotation must not mark it as unsaved merely because the available series option changed.

## Production deployment

Confirm that the `sbargon` app is installed on the production site, then pull and migrate:

```bash
cd /path/to/frappe-bench/apps/sbargon
git pull origin version-16

cd /path/to/frappe-bench
bench --site <production-site> list-apps | grep sbargon
bench --site <production-site> migrate
```

No asset build is required for this metadata-only fixture.

## Verification

After migration:

1. Open a new Quotation.
2. Confirm that its Series field shows `SAL-QTN-2026-`.
3. Save the Quotation and confirm its name follows `SAL-QTN-2026-#####`.
4. Open an older Quotation and confirm its existing name is unchanged.
5. Refresh the older Quotation and confirm it does not show `Not Saved` without a user edit.


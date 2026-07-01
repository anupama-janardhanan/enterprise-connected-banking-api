---
title: Beneficiary Management
deprecated: false
hidden: false
metadata:
  robots: index
---
> **Scope:** Beneficiary addition is currently **only required and available for AXIS Bank RIB (Retail Internet Banking) accounts**. It is **mandatory** to add a beneficiary before initiating a fund transfer from an AXIS Bank RIB debit account — payouts from AXIS RIB accounts cannot be made to ad-hoc (non-onboarded) account numbers.
>
> **AXIS CIB (NFC — Neo for Corporates) accounts do not require beneficiary pre-registration.** For AXIS CIB accounts, pass `vendor_name`, `bank_account_number`, and `ifsc` directly in the fund transfer request (same as non-AXIS banks).
>
> For all other partner banks, beneficiary addition is neither required nor supported; payouts can be initiated directly by passing `vendor_name`, `bank_account_number`, and `ifsc` in the fund transfer request.

<br />

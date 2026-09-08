# Key Edge Cases

The following examples represent key alternative and error scenarios. The list is intentionally non-exhaustive for this mock case.

### 1. Failed age verification

If Company X does not receive a successful age-verification result from Diia, no OTP is registered and parcel release is not permitted.

### 2. Invalid or expired OTP

If the entered code does not match an active OTP for the parcel or its TTL has expired, the locker compartment remains closed and parcel release is denied.

Additional error and recovery scenarios should be refined before the functionality is handed over for development.

# Assumptions and Open Questions

## Assumptions

The proposed solution is based on the following assumptions:

1. **Company X is the source of truth for the age-verification result** and generates an OTP only after successful verification through Diia.

2. **The parcel locker has online connectivity to Our System Backend** when the OTP is validated.

## Open Questions

Before development, the following questions should be clarified, among others:

1. **What OTP lifecycle policy should be applied:** code format, TTL, allowed failed attempts, and regeneration rules?

The list is intentionally non-exhaustive. Additional business and technical questions should be addressed during integration refinement.

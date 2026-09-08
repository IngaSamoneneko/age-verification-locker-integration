# Code Validation Integration Schema

The proposed solution uses a **Pre-registered OTP Validation** mechanism.

```text
                    Successful age verification
                                │
                                ▼
                           Company X
                             Backend
                                │
                                │  Generate OTP
                                │
                                │  Register OTP
                                ▼
                        Our System
                          Backend
                                │
                      Store protected OTP,
                        TTL and status
                                │
                                │  Registration confirmed
                                ▼
                           Company X
                       Mobile Application
                                │
                                │  Display OTP
                                ▼
                              User
                                │
                                │  Enter OTP
                                ▼
                       Parcel Locker
                          Software
                                │
                                │  OTP validation request
                                ▼
                        Our System
                          Backend
                          ┌─────┴─────┐
                          │           │
                         Valid      Invalid
                          │           │
                          ▼           ▼
                    Mark OTP as    Deny parcel
                       used          release
                          │
                          ▼
                    Authorize locker
                    compartment opening

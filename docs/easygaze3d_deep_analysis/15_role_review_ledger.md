# Role Review Ledger (Pass 2)

| Role | What changed after pass 2? | Accepts? | Rejects/Questions | Implementation Constraints |
|---|---|---|---|---|
| User Intention Enforcer | Verified W and C isolation | Yes | None | Must expose both W and C |
| Math Enforcer | Verified missing normalization on average | Yes | Warns about returning np.zeros | Must normalize separated vectors |
| Red Team Reviewer | Mapped the `np.zeros` silent failure risk | Yes | Zeros must not bypass validation | Add `frame_valid` flag |
| Coordinate System Reviewer | Defined W as head-relative explicitly | Yes | User misuse of C vs W | Add explicit coordinate metadata |
| Signal Processing Reviewer | Verified API is unsmoothed natively | Yes | None | Do not touch `demo_webcam.py` smoothing |
# ⭐ **STRIDE — Reproduction Protocol**

**Structural Robotic Intention and Deterministic Execution**

**Reproducing the STRIDE v1.1.3 Reference Result**

---

# **1. Purpose**

This protocol defines the steps required to reproduce the STRIDE v1.1.3 reference behavior.

It covers:

- file integrity
- local launch
- version verification
- release audit
- scenario verification
- motion verification
- pause verification
- completion verification
- 346-case qualification
- evidence comparison

---

# **2. Repository Layout**

The expected repository structure is:

```text
STRIDE/
│
│   README.md
│   LICENSE
│
├── demo
│   └── STRIDE_v1_1_3.html
│
├── docs
│   ├── FAQ.md
│   ├── Quickstart.md
│   ├── STRIDE-Architecture-Notes.md
│   ├── STRIDE-Challenge.md
│   ├── STRIDE-Motion-Contract.md
│   ├── STRIDE-Runtime-State-Model.md
│   ├── STRIDE-Resolution-Guarantees.md
│   ├── STRIDE-Claim-Boundary.md
│   ├── STRIDE-Known-Limitations.md
│   ├── STRIDE-Reproduction-Protocol.md
│   ├── STRIDE-Diagram.png
│   └── Shunyaya-Structural-Stack.png
│
├── evidence
│   ├── STRIDE_v1_1_3_RELEASE_MANIFEST.json
│   ├── STRIDE_v1_1_3_346_Case_Qualification_Report.json
│   ├── QUALIFICATION_SUMMARY.md
│   └── EXPECTED_RESULTS.json
│
└── VERIFY
    ├── FREEZE_DEMO_SHA256.txt
    ├── VERIFY.txt
    └── RELEASE_CHECKLIST.md
```

---

# **3. Minimum Requirements**

Required:

- current desktop browser
- JavaScript enabled

Optional:

- Python 3 for a local static server

No package manager is required.

No build step is required.

No network connection is required after download.

---

# **4. Freeze the Test Environment**

Before verification:

1. Place all repository files in their declared locations.
2. Do not edit `demo/STRIDE_v1_1_3.html`.
3. Close other copies of the observatory.
4. Use a current desktop browser.
5. Disable browser extensions that modify page content where practical.
6. Use a fresh browser tab.
7. Record the browser name and version.

---

# **5. Verify the HTML Checksum**

From the repository root:

## **Windows**

```bat
certutil -hashfile demo\STRIDE_v1_1_3.html SHA256
```

## **macOS / Linux**

```bash
sha256sum demo/STRIDE_v1_1_3.html
```

Compare the result with:

`VERIFY/FREEZE_DEMO_SHA256.txt`

Expected:

- the calculated hash matches the frozen hash
- the verified file is exactly `demo/STRIDE_v1_1_3.html`

If the hash differs, stop the reproduction process.

---

# **6. Start the Local Server**

From the repository root:

## **Windows**

```bat
python -m http.server 8000
```

## **macOS / Linux**

```bash
python3 -m http.server 8000
```

Open:

`http://localhost:8000/demo/STRIDE_v1_1_3.html`

Direct `file://` opening may work.

The local server path is recommended.

---

# **7. Confirm Initial Load**

After opening the observatory, confirm:

- the header shows `STRIDE v1.1.3`
- the operational runtime is visible
- the action controls are visible
- the initial admission state is visible
- the interface remains responsive
- the verification laboratory is dormant
- the qualification worker has not loaded automatically

Expected runtime rule:

`runtime_does_not_load_verification_until_requested`

---

# **8. Verify the Version**

Open the browser developer console.

Run:

```javascript
STRIDE.version()
```

Expected:

`1.1.3`

---

# **9. Verify Release Status**

Run:

```javascript
STRIDE.release()
```

Confirm that the returned object identifies:

- version `1.1.3`
- reference architecture status
- declared claim boundary
- declared qualification suite

---

# **10. Run the Release Audit**

Run:

```javascript
STRIDE.releaseAudit()
```

Expected:

`allPass = true`

Record the returned result.

---

# **11. Run the Interface Audit**

Run:

```javascript
STRIDE.uiAudit()
```

Expected:

`allPass = true`

Confirm that required interface elements are present.

---

# **12. Run the Core Audit**

Run:

```javascript
STRIDE.coreAudit()
```

Expected:

`allPass = true`

Confirm that the deterministic core is available.

---

# **13. Run the Performance Audit**

Run:

```javascript
STRIDE.performanceAudit()
```

Expected:

`allPass = true`

Confirm that verification remains unloaded before qualification is requested.

---

# **14. Run the Geometry Audit**

Run:

```javascript
STRIDE.geometryStatus()
```

Expected:

`allPass = true`

Confirm:

- robot and objects are in valid initial positions
- gripper and target geometry are coherent
- placement geometry is coherent

---

# **15. Run the Viewport Audit**

Run:

```javascript
STRIDE.viewportAudit()
```

Expected:

`allPass = true`

Confirm that the runtime and controls remain visible and usable.

---

# **16. Reproduce the Ambiguous Scenario**

1. Press **AMBIGUOUS**.
2. Press **EVALUATE**.

Expected:

`INCOMPLETE`

Expected reason:

- target is not uniquely resolved
- no Motion Contract exists
- no authorized motion begins

Invariant:

`target_not_unique -> no_target_specific_authority`

---

# **17. Reproduce the Conflict Scenario**

1. Press **CONFLICT**.
2. Press **EVALUATE**.

Expected:

`CONFLICT`

Expected result:

- required observations disagree
- no Motion Contract exists
- no authorized motion begins

Invariant:

`conflicting_required_observations -> CONFLICT`

---

# **18. Reproduce the Human-Zone Scenario**

1. Press **HUMAN ZONE**.
2. Press **EVALUATE**.

Expected:

`FORBIDDEN`

Expected result:

- protected human zone is occupied
- no Motion Contract exists
- no authorized motion begins

Invariant:

`human_in_protected_zone -> no_motion_authority`

---

# **19. Reproduce the Forbidden-Contact Scenario**

1. Press **FORBIDDEN**.
2. Press **EVALUATE**.

Expected:

`FORBIDDEN`

Expected result:

- contact is not permitted
- no Motion Contract exists
- no authorized motion begins

Invariant:

`reachable != contact_admissible`

---

# **20. Reproduce the Excess-Force Scenario**

1. Press **EXCESS FORCE**.
2. Press **EVALUATE**.

Expected:

`FORBIDDEN`

Expected result:

- requested force exceeds the permitted force
- no Motion Contract exists
- no authorized motion begins

Invariant:

`requested_force > permitted_force -> FORBIDDEN`

---

# **21. Reproduce the Resolved Scenario**

1. Press **RESOLVED**.
2. Press **EVALUATE**.

Expected:

`RESOLVED`

Expected result:

- target is unique
- path is clear
- protected human zone is clear
- contact is allowed
- requested force is within limit
- recovery is defined
- a bounded Motion Contract is available

---

# **22. Reproduce Complete Execution**

After the resolved evaluation:

1. Press **EXECUTE**.
2. Observe the runtime phases.

Expected sequence:

`APPROACH -> CONTACT -> LIFT -> TRANSFER -> PLACE -> RETURN -> COMPLETED`

Expected visual behavior:

- robot approaches the selected object
- gripper reaches the target
- object attaches
- object lifts
- object transfers with the gripper
- object enters Zone B
- object releases
- robot returns home
- runtime reaches `COMPLETED`

---

# **23. Verify Completion Cleanup**

After completion, confirm:

- the object remains at the destination
- the gripper is no longer carrying the object
- the robot is home
- no active motion phase remains
- no active scheduler remains

Invariant:

`COMPLETED -> no_active_scheduler`

---

# **24. Reproduce Pause Behavior**

1. Reset the observatory.
2. Press **RESOLVED**.
3. Press **EVALUATE**.
4. Press **EXECUTE**.
5. Press **PAUSE** during movement.

Confirm:

* the phase displays **MOTION PAUSED**
* the control changes from **PAUSE** to **RESUME**
* motion animation stops
* phase progression stops
* carried-object state remains coherent
* reset remains available
* the interface remains responsive

Run:

```javascript
STRIDE.runtimeStatus()
```

Confirm:

`paused = true`

`animation_active = false`

`phase_timer_active = false`

---

# **25. Reproduce Resume Revalidation**

While paused:

1. Change a mandatory condition to an invalid value, such as setting **HUMAN ZONE** to **Occupied**.
2. Press **RESUME**.
3. Confirm that motion does not resume.
4. Confirm that the runtime remains paused.
5. Restore the mandatory condition, such as setting **HUMAN ZONE** to **Clear**.
6. Press **RESUME** again.
7. Confirm that STRIDE revalidates the current structure automatically.
8. Confirm that motion resumes only when the structure is again `RESOLVED`.

Expected:

`resume_request + invalid_structure -> remain_paused`

and:

`PAUSED + renewed_validity -> RESUME`

A separate manual **EVALUATE** step is not required before pressing **RESUME**.

---

# **26. Generate a Certificate**

Use the certificate control or the relevant API.

Confirm that:

- a deterministic evidence object is produced
- the certificate identifies the admission basis
- equivalent structure produces equivalent identity

Invariant:

`same evidence -> same certificate identity`

---

# **27. Run the Full Qualification**

Press:

**RUN 346-CASE AUDIT**

Or run:

```javascript
(async () => {
  const report = await STRIDE.audit();
  console.log(report);
})();
```

Wait for completion.

Expected:

`allPass = true`

`total_case_count = 346`

`unsafe_admissions = 0`

---

# **28. Record Qualification Identity**

Confirm:

`runtime_release_version = 1.1.3`

`inherited_kernel_version = 1.0.1`

`qualification_suite_id = stride.qualification/346`

Record the elapsed time.

Elapsed time may vary by hardware and browser.

Admission truth must not vary.

---

# **29. Download the Qualification Report**

After qualification completes:

1. Press **DOWNLOAD REPORT**.
2. Save the generated JSON.
3. Compare it with:

`evidence/STRIDE_v1_1_3_346_Case_Qualification_Report.json`

Compare:

- release version
- kernel version
- suite identifier
- total case count
- pass state
- unsafe admissions
- family totals

Timing fields may differ.

Structural outcomes should match.

---

# **30. Compare Expected Results**

Review:

`evidence/EXPECTED_RESULTS.json`

Confirm that the observed results match the declared expected values.

---

# **31. Review the Qualification Summary**

Review:

`evidence/QUALIFICATION_SUMMARY.md`

Confirm that the reproduced result is consistent with the frozen summary.

---

# **32. Review the Release Manifest**

Review:

`evidence/STRIDE_v1_1_3_RELEASE_MANIFEST.json`

Confirm:

- version identity
- demo filename
- qualification suite
- claim boundary
- artifact references
- release fingerprint

---

# **33. Determinism Check**

Repeat the resolved evaluation.

Expected:

`same structure + same policy -> same admission`

Repeat certificate generation.

Expected:

`same normalized evidence -> same certificate identity`

Repeat qualification.

Expected:

- same case count
- same pass result
- same unsafe-admission result

Elapsed time may differ.

---

# **34. Negative Reproduction Check**

Attempt:

* execute while `INCOMPLETE`
* execute while `CONFLICT`
* execute while `FORBIDDEN`
* resume while mandatory structure remains invalid
* alter required structure during execution
* confirm that a completed runtime retains no active scheduler
* run the qualification suite to verify expiration, replay, and invalid-authority cases

Expected:

* no unauthorized motion
* no Motion Contract for non-resolved structure
* continuing authority is withdrawn when required structure becomes invalid
* invalid resume requests remain paused
* completed execution retains no active scheduler
* tested expired, replayed, and invalid authority cases are rejected

Note:

Pressing **EXECUTE** after completion performs a fresh structural evaluation in the reference observatory.

It is not a direct replay of the completed Motion Contract.

---

# **35. Reproduction Success Criteria**

Reproduction succeeds when:

- checksum matches
- version is `1.1.3`
- release audit passes
- UI audit passes
- core audit passes
- performance audit passes
- geometry audit passes
- viewport audit passes
- ambiguous scenario is not admitted
- conflict scenario is not admitted
- human-zone scenario is not admitted
- forbidden-contact scenario is not admitted
- excessive-force scenario is not admitted
- resolved scenario completes
- pause stops execution
- resume requires revalidation
- completion clears active scheduling
- 346-case qualification passes
- unsafe admissions remain zero

---

# **36. Reproduction Failure Conditions**

Reproduction fails if:

- checksum differs
- version differs
- a release audit gate fails
- a non-resolved scenario creates a Motion Contract
- an unsafe scenario is admitted
- pause does not stop active execution
- resume occurs without renewed validity
- completion leaves active scheduling
- qualification does not reach 346 cases
- unsafe admissions are greater than zero
- equivalent structure produces inconsistent admission

---

# **37. Reporting a Reproduction Result**

A reproduction record should include:

- operating system
- browser and version
- repository revision
- HTML checksum
- STRIDE version
- audit results
- qualification result
- unsafe-admission count
- elapsed time
- observed deviations

Do not represent successful reproduction as physical safety certification.

---

# **38. Claim Boundary**

Successful reproduction establishes that the bounded browser reference behavior was reproduced.

It does not establish:

- physical robot validation
- actuator-level enforcement
- certified functional safety
- secure production cryptography
- regulatory approval
- production deployment readiness

Governing boundary:

`reference_architecture_validated != production_safety_certified`

---

# **39. Final Reproduction Invariant**

`same structure + same policy -> same admission`

`non_resolved_structure -> no_motion_contract`

`no_valid_motion_contract -> no_authorized_motion`

`total_case_count = 346`

`unsafe_admissions = 0`

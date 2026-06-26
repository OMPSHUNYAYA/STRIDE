# ⭐ **STRIDE — Quickstart**

## **Structural Robotic Intention and Deterministic Execution**

**Deterministic • Structure-Based • Bounded Motion Authority • Runtime Revalidation • Replay-Verifiable Evidence**

A self-contained browser reference implementation for structural robotic authority.

**Intelligence may propose.**

**Structure must permit.**

`intention != execution_permission`

---

## ⚡ **30-Second Start**

From the repository root, start a local server.

**Windows**

```bat
python -m http.server 8000
```

**macOS / Linux**

```bash
python3 -m http.server 8000
```

Open:

`http://localhost:8000/demo/STRIDE_v1_1_3.html`

The observatory runs fully offline after download.

---

## 🖥️ **Direct Browser Opening**

The reference application is contained in:

[Open STRIDE v1.1.3](../demo/STRIDE_v1_1_3.html)

Direct `file://` opening may work in current desktop browsers.

A local server is recommended because some browsers restrict local-file behavior.

---

## 🔍 **What to Observe**

The observatory demonstrates:

- explicit robotic intention
- deterministic structural admission
- ambiguous-target refusal
- sensor-conflict refusal
- human-zone protection
- contact authorization
- force-limit enforcement
- bounded Motion Contracts
- runtime pause
- structural revalidation
- object transfer and placement
- completion evidence
- on-demand qualification

Core sequence:

`proposal -> structural_admission -> motion_contract -> protected_execution -> evidence`

---

## 🧪 **60-Second Structural Proof**

1. Open the observatory.
2. Select **RESOLVED**.
3. Press **EVALUATE**.
4. Confirm the admission state is `RESOLVED`.
5. Press **EXECUTE**.
6. Observe the complete transfer sequence.
7. Confirm the final state is `COMPLETED`.
8. Press **VERIFY 346** or **RUN 346-CASE AUDIT**.
9. Confirm all qualification cases pass.
10. Confirm unsafe admissions remain zero.

Expected qualification result:

`allPass = true`

`total_case_count = 346`

`unsafe_admissions = 0`

---

## 🧭 **Core Structural Behavior**

| Condition                           | Result       |
| ----------------------------------- | ------------ |
| complete and admissible structure   | `RESOLVED`   |
| missing or ambiguous structure      | `INCOMPLETE` |
| conflicting mandatory evidence      | `CONFLICT`   |
| explicit policy violation           | `FORBIDDEN`  |
| recoverable runtime validity loss   | `PAUSED`     |
| unrecoverable runtime validity loss | `ABORTED`    |
| completion condition satisfied      | `COMPLETED`  |

Core invariants:

`non_resolved_structure -> no_motion_contract`

`authority_validity_lost -> motion_authority_withdrawn`

---

## 🎯 **Try the Included Scenarios**

### **AMBIGUOUS**

Two objects match the requested target.

Expected:

`INCOMPLETE`

`target_not_unique -> no_target_specific_authority`

---

### **CONFLICT**

Required observations disagree.

Expected:

`CONFLICT`

`conflicting_required_observations -> CONFLICT`

---

### **HUMAN ZONE**

A protected human zone is occupied.

Expected:

`FORBIDDEN`

`human_in_protected_zone -> no_motion_authority`

---

### **FORBIDDEN**

The requested contact is not permitted.

Expected:

`FORBIDDEN`

`reachable != contact_admissible`

---

### **EXCESS FORCE**

Requested force exceeds the policy limit.

Expected:

`FORBIDDEN`

`requested_force > permitted_force -> FORBIDDEN`

---

### **RESOLVED**

The action, world, policy, and recovery structure are admissible.

Expected:

`RESOLVED`

A bounded Motion Contract becomes available.

---

## ⏸️ **Pause and Revalidation Test**

1. Select **RESOLVED**.
2. Press **EVALUATE**.
3. Press **EXECUTE**.
4. Press **PAUSE** during movement.
5. Confirm active motion stops.
6. Confirm the phase displays **MOTION PAUSED**.
7. Confirm the control changes from **PAUSE** to **RESUME**.
8. Press **RESUME**.
9. Confirm STRIDE revalidates the current structure before continuing.
10. Confirm motion resumes only when the structure remains `RESOLVED`.

Core laws:

`previous_permission != permanent_permission`

`PAUSED + renewed_validity -> RESUME`

`resume_request + invalid_structure -> remain_paused`

---

## ✅ **Completion Test**

A successful resolved action should follow:

`APPROACH -> CONTACT -> LIFT -> TRANSFER -> PLACE -> RETURN -> COMPLETED`

Expected observations:

- the selected object is approached
- contact occurs
- the object becomes attached
- the object is lifted
- the object is transferred
- the object is placed in the destination
- the gripper releases the object
- the robot returns home
- active scheduling ends

Completion invariant:

`COMPLETED -> no_active_scheduler`

---

## 🔎 **Browser Console Verification**

Open the browser developer console.

Check the version:

```javascript
STRIDE.version()
```

Expected:

`1.1.3`

Check release identity:

```javascript
STRIDE.release()
```

Check the embedded release audit:

```javascript
STRIDE.releaseAudit()
```

Expected:

`allPass = true`

Check runtime status:

```javascript
STRIDE.runtimeStatus()
```

Check the user interface:

```javascript
STRIDE.uiAudit()
```

Check the structural core:

```javascript
STRIDE.coreAudit()
```

Check performance:

```javascript
STRIDE.performanceAudit()
```

Check geometry:

```javascript
STRIDE.geometryStatus()
```

Check viewport integrity:

```javascript
STRIDE.viewportAudit()
```

---

## 🧪 **Run the Full Qualification**

In the observatory, press:

**RUN 346-CASE AUDIT**

Or run from the browser console:

```javascript
(async () => {
  const report = await STRIDE.audit();
  console.table({
    runtime_release_version: report.runtime_release_version,
    inherited_kernel_version: report.inherited_kernel_version,
    qualification_suite_id: report.qualification_suite_id,
    allPass: report.allPass,
    total_case_count: report.total_case_count,
    unsafe_admissions: report.unsafe_admissions
  });
})();
```

Expected:

`runtime_release_version = 1.1.3`

`inherited_kernel_version = 1.0.1`

`qualification_suite_id = stride.qualification/346`

`allPass = true`

`total_case_count = 346`

`unsafe_admissions = 0`

The qualification worker is created only when requested.

Core runtime rule:

`runtime_does_not_load_verification_until_requested`

---

## 🔐 **Verify the Frozen HTML**

From the repository root:

**Windows**

```bat
certutil -hashfile demo\STRIDE_v1_1_3.html SHA256
```

**macOS / Linux**

```bash
sha256sum demo/STRIDE_v1_1_3.html
```

Expected SHA-256:

`4550091e1da5e3e6fcf54f9d641d46ca1b9200045e0f221d60cf415de633049f`

The calculated value must match exactly.

Compare the result with:

[Frozen Demo SHA-256](../VERIFY/FREEZE_DEMO_SHA256.txt)

Complete verification instructions:

[STRIDE Verification Procedure](../VERIFY/VERIFY.txt)

---

## ⚙️ **Minimum Requirements**

* current desktop browser
* JavaScript enabled
* no build system
* no package manager
* no external runtime dependency
* no external network connection after download

Python 3 is optional.

It is used only to start a local static server.

---

## 📁 **Repository Structure**

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

## 📚 **Documentation**

### **Core Understanding**

- [FAQ](FAQ.md)
- [STRIDE Challenge](STRIDE-Challenge.md)
- [Architecture Notes](STRIDE-Architecture-Notes.md)
- [Resolution Guarantees](STRIDE-Resolution-Guarantees.md)

### **Structural Model**

- [Motion Contract](STRIDE-Motion-Contract.md)
- [Runtime State Model](STRIDE-Runtime-State-Model.md)

### **Release Boundary**

- [Claim Boundary](STRIDE-Claim-Boundary.md)
- [Known Limitations](STRIDE-Known-Limitations.md)
- [Reproduction Protocol](STRIDE-Reproduction-Protocol.md)

### **Visual References**

- [STRIDE Architecture Diagram](STRIDE-Diagram.png)
- [Shunyaya Structural Stack](Shunyaya-Structural-Stack.png)

---

## 📊 **Evidence**

- [Release Manifest](../evidence/STRIDE_v1_1_3_RELEASE_MANIFEST.json)
- [346-Case Qualification Report](../evidence/STRIDE_v1_1_3_346_Case_Qualification_Report.json)
- [Qualification Summary](../evidence/QUALIFICATION_SUMMARY.md)
- [Expected Results](../evidence/EXPECTED_RESULTS.json)

The evidence directory contains frozen reference artifacts.

Independent reproduction remains recommended.

---

## 🔐 **Deterministic Guarantees**

`same structure + same policy -> same admission`

`non_resolved_structure -> no_motion_contract`

`no_valid_motion_contract -> no_authorized_motion`

`human_in_protected_zone -> no_continuing_motion`

`requested_force > permitted_force -> FORBIDDEN`

`motion_admitted -> safe_stop_or_recovery_defined`

`world_change -> authority_revalidation`

`optimization_must_not_change_admission_truth`

---

## 🚫 **What STRIDE Does Not Do**

STRIDE does not:

- replace robot controllers
- replace certified safety functions
- replace emergency stops
- replace physical guarding
- provide real actuator isolation
- provide measured stopping-distance evidence
- provide regulatory approval
- provide production deployment authority
- establish universal physical safety

---

## ✅ **What STRIDE Demonstrates**

STRIDE demonstrates:

- explicit robotic intention
- deterministic admission
- bounded physical authority
- valid non-movement
- human-zone protection
- contact and force governance
- runtime authority withdrawal
- structural revalidation
- simulated object transfer
- replay-verifiable evidence
- reproducible browser qualification

---

## ⚠️ **Claim Boundary**

Current maturity:

`reference_architecture_validated`

It is not:

`production_safety_certified`

Governing boundary:

`reference_architecture_validated != production_safety_certified`

---

## 🧠 **Core Insight**

A robot may understand.

A robot may plan.

A robot may be capable.

The robot may remain unauthorized to move.

**Capability is not permission.**

---

## ⭐ **One-Line Summary**

STRIDE explores whether robotic motion can be authorized through deterministic structural admission rather than directly through intelligence, commands, or capability.

---

## 🔥 **Final Line**

Intelligence may propose.

Structure must permit.

**Absence of permission means absence of motion.**

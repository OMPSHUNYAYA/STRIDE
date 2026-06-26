# ⭐ **STRIDE — Motion Contract**

**Structural Robotic Intention and Deterministic Execution**

**Bounded Physical Authority After Structural Admission**

---

# **1. Purpose**

The STRIDE Motion Contract defines the exact physical authority granted after a robotic action has been structurally admitted.

A Motion Contract is not a general permission to move.

It is a bounded authorization tied to:

- one robot
- one action
- one target
- one path
- one policy
- one operating context
- one validity period
- one declared completion condition

Core rule:

`motion_allowed iff valid_motion_contract_exists`

---

# **2. Architectural Position**

The Motion Contract sits between structural admission and execution.

`proposal -> structural_admission -> motion_contract -> protected_execution -> controller -> evidence`

The proposal describes intention.

Structural admission evaluates whether the intention is complete, consistent, permitted, and recoverable.

The Motion Contract converts a resolved admission into bounded physical authority.

The controller executes only within that authority.

---

# **3. Contract Creation**

A Motion Contract may be created only when the initial admission result is:

`admission = RESOLVED`

A contract must not be created when the initial admission result is:

* `INCOMPLETE`
* `CONFLICT`
* `FORBIDDEN`

Core invariant:

`non_resolved_structure -> no_motion_contract`

`PAUSED` and `ABORTED` are runtime states associated with an existing Motion Contract.

`COMPLETED` is a terminal state.

An aborted or completed contract cannot silently create renewed authority.

A new proposal and admission cycle may be required.

---

# **4. Contract Scope**

A Motion Contract may define:

- contract identifier
- schema version
- robot identity
- action identity
- action type
- target identity
- source location
- destination location
- authorized path
- workspace boundary
- contact authority
- force limit
- speed limit
- acceleration limit
- protected-zone condition
- required sensors
- evidence freshness requirements
- pause conditions
- abort conditions
- recovery action
- completion condition
- expiration condition
- policy identity
- action-capsule fingerprint
- world-snapshot fingerprint
- contract fingerprint

Core law:

`permission = exact_action + exact_limits + exact_context`

---

# **5. Bounded Authority**

The Motion Contract grants only the authority explicitly represented within it.

It does not grant:

- unrestricted movement
- general controller access
- authority over unrelated objects
- authority outside the declared workspace
- authority beyond declared physical limits
- authority after expiration
- authority after withdrawal
- authority after completion
- authority after replay

Core invariant:

`bounded_authority != general_authority`

---

# **6. Contract Identity**

Contract identity should be derived from normalized structural content.

A simplified model is:

`normalized_contract = normalize(contract)`

`contract_fingerprint = hash(normalized_contract)`

Equivalent contracts should produce equivalent deterministic identities within a conforming implementation.

`same normalized contract -> same contract fingerprint`

The reference fingerprints support deterministic comparison.

They are not represented as secure production signatures.

---

# **7. Required Structural Linkage**

A Motion Contract should identify the structures that produced it.

These include:

- Structural Action Capsule
- World Snapshot
- Policy Structure
- Runtime Structure
- Admission result

This linkage supports:

- replay
- auditability
- change detection
- independent verification
- evidence reconstruction

A contract should not exist independently of its admission basis.

---

# **8. Action Binding**

The contract must identify the exact action being authorized.

Examples include:

- move
- approach
- grasp
- release
- place
- retreat
- return home

A contract for one action must not silently authorize another.

`authorized_action_A != authority_for_action_B`

---

# **9. Robot Binding**

The contract must identify the robot or execution endpoint.

A contract for one robot must not be transferable to another robot unless the transfer is explicitly declared and independently admitted.

`contract_robot_A != authority_for_robot_B`

---

# **10. Target Binding**

The contract must identify the exact target.

A class-level description is insufficient when multiple objects satisfy the same description.

Example:

- target class: red cup
- observed objects: Cup A and Cup B

Result:

`target_not_unique -> no_target_specific_contract`

---

# **11. Path Binding**

The contract must identify the permitted path or bounded path family.

Path authority may include:

- trajectory identity
- permitted corridor
- workspace limits
- obstacle exclusions
- protected-zone exclusions
- safe stopping points
- recovery route

Core distinction:

`path_exists != path_admissible`

A different path requires explicit structural evaluation.

---

# **12. Contact Binding**

Contact authority must be explicit.

A contract may define:

- whether contact is permitted
- permitted contact surface
- permitted tool
- permitted grasp type
- permitted force
- permitted duration
- release conditions

Core invariant:

`reachable != contact_admissible`

---

# **13. Force, Speed, and Acceleration Limits**

A Motion Contract should define relevant physical limits.

Examples:

- maximum contact force
- maximum translational speed
- maximum rotational speed
- maximum acceleration
- maximum deceleration
- maximum joint velocity
- maximum contact duration

Core law:

`requested_force > permitted_force -> FORBIDDEN`

The controller must not exceed the contract limits.

---

# **14. Human-Zone Conditions**

The contract should identify protected human zones and the conditions under which motion remains valid.

Core invariant:

`human_in_protected_zone -> no_continuing_motion`

If a protected zone becomes occupied:

`EXECUTING -> PAUSED`

The contract does not remain permanently valid merely because execution already began.

---

# **15. Sensor and Evidence Requirements**

The contract may require specific evidence to remain valid.

Examples:

* vision available
* proximity sensing available
* target still present
* path still clear
* controller state valid
* gripper state valid
* observations within freshness limits

Before admission:

`expired_required_evidence -> INCOMPLETE`

During execution:

`runtime_evidence_expired -> no_continuing_authority`

---

# **16. Validity Period**

A Motion Contract must have a declared validity boundary.

The boundary may be based on:

* creation time
* start deadline
* maximum execution duration
* phase deadline
* evidence freshness
* world-state fingerprint
* controller session identity

Core invariant:

`expired_contract -> no_continuing_authority`

Expiration withdraws authority even when execution previously began.

---

# **17. Pause Conditions**

Pause conditions define when execution authority must be temporarily withdrawn while recovery remains possible within the existing contract.

Examples:

* human-zone entry
* new obstacle
* sensor disagreement
* sensor loss
* stale evidence
* controller-state change
* communication loss
* path invalidation
* recoverable limit deviation

Transition:

`EXECUTING + recoverable_invalidated_condition -> PAUSED`

Pausing withdraws continuing motion authority.

It does not destroy the contract when valid recovery and revalidation remain possible.

---

# **18. Resume Conditions**

Resume requires renewed structural validity.

`PAUSED + renewed_validity -> RESUME`

A resume request, time delay, operator instruction, or apparently restored condition does not recreate authority by itself.

`resume_request + invalid_structure -> remain_paused`

An aborted or completed contract cannot resume.

---

# **19. Abort Conditions**

Abort conditions define when the existing contract can no longer support safe continuation or recovery.

Examples:

* unrecoverable controller failure
* invalid robot identity
* unrecoverable path loss
* contract integrity failure
* recovery unavailable
* persistent protected-zone violation
* physical state outside contract bounds

Transition:

`unrecoverable_runtime_change -> ABORTED`

An aborted contract must not silently regain authority.

A new proposal and admission cycle may be required.

---

# **20. Recovery Authority**

Recovery authority must remain bounded.

A contract may permit:

- controlled stop
- safe object release
- retreat
- return home
- movement to a safe pose
- restoration of a stable state

Core law:

`motion_admitted -> safe_stop_or_recovery_defined`

Recovery does not grant unrestricted movement.

---

# **21. Completion Condition**

The contract must declare what constitutes completion.

Examples:

- target placed within destination zone
- gripper released
- robot returned home
- controller reported success
- final evidence sealed

Transition:

`completion_condition_satisfied -> COMPLETED`

Completion ends the active authority.

`COMPLETED -> contract_no_longer_executable`

---

# **22. One-Use Authority**

A Motion Contract may be defined as one-use.

Once consumed or completed:

`one_use_contract + replay -> REJECT`

A completed contract must not authorize a second execution.

---

# **23. Protected Execution Boundary**

A physical implementation should enforce:

`no_valid_STRIDE_authority -> no_actuator_command`

The protected execution boundary must reject:

- missing contracts
- malformed contracts
- expired contracts
- withdrawn contracts
- completed contracts
- replayed contracts
- contracts for another robot
- contracts for another action
- contracts with invalid policy identity
- contracts whose required evidence is no longer valid

---

# **24. Evidence Produced**

Execution under a Motion Contract may produce:

- admission evidence
- contract identity
- execution-start evidence
- phase transitions
- pause evidence
- resume evidence
- abort evidence
- completion evidence
- controller acknowledgement
- final certificate

Core replay relation:

`same structure + same policy -> same admission -> same contract identity`

---

# **25. Reference Contract Lifecycle**

A simplified lifecycle is:

`PROPOSED`

↓

`EVALUATED`

↓

`RESOLVED`

↓

`CONTRACT_CREATED`

↓

`EXECUTING`

From `EXECUTING`, the contract may transition to:

`PAUSED`

↓

`REVALIDATED`

↓

`EXECUTING`

or:

`ABORTED`

or:

`COMPLETED`

A paused contract may return to execution only after renewed structural validity.

An aborted or completed contract does not return to `EXECUTING`.

`ABORTED or COMPLETED -> no_further_execution_under_existing_contract`

---

# **26. Conformance Requirements**

A compatible implementation should preserve:

- contract creation only after `RESOLVED`
- exact robot binding
- exact action binding
- exact target binding
- declared physical limits
- explicit validity boundaries
- runtime withdrawal of authority
- revalidation before resume
- rejection after completion
- deterministic contract identity

Core conformance invariant:

`non_resolved_structure -> no_motion_contract`

---

# **27. Reference Example**

A bounded tabletop contract may authorize:

- robot: `robot_arm_A`
- action: `move`
- target: `cupA`
- source: `zoneA`
- destination: `zoneB`
- contact: `allowed`
- requested force: `6 N`
- maximum force: `8 N`
- path: `clear`
- human zone: `clear`
- recovery: `return_home`
- completion: `object_released_in_zoneB`

This contract does not authorize:

- moving Cup B
- moving the cube
- exceeding `8 N`
- entering the protected human zone
- executing after expiration
- executing after completion

---

# **28. Claim Boundary**

The reference Motion Contract demonstrates bounded structural authority within a browser-based observatory.

It does not provide:

- actuator-level enforcement
- certified safety
- secure production signing
- real controller isolation
- measured stopping performance
- regulatory approval

Governing boundary:

`reference_architecture_validated != production_safety_certified`

---

# **29. Final Principle**

A command may request movement.

A controller may be capable of movement.

Only a valid Motion Contract grants bounded execution authority.

`intention != execution_permission`

`capability != permission`

`no_valid_motion_contract -> no_authorized_motion`

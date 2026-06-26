# ⭐ **STRIDE — Runtime State Model**

**Structural Robotic Intention and Deterministic Execution**

**Continuing Authority, Pause, Revalidation, Abort, and Completion**

---

# **1. Purpose**

The STRIDE Runtime State Model defines how structural authority changes before, during, and after execution.

Initial admission is not permanent authority.

The runtime must continue to satisfy the conditions that supported the Motion Contract.

Core law:

`previous_permission != permanent_permission`

---

# **2. Runtime Principle**

The runtime state reflects both:

- execution progress
- continuing structural validity

A valid action may begin only after admission.

A running action may continue only while authority remains valid.

`motion_continues iff contract_valid AND runtime_conditions_valid`

---

# **3. Structural and Runtime States**

STRIDE distinguishes initial admission outputs from runtime and terminal states.

Primary admission outputs:

* `INCOMPLETE`
* `CONFLICT`
* `FORBIDDEN`
* `RESOLVED`

Runtime and terminal states:

* `EXECUTING`
* `PAUSED`
* `ABORTED`
* `COMPLETED`

`INCOMPLETE`, `CONFLICT`, `FORBIDDEN`, and `RESOLVED` describe the result of structural admission.

`EXECUTING`, `PAUSED`, `ABORTED`, and `COMPLETED` describe the lifecycle of admitted execution.

These states represent structural and runtime truth.

They are not merely interface labels.

---

# **4. INCOMPLETE**

`INCOMPLETE` means required structure is missing, ambiguous, stale, or insufficient.

Examples:

- target not unique
- destination missing
- sensor unavailable
- evidence expired
- recovery missing
- contact state unknown

Runtime effect:

- no Motion Contract
- no authorized movement
- hold position

Invariant:

`INCOMPLETE -> no_motion_contract`

---

# **5. CONFLICT**

`CONFLICT` means required structures disagree.

Examples:

- vision reports clear
- proximity reports blocked
- target identity mismatch
- policy identity mismatch
- controller state mismatch

Runtime effect:

- no Motion Contract
- no authorized movement
- hold position

Invariant:

`conflicting_required_observations -> CONFLICT`

---

# **6. FORBIDDEN**

`FORBIDDEN` means the action violates an explicit rule or boundary.

Examples:

- protected human zone occupied
- contact prohibited
- excessive force
- prohibited target
- prohibited action
- path outside workspace

Runtime effect:

- request rejected
- no Motion Contract
- no movement

Invariant:

`FORBIDDEN -> no_motion_contract`

---

# **7. RESOLVED**

`RESOLVED` means the proposed action is structurally admissible.

Required structure is:

* complete
* consistent
* policy-compliant
* path-admissible
* human-safe
* contact-admissible
* within limits
* recoverable

Runtime effect:

* a bounded Motion Contract may be created
* execution may begin only after the Motion Contract is validated

Core transitions:

`RESOLVED -> motion_contract_may_be_created`

`RESOLVED + valid_motion_contract -> EXECUTING`

---

# **8. EXECUTING**

`EXECUTING` means a valid Motion Contract is active and the controller is performing the bounded action.

Continuing requirements may include:

- contract not expired
- required sensors valid
- path remains admissible
- protected zone remains clear
- force remains within limits
- controller remains in the expected session
- recovery remains available

Invariant:

`EXECUTING -> continuous_authority_monitoring`

---

# **9. PAUSED**

`PAUSED` means active execution has been temporarily suspended.

This may occur because:

* a recoverable continuing-validity condition was lost
* an explicit pause was requested

Examples:

* human enters protected zone
* obstacle enters path
* sensor becomes unavailable
* observations become stale
* controller state changes
* communication is interrupted
* pause is requested

Runtime effect within the reference model:

* motion animation stops
* phase scheduling stops
* active velocity becomes zero
* current phase is preserved
* reset remains available
* resume requires renewed validity

Core transitions:

`EXECUTING + recoverable_invalidated_condition -> PAUSED`

`EXECUTING + pause_request -> PAUSED`

---

# **10. ABORTED**

`ABORTED` means execution cannot safely continue or recover under the existing Motion Contract.

Examples:

- unrecoverable controller failure
- invalid contract identity
- recovery unavailable
- persistent path loss
- physical state outside contract bounds
- contract integrity failure

Runtime effect:

- active authority ends
- existing contract cannot resume
- new proposal may be required
- abort evidence is produced

Invariant:

`unrecoverable_runtime_change -> ABORTED`

---

# **11. COMPLETED**

`COMPLETED` means the declared completion condition has been satisfied.

The reference completion sequence includes:

- target placed
- target released
- robot returned home
- active timers cleared
- active animation stopped
- final evidence sealed

Invariant:

`completion_condition_satisfied -> COMPLETED`

Cleanup invariant:

`COMPLETED -> no_active_scheduler`

---

# **12. Reference Execution Phases**

The STRIDE v1.1.3 observatory demonstrates:

`APPROACH -> CONTACT -> LIFT -> TRANSFER -> PLACE -> RETURN -> COMPLETED`

---

## **12.1 APPROACH**

The robot moves toward the admitted target.

Required conditions include:

- target remains present
- path remains clear
- contract remains valid
- human zone remains clear

---

## **12.2 CONTACT**

The gripper reaches the declared contact state.

Required conditions include:

- contact remains permitted
- force remains within limits
- target identity remains valid

---

## **12.3 LIFT**

The target is attached and lifted.

Required conditions include:

- grasp remains valid
- object remains attached
- force remains admissible
- path remains valid

---

## **12.4 TRANSFER**

The target moves toward the destination.

Required conditions include:

- destination remains valid
- transfer path remains clear
- protected zones remain clear

---

## **12.5 PLACE**

The target is lowered into the destination.

Required conditions include:

- destination remains admissible
- placement remains within bounds
- release condition remains valid

---

## **12.6 RETURN**

The gripper releases the target and the robot returns home.

Required conditions include:

- release confirmed
- home path valid
- controller state valid

---

# **13. State Transition Model**

Admission outputs may change after relevant structure is corrected and evaluated again:

`INCOMPLETE -> RESOLVED`

`CONFLICT -> RESOLVED`

`FORBIDDEN -> RESOLVED`

Primary runtime transitions include:

`RESOLVED + valid_motion_contract -> EXECUTING`

`EXECUTING -> PAUSED`

`PAUSED + renewed_validity -> RESUME -> EXECUTING`

`EXECUTING -> ABORTED`

`PAUSED -> ABORTED`

`EXECUTING -> COMPLETED`

Invalid transitions under the existing Motion Contract include:

`INCOMPLETE -> EXECUTING`

`CONFLICT -> EXECUTING`

`FORBIDDEN -> EXECUTING`

`COMPLETED -> EXECUTING`

`ABORTED -> EXECUTING`

A completed or aborted action may execute again only through a new proposal, admission, and Motion Contract cycle.

---

# **14. Admission-to-Execution Transition**

Execution may begin only when:

- admission is `RESOLVED`
- a valid Motion Contract exists
- the contract matches the robot
- the contract matches the action
- the contract is unexpired
- runtime conditions remain valid

Core transition:

`RESOLVED + valid_contract -> EXECUTING`

---

# **15. Runtime Revalidation**

Runtime revalidation checks whether continuing authority remains valid.

Triggers may include:

- sensor update
- human-zone change
- path change
- controller acknowledgement
- contract expiration
- communication loss
- phase transition
- manual pause
- recovery event

Core rule:

`world_change -> authority_revalidation`

---

# **16. Pause Trigger Model**

A pause may be triggered by:

* protected-zone occupation
* obstacle detection
* sensor conflict
* sensor loss
* stale evidence
* path invalidation
* controller-state change
* recoverable limit deviation
* explicit pause request

A condition that cannot be safely recovered within the existing Motion Contract may instead produce `ABORTED`.

The pause must preserve enough state for deterministic inspection and possible revalidation.

---

# **17. Pause Behavior**

During `PAUSED`:

* no new motion phase begins
* the current phase does not advance
* active motion animation is stopped
* the active phase timer is stopped
* object attachment state is preserved
* event logging remains available
* reset remains available
* a resume request may be made
* execution cannot continue until revalidation succeeds

Visual rule:

`paused_runtime -> paused_visual_activity`

Scheduler rule:

`PAUSED -> no_active_motion_scheduler`

---

# **18. Resume Model**

Resume requires:

* original contract still valid
* contract not expired
* required evidence renewed
* path valid
* human zone clear
* controller ready
* recovery still available
* structural evaluation resolved

Core transition:

`PAUSED + renewed_validity -> RESUME -> EXECUTING`

Invalid resume:

`resume_request + invalid_structure -> remain_paused`

In the reference observatory, pressing **RESUME** automatically revalidates the current structure.

A separate manual evaluation is not required.

An aborted or completed contract cannot resume.

---

# **19. Reset Model**

Reset returns the observatory to a stable initial state.

Reset should:

- stop active animation
- clear active timers
- clear carried-object state
- restore robot pose
- restore object positions
- clear active execution authority
- return admission to the current evaluated input state

Reset is not equivalent to successful completion.

---

# **20. Abort Model**

Abort ends the existing execution authority.

Abort should:

- stop active scheduling
- clear or seal the contract
- record the abort reason
- preserve evidence
- prevent silent resume
- require new admission where appropriate

Core invariant:

`ABORTED -> existing_contract_not_executable`

---

# **21. Completion Model**

Completion is based on the declared completion condition.

In the reference observatory, completion requires:

- admitted object transferred
- object placed in destination
- object released
- robot returned home
- no active scheduler
- final runtime evidence available

Completion is not based only on elapsed time.

---

# **22. Contract Expiration**

If the Motion Contract expires:

`expired_contract -> no_continuing_authority`

The runtime should transition to:

- `PAUSED` when safe recovery remains possible
- `ABORTED` when continuation and recovery are no longer supported

---

# **23. Communication Loss**

Communication loss must not silently preserve authority.

A future physical implementation should define:

`communication_loss -> pause_or_abort`

The exact result depends on:

- controller autonomy
- recovery structure
- safe-stop capability
- contract policy

---

# **24. Controller Restart**

A controller restart changes runtime identity.

The previous authority must not be assumed valid.

Core rule:

`controller_restart -> authority_revalidation`

A new controller session may require a new contract.

---

# **25. Object Attachment State**

The runtime tracks whether the gripper is carrying the admitted object.

Reference states include:

- not attached
- attached
- released

The object should move with the gripper only while attached.

Visual invariant:

`object_motion -> object_attached_to_authorized_gripper`

---

# **26. Scheduler Integrity**

The runtime may use:

* animation frames
* timers
* phase scheduling

Scheduler state must match runtime state.

Examples:

`EXECUTING -> scheduler_active`

`PAUSED -> scheduler_inactive`

`ABORTED -> scheduler_stopped`

`COMPLETED -> no_active_scheduler`

A paused runtime may preserve its phase and object state.

It must not preserve active motion scheduling.

---

# **27. Event Trace**

The runtime event trace may record:

- evaluation
- admission state
- contract creation
- execution start
- phase transition
- pause
- resume
- abort
- completion
- certificate generation
- qualification start
- qualification completion

The event trace supports inspectability.

It does not create authority.

---

# **28. Deterministic Replay**

Equivalent complete runtime conditions should reproduce equivalent transitions within the same conforming implementation and implementation version.

Core invariant:

`same relevant structure + same runtime state + same event + same implementation version -> same transition`

Replay divergence indicates that one or more of the following may have changed:

* action structure
* world structure
* policy structure
* runtime state
* runtime event
* implementation behavior
* implementation version

---

# **29. Visual Truth**

The observatory must not show active authority when none exists.

Core invariant:

`visual_activity_must_reflect_runtime_truth`

Examples:

- unresolved action shows no authorized flow
- executing action shows active phase
- paused action freezes motion
- completed action shows no active scheduler

---

# **30. Qualification Expectations**

The runtime state model is exercised by the bounded qualification suite.

Expected frozen result:

`total_case_count = 346`

`allPass = true`

`unsafe_admissions = 0`

This confirms the declared reference scenarios.

It does not certify real physical safety.

---

# **31. Conformance Requirements**

A compatible runtime should preserve:

- no execution from non-resolved states
- contract validation before execution
- continuous runtime validity
- pause on invalidation
- revalidation before resume
- no silent resume after abort
- no execution after completion
- no active scheduler after completion
- deterministic state transitions
- evidence for terminal states

---

# **32. Claim Boundary**

The STRIDE v1.1.3 runtime is a browser-based reference implementation.

It does not provide:

- real actuator enforcement
- certified stopping performance
- real controller isolation
- physical fault tolerance
- regulatory approval
- production safety certification

Governing boundary:

`reference_architecture_validated != production_safety_certified`

---

# **33. Final Principle**

Authority is not permanent.

Execution remains valid only while the structure supporting it remains valid.

`previous_permission != permanent_permission`

`world_change -> authority_revalidation`

`PAUSED + renewed_validity -> RESUME`

`COMPLETED -> no_active_scheduler`

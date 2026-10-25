# FutureSOC AI 2.0

### Evidence-Grounded AI-Assisted Security Incident Investigation

FutureSOC AI 2.0 is a research prototype for organizing security-event
evidence, correlating potentially relevant records, and evaluating
investigation claims against explicit evidence-validation rules. It is
designed to help keep investigation conclusions within the limits of the
available evidence.

> **Project status:** Research prototype. The evaluated workflow does
> not automatically execute containment or remediation actions. Human
> review remains necessary before consequential security decisions.

## Why this project?

Security telemetry can reveal suspicious activity without proving a
broader claim such as account compromise, unauthorized access, or
malicious intent. FutureSOC AI 2.0 focuses on this distinction: evidence
of an event is not automatically evidence for every hypothesis
associated with it.

The project evaluates a deterministic evidence-validation mechanism that
can return **Supported**, **Rejected**, or **Inconclusive** rather than
forcing a definitive conclusion when evidence is missing, conflicting,
or insufficient.

## Key capabilities

-   **Claim-level evidence validation** using predefined, deterministic
    rules.
-   **Three-way outcomes:** supported, rejected, and inconclusive.
-   **Evidence rationale** recording supporting evidence,
    contradictions, and missing-information explanations.
-   **Security-event correlation** using available incident attributes,
    such as organization, source IP address, and username.
-   **Traceable investigation workflow** covering evidence review, event
    correlation, validation, and investigation disposition.
-   **Persisted investigation results** including run information,
    validation outcome, summary, correlated-event count, and
    response-execution status.
-   **Human-oversight boundary:** the evaluated workflow records
    investigation results but does not execute response actions.

## How the workflow operates

``` text
Security-event records + investigation hypothesis
                      |
                      v
               Evidence review
                      |
                      v
            Security-event correlation
                      |
                      v
          Deterministic evidence validator
             /          |          \
       Supported     Rejected    Inconclusive
             \          |          /
                      v
            Investigation disposition
                      |
                      v
        Persist outcome, rationale, references
             and information gaps for review
```

Correlation identifies potentially relevant records; it does not by
itself establish causation or prove a hypothesis. The validator applies
the implemented claim-specific rules before the workflow records its
disposition.

## Validation outcomes

  -----------------------------------------------------------------------
  Outcome                             Meaning
  ----------------------------------- -----------------------------------
  **Supported**                       Available evidence satisfies the
                                      implemented rule for the specific
                                      claim.

  **Rejected**                        Available evidence contradicts the
                                      specific claim under the
                                      implemented rule.

  **Inconclusive**                    Evidence is missing, invalid,
                                      conflicting, or insufficient for a
                                      definitive decision.
  -----------------------------------------------------------------------

These outcomes apply to the claim being evaluated. For example,
rejecting a narrow claim does not prove that no attack occurred.
Implemented thresholds and recognized values are prototype-specific
rules, not universal cybersecurity standards.

## Research evaluation

The research evaluated the validator on **25 controlled cases** and
compared it with a deliberately simple first-evidence-only baseline. The
expected outcomes were defined by the test protocol; they were not
independent judgments from SOC analysts or verified real-world incident
outcomes.

  Metric                                         First-evidence-only baseline   Proposed validator
  -------------------------------------------- ------------------------------ --------------------
  Protocol conformance                                            80% (20/25)         100% (25/25)
  Unsupported acceptances under the protocol                                5                    0
  Decision coverage                                               68% (17/25)          48% (12/25)
  Abstention rate                                                  32% (8/25)          52% (13/25)

**Interpretation:** On these predefined cases, the validator followed
the specified protocol more consistently and abstained more often.
Because the validator and expected labels share the intended rule
specification, the evaluation is vulnerable to specification coupling.
The results demonstrate conformance on the tested cases; they do **not**
establish independent real-world accuracy or production SOC
effectiveness.

The project also includes exploratory CIC-IDS2017 PortScan-versus-BENIGN
traffic-classification experiments. Those experiments are separate from
the evidence-validator evaluation and should not be interpreted as
evidence that the validator is correct.

## Architecture and implementation notes

The documented implementation includes a deterministic validator
(`evidence_validator.py`) and a workflow component
(`investigation_workflow.py`). The workflow persists its steps and
validation results. The evaluated design is a deterministic workflow,
**not** an autonomous multi-agent SOC.

The source code and project-specific dependency/configuration files
should be used as the authority for installation and execution
instructions. Those details are intentionally not guessed in this
README.

## Getting started

The complete application source and its dependency files must be present
in this repository before setup commands can be documented reliably.

1.  Clone the private repository.
2.  Review the actual project structure and dependency manifest.
3.  Configure environment variables using a template; do not commit
    secrets.
4.  Set up the required database and other services according to the
    project configuration.
5.  Install dependencies and run the application using the commands
    documented in the source repository.
6.  Run the provided tests before using the prototype.

> **Repository preparation note:** Add verified installation commands,
> an environment-variable table, database setup/migration instructions,
> API entry point, test commands, and frontend launch instructions after
> the actual source tree has been reviewed. Do not publish placeholder
> commands as if they were tested.

## Security and responsible use

-   Do not commit `.env` files, credentials, API tokens, private keys,
    database dumps, or sensitive security logs.
-   Use synthetic or sanitized security-event data in examples and
    tests.
-   Keep response execution disabled unless a separately reviewed
    implementation and appropriate safeguards are introduced.
-   Require qualified human review before taking containment,
    remediation, or other consequential actions.
-   Treat validator outcomes as decision support, not as a substitute
    for analyst judgment.

## Current limitations

-   The comparative validator evaluation uses 25 controlled cases with
    protocol-defined expected outcomes.
-   Independent expert-adjudicated cases and verified operational
    incident outcomes have not been established by this evaluation.
-   The first-evidence-only baseline is intentionally simple and is not
    a strong external benchmark.
-   Attribute-based event correlation does not establish causal
    relationships between records.
-   The classification experiments use a separate task and do not
    validate investigation conclusions.
-   Production readiness, live SOC effectiveness, and autonomous
    incident response have not been demonstrated.

## Research direction

Future work should prioritize independently constructed and
expert-adjudicated investigation cases, stronger comparison methods,
broader evidence sources, reproducible case-level artifacts, and
evaluation in realistic analyst workflows. Operational deployment would
also require security review, access controls, auditability, and human
approval for consequential actions.

## Citation

If you use or build on this project, cite the associated research
manuscript once its bibliographic details and publication status are
finalized.

**Working research title:** *Evidence-Grounded AI-Assisted Security
Incident Investigation: Design and Controlled Evaluation of FutureSOC AI
2.0*

## License

No license is specified yet. Until a license is added, do not assume
that the repository grants permission to reuse, modify, or redistribute
its contents.

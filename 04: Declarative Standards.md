## Flow Design & Architecture

### Flows Per Object: A Modern Approach

The traditional "one Flow per object" rule is an outdated anti-pattern that can lead to large, complex, and unmanageable flows. The original purpose of this rule was to work around a platform limitation, namely the unpredictable order of execution for multiple automations on a single object. With the introduction of the "Trigger Order" feature in the Spring '22 release, which allows for the explicit ordering of record-triggered flows, this limitation has been removed.

The modern and more efficient approach is to use multiple, smaller, and highly focused record-triggered flows on a single object. Each flow should have very specific and narrow entry criteria. This strategy offers several benefits:

- Enhanced Maintainability: A smaller flow is easier to understand, debug, and maintain, especially in a team environment with multiple administrators.
- Improved Performance: Flows with narrow entry conditions are more performant because they only fire when truly relevant, avoiding the overhead of a large, monolithic flow that must trigger on every creation or update.
- Simplified Troubleshooting: When an issue arises, it is far easier to isolate and troubleshoot a problem in a small, single-purpose flow than in a complex, multi-branching automation.

While there is no hard limit on the number of flows, the "Rule of Three" serves as a practical guideline for consolidating automation. It suggests having up to three record-triggered flows per object: one for Before create or update, one for After create or update, and one for Before delete. This provides a sensible balance between consolidation and modularity.

## Managing Flow Complexity

Overly complex flows are a primary source of technical debt in a declarative environment. When a flow becomes too large or does too much, it is a clear indicator that it needs to be refactored. A modular approach should be adopted, leveraging subflows to break down complex business processes into smaller, reusable components.

For multi-stage, sequential processes, the use of Flow Orchestrator is recommended. This tool provides a clear, modular way to manage complex workflows, ensuring that the process is traceable and easy to debug. A flow can be deemed complex if it has a high number of blocks, loops, decisions, or subflows. These flows are often candidates for simplification, being either broken down into subflows or rebuilt with a more streamlined design.

## Performance and Scalability

Designing flows for performance is critical for long-term org health.

- Prioritize Before-Save Flows: For field updates on the same record that triggered the flow, a Before-Save flow is the most performant option as it runs before the record is saved to the database, eliminating a second DML operation.
- Avoid Loops for SOQL/DML: Just like in Apex, DML and SOQL operations should not be placed inside loops in Flows. This prevents the consumption of excessive governor limits. The correct approach is to use a
- Get Records element outside the loop to retrieve a collection of records and then process that collection.
- Leverage External Configuration: For frequently changing business logic, such as a set of rules or criteria, it is better to store this information in a custom object or custom metadata type. The flow can then retrieve this data, making it more flexible and easier to update without requiring a flow revision.

## Robustness and Error Handling

Proper error handling is a mandatory component of a robust flow design. Every Create, Update, Delete, and Action element in a flow must have a Fault Path. A fault path ensures that if an operation fails, the entire transaction does not fail, and a custom error message or alternative route can be provided to the user. This prevents complex system fault messages from being displayed to the end-user and improves the overall user experience.

All flows should be thoroughly debugged and tested in a sandbox environment before deployment to production. The Flow Builder's built-in debugger is an essential tool for identifying issues and ensuring the flow behaves as expected.

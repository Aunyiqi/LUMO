UMO AI Chat Box Architecture

The LUMO AI Chat Box is a single student interface that routes each message to the Stress Assistant, Assignment Task Assistant, or both services. A deterministic engine calculates the stress estimate, while the AI explains the result, breaks assignments into manageable steps, and proposes only changes that pass the Feasibility Gate and receive student approval.

flowchart TD
    Student([Student]) --> App["LUMO Mobile App"]
    App --> Chat["AI Chat Box<br/>Receive student question"]
    Chat --> Router["Intent and Context Router<br/>Identify required support"]

    Router --> StressNeeded{"Stress support<br/>needed?"}

    StressNeeded -->|Yes| Stress["Stress Assistant<br/>Retrieve permitted student data<br/>Calculate weighted stress score<br/>Classify Stable, Rising or High Risk<br/>Explain causes and confidence"]
    StressNeeded -->|No| AssignmentNeeded{"Assignment support<br/>needed?"}

    Database[("Supabase PostgreSQL<br/>Schedule, deadlines, health summaries,<br/>check-ins and assignments")] --> Stress

    Stress --> Safety{"Immediate safety<br/>concern?"}
    Safety -->|Yes| Support["Stop productivity advice<br/>Display university or emergency support"]
    Safety -->|No| AssignmentNeeded

    AssignmentNeeded -->|Yes| Assignment["Assignment Task Assistant<br/>Retrieve deadline, importance,<br/>progress and available time"]
    AssignmentNeeded -->|No| Coordinator["AI Response Coordinator<br/>Combine explanations and assistance"]

    Database --> Assignment
    Assignment --> EnoughInfo{"Enough assignment<br/>information?"}
    EnoughInfo -->|No| Ask["Ask the student for<br/>missing information"]
    EnoughInfo -->|Yes| Breakdown["Break assignment into steps<br/>Estimate duration and priority<br/>Create a proposed study plan"]
    Breakdown --> Coordinator

    Coordinator --> Change{"Schedule change<br/>proposed?"}
    Change -->|No| Advice["Display explanation<br/>and recommended actions"]
    Change -->|Yes| Gate["Feasibility Gate<br/>Check deadlines, fixed commitments,<br/>sleep, recovery, conflicts and capacity"]

    Gate --> Feasible{"Plan feasible?"}
    Feasible -->|No| Shortfall["Explain the shortfall<br/>Suggest extension, delegation<br/>or renegotiation"]
    Feasible -->|Yes| Preview["Show before-and-after preview"]

    Preview --> Approval{"Student approves?"}
    Approval -->|Yes| Save["Save approved plan<br/>in Supabase PostgreSQL"]
    Approval -->|No| Unchanged["Keep current schedule unchanged"]

    Support --> Response["Return response through<br/>the AI Chat Box"]
    Ask --> Response
    Advice --> Response
    Shortfall --> Response
    Save --> Response
    Unchanged --> Response

    Response --> End(( ))

    classDef endpoint fill:#000000,stroke:#000000,color:#000000;
    class End endpoint;

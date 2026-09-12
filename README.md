<details>
<summary>View editable Mermaid source</summary>

flowchart TD
    Student([Student]) --> App["LUMO Mobile App"]
    App --> Chat["AI Chat Box<br/>Receive student question"]
    Chat --> Router{"Question type?"}

    Router -->|Stress| Stress["Stress Assistant<br/>Ask focused questions<br/>Explain stress level and causes"]
    Router -->|Assignment| Assignment["Assignment Task Assistant<br/>Understand assignment difficulty"]
    Router -->|Both| Stress
    Router -->|Both| Assignment

    Stress --> Calculator["Stress Calculation Engine<br/>Schedule pressure: 35%<br/>Deadline urgency: 25%<br/>Exam demand: 15%<br/>Behaviour change: 15%<br/>Student check-in: 10%"]
    Data[("Supabase PostgreSQL<br/>Schedules, deadlines, health summaries,<br/>screen time, check-ins and assignments")] <--> Calculator

    Calculator --> Level["Stress Result<br/>Stable, Rising or High Risk<br/>Main causes and confidence"]
    Level --> Safety{"Immediate safety<br/>concern?"}
    Safety -->|Yes| Support["Stop productivity advice<br/>Show university or emergency support"]
    Safety -->|No| Response["AI Response Coordinator"]

    Assignment --> Breakdown["Task Breakdown Engine<br/>Create steps<br/>Estimate duration<br/>Set priorities"]
    Data <--> Breakdown
    Breakdown --> Gate["Feasibility Gate<br/>Check deadlines, fixed commitments,<br/>recovery time, conflicts and capacity"]
    Gate --> Feasible{"Plan feasible?"}

    Feasible -->|No| Shortfall["Explain remaining shortfall<br/>Suggest extension, delegation<br/>or renegotiation"]
    Feasible -->|Yes| Preview["Show proposed plan preview"]
    Preview --> Approval{"Student approves?"}
    Approval -->|Yes| Save["Save approved plan"]
    Approval -->|No| Unchanged["Keep schedule unchanged"]

    Save --> Data
    Support --> Response
    Shortfall --> Response
    Save --> Response
    Unchanged --> Response

    Response --> Result["AI Chat Box<br/>Display explanation, advice or plan"]
    Result --> End(( ))

    classDef endpoint fill:#000000,stroke:#000000,color:#000000;
    class End endpoint;

</details>

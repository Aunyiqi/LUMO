@startuml
title LUMO AI Chat Box Architecture

skinparam backgroundColor white
skinparam shadowing false
skinparam defaultFontName Arial
skinparam ArrowColor #222222
skinparam activityBorderColor #555555
skinparam activityBackgroundColor #F7F7F7
skinparam activityDiamondBackgroundColor white
skinparam activityDiamondBorderColor #222222

start

:Student sends a question through
the LUMO Mobile App;

:AI Chat Box receives the message;

:Intent and Context Router
Identifies Stress Support,
Assignment Support, or Both;

if (Stress support needed?) then (Yes)

  partition "Stress Assistant" {

    :Retrieve permitted schedule, health,
    screen-time and check-in data
    from Supabase PostgreSQL;

    :Stress Calculation Engine
    Calculate the weighted stress score;

    :Classify the result as
    Stable, Rising or High Risk;

    :Ask a focused question
    if important context is missing;

    :Explain the main causes
    and confidence level;
  }

endif

if (Assignment support needed?) then (Yes)

  partition "Assignment Task Assistant" {

    :Retrieve the assignment deadline,
    importance, available time
    and current progress;

    if (Enough assignment information?) then (Yes)

      :Break the assignment into
      manageable steps;

      :Estimate the duration and
      priority of each step;

      :Create a proposed study plan;

    else (No)

      :Ask the student for
      the missing information;

    endif
  }

endif

if (Immediate safety concern reported?) then (Yes)

  :Stop productivity advice and display
  university or emergency support information;

else (No)

  :AI Response Coordinator
  Combines the stress explanation
  and assignment assistance;

  if (Schedule change proposed?) then (Yes)

    partition "Feasibility Gate" {

      :Check deadlines, fixed commitments,
      sleep, recovery, conflicts and capacity;
    }

    if (Proposed plan feasible?) then (Yes)

      :Show the before-and-after preview;

      if (Student approves the plan?) then (Yes)

        :Save the approved plan
        in Supabase PostgreSQL;

      else (No)

        :Keep the current schedule unchanged;

      endif

    else (No)

      :Explain the remaining shortfall and suggest
      an extension, delegation or renegotiation;

    endif

  else (No)

    :Display the explanation
    and recommended actions;

  endif

endif

:Return the response through
the AI Chat Box;

stop

@enduml

# PawPal+ Project Reflection

## 1. System Design

**a. Initial design**

My initial design centered on four classes: `Task`, `Pet`, `Owner`, and `Scheduler`. `Task` was responsible for holding all the details of a single care activity — its description, scheduled time, duration, priority, frequency, and completion status. `Pet` was responsible for storing a pet's basic information and maintaining its list of tasks. `Owner` was responsible for managing a collection of pets and providing access to all their tasks in one place. `Scheduler` was designed as the "brain" of the system — a separate class that takes an `Owner` as input and handles all the logic for organizing, sorting, filtering, and analyzing tasks across all pets.

**b. Design changes**

Yes, the design evolved during implementation. Initially, `Task` only had a `time`, `frequency`, and `completed` field. During implementation, I added `duration` and `priority` fields because the project requirements called for scheduling based on constraints — not just time. I also added a `due_date` field to support recurring tasks properly, since daily and weekly tasks need to know their current due date in order to calculate the next one using `timedelta`. These additions made the `Task` class more realistic and the scheduler more intelligent.

---

## 2. Scheduling Logic and Tradeoffs

**a. Constraints and priorities**

The scheduler considers three main constraints: time (when a task is scheduled), priority (High, Medium, or Low), and frequency (once, daily, or weekly). When generating the daily plan, priority is the primary sort key, with time used as a tiebreaker. This means a High priority task at 3:00 PM will always appear before a Medium priority task at 8:00 AM. I decided priority should matter most because a pet owner should never miss a medication or vet appointment just because it happens to be scheduled later in the day.

**b. Tradeoffs**

One tradeoff the scheduler makes is that conflict detection only checks for exact time matches rather than overlapping time windows. For example, if one task starts at 08:00 and takes 30 minutes, and another starts at 08:15, the scheduler won't flag that as a conflict. This is a reasonable tradeoff for this scenario because most pet care tasks are short and loosely scheduled — owners don't need minute-by-minute precision. A simple exact-match check catches the most obvious conflicts (two tasks literally at the same time) without overcomplicating the logic.

---

## 3. AI Collaboration

**a. How you used AI**

I used AI throughout the project for several purposes: designing the class structure and relationships, scaffolding the initial code, implementing algorithmic methods like sorting and conflict detection, writing the Streamlit UI, and generating the automated test suite. The most helpful prompts were specific and context-rich — for example, asking the AI to explain how `timedelta` works for recurring tasks, or asking it to suggest a more readable way to format terminal output. Breaking the project into phases and asking focused questions for each phase kept the AI's responses relevant and manageable.

**b. Judgment and verification**

One moment where I didn't accept an AI suggestion as-is was with the conflict detection logic. The AI's first suggestion used a nested loop that compared every task against every other task, which was harder to read and could produce duplicate warnings. I evaluated it by tracing through the logic manually with a simple example (two tasks at the same time for the same pet) and realized a dictionary-based approach — storing seen times as keys — was simpler, faster, and produced cleaner output. I kept the dictionary version because it was easier to understand and maintain.

---

## 4. Testing and Verification

**a. What you tested**

The test suite covers ten behaviors: marking a task complete, adding a task to a pet, sorting tasks chronologically, sorting tasks by priority, daily recurrence (new task created for tomorrow), once-frequency tasks not recurring, conflict detection triggering correctly, no false conflict warnings when times differ, an empty schedule for an owner with no pets, and filtering tasks by pet name. These tests were important because they verify both the "happy path" (normal usage) and edge cases (empty data, non-recurring tasks) that could silently break the app if left untested.

**b. Confidence**

I'm confident the core scheduling behaviors work correctly — all 10 tests pass. If I had more time, I would test additional edge cases such as: a pet with tasks on different due dates (ensuring only today's tasks appear in the daily schedule), marking a weekly task complete near the end of a month (to verify date rollover works), and adding two pets with the same name (to verify the duplicate check in the Owner class behaves correctly).

---

## 5. Reflection

**a. What went well**

I'm most satisfied with how cleanly the four classes work together. The separation between `Owner`, `Pet`, `Task`, and `Scheduler` made it easy to add features like recurring tasks and conflict detection without touching unrelated parts of the code. The "CLI-first" approach — verifying the backend worked in `main.py` before connecting it to the Streamlit UI — also saved a lot of debugging time.

**b. What you would improve**

If I had another iteration, I would add data persistence so that pets and tasks aren't lost when the app restarts. Currently everything lives in Streamlit's session state and disappears when the browser tab closes. Saving data to a JSON file would make the app genuinely useful for a real pet owner. I would also improve conflict detection to account for task duration, so overlapping windows are caught — not just exact time matches.

**c. Key takeaway**

The most important thing I learned is that AI is most useful when you already have a clear picture of what you're building. When I gave the AI a specific problem — "how should the Scheduler retrieve tasks from the Owner's pets?" — it gave me a useful, focused answer. When I asked something vague, the responses were harder to apply. Being the "lead architect" means knowing what questions to ask, not just accepting whatever the AI produces first.

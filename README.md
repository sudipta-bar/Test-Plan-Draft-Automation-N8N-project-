# Project: Automated Test Plan Generator (n8n + Gemini)

## Introduction
In this project, I have developed an automation workflow that converts software requirements into a structured Test Plan draft. The goal was to eliminate the manual effort of creating standard planning documents while maintaining high accuracy in estimation and risk analysis.

## Workflow Architecture
The system is built on n8n and operates through a sequential data pipeline:
1. **Endpoint (Webhook):** I set up a POST method at `/generate-test-plan` to receive requirement data from the frontend.
2. **AI Analysis (Gemini 1.5 Flash):** The module details and features are sent to the LLM to generate the qualitative sections like scope, test types, and criteria.
3. **Deterministic Logic (JavaScript):** Instead of letting the AI estimate time and effort (which can be inconsistent), I used a custom JavaScript node to apply hard-coded estimation rules.
4. **File Storage:** The final plan is converted into a JSON file and saved locally on the disk for version tracking.
5. **UI Response:** The finalized JSON object is sent back to the frontend to be displayed to the user.



## Effort Estimation Logic
I implemented the estimation logic specifically to follow the complexity-based rules defined in the assignment brief. The JavaScript node evaluates the `complexity` field from the input and assigns the following:

- **Low Complexity:** 10 test cases | 2 days of effort.
- **Medium Complexity:** 25 test cases | 5 days of effort.
- **High Complexity:** 50 test cases | 10 days of effort.

This approach ensures that the "quantitative" part of the plan is always predictable and meets business requirements.

## Impact of Complexity on the Plan
The complexity level is the core driver of the output:
* **For Calculations:** It sets the exact test case count and effort days as mentioned above.
* **For Content:** I engineered the prompt so that a "High" complexity setting forces the AI to look for more detailed risks (like security or scalability) and more stringent entry/exit criteria. For "Low" complexity, it keeps the plan concise and focused on basic functional checks.



## Frontend & Integration
I built a clean, functional web interface to test the workflow. It allows for:
- Pasting the requirement JSON directly.
- Monitoring the generation status in real-time.
- Viewing the formatted output in a developer-friendly terminal view.

## Project Deliverables
- `workflow.json`: The full n8n workflow export.
- `index.html`: Source code for the frontend interface.
- `prompt.txt`: The system prompt used for the LLM node.
- `test_plan_sample.json`: A sample of the saved artifact.
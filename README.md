# Code-Interpreter-Python-Exec-Visuals

Welcome! This repository contains a collection of prompt templates designed to turn Microsoft Copilot Studio (or any Python-enabled AI agent) into a high-end Data Information Designer.

These aren't your standard Excel charts. They are "Executive Scorecards"—visuals designed to reduce cognitive load, tell a clear story, and look professional enough for a boardroom presentation without requiring a human designer to touch them.

>> The Philosophy: "Stop Reporting, Start Storytelling"
Most default charts (in Python, Excel, or PowerBI) are designed for analysts, not executives. They are cluttered with gridlines, axis ticks, heavy borders, and legends that force the viewer to do mental math.

The prompts in this repository rely on three core design principles inspired by modern data storytelling:

The "Floating" Aesthetic: We remove the "box." Data shouldn't look like it's trapped in a cage. By removing axis lines and borders, the data feels lighter and easier to read.

Hierarchy of Information: The most important number (The "Hero Metric") is always the biggest thing on the page. We use color intentionally—highlighting only what matters (the result) and greying out the context.

Integrated Narrative: A chart without an insight is just a drawing. Every visual here includes a text zone specifically for a "Business Narrative"—explaining the why, not just the what.

![Examples](https://github.com/ITSpecialist111/Code-Interpreter-Python-Exec-Visuals/blob/main/Screenshot%206%20Exec%20Visualisations.png)

>> The Engineering: Breaking Down the Prompt Logic
You will notice that every prompt in this repo follows a strict, non-negotiable structure. We aren't just asking the AI to "make a chart." We are giving it a Design System.

Here is why we built the prompts this way:

1. The "Canvas" vs. "The Chart"
Standard Python code tries to fill the whole image with the graph. We force the AI to treat the image as a Blank Canvas (fig = plt.figure) and then manually draw "zones" on top of it.

Why? This prevents text from overlapping with bars. It allows us to put the Title and Hero Number on the left and the Chart on the right, creating a magazine-style layout.

2. The "Nuclear Option" (axis('off'))
You will see this command in every prompt. We don't just ask the AI to hide the top spine; we tell it to turn off the axis entirely.

Why? AI agents love default settings. If you give them an inch, they will add a black border or a random tick mark. The "Nuclear Option" forces a clean slate, allowing us to float labels exactly where we want them.

3. The "Fallback" Protocol
Real-world data is messy. PDFs are hard to read.

Why? Each prompt includes a Fallback Data block. If the AI cannot parse your file, it defaults to using demo data. This ensures the code never crashes during a demo, allowing you to see the visual style even if the data extraction fails.

4. Data Sanitation
AI agents often get confused between a "Year" (2024) and a "Value" (2,024).

Why? We include specific logic (e.g., "Ignore values > 1900") to prevent the AI from plotting years as data points, which creates wild, unusable charts.

>> The Gallery: The 6 Visuals
Here is what you will find in this repo and when to use them.

1. The Executive Scorecard (Sparkline)
Use this when: You need to show a single high-level trend (e.g., Revenue over 8 quarters).
The Secret Sauce: It uses a "Sparkline" approach—no X or Y axis numbers. Just the shape of the trend and the final number.

2. The Side-by-Side (Bar)
Use this when: You need to compare recent performance against history with a strong written narrative.
The Secret Sauce: It splits the canvas 50/50. Text on the left, chart on the right. It highlights the current period in blue and fades history to grey.

3. The Leaderboard (Lollipop)
Use this when: You are ranking items (Top Regions, Top Sales Reps, Top Products).
The Secret Sauce: Standard bar charts look heavy. The Lollipop chart uses whitespace to allow for long labels (no text cutoff!) and puts the value inside the dot to save space.

4. The Change Delta (Slope)
Use this when: Comparing "Before vs. After" or "Year 1 vs. Year 2."
The Secret Sauce: It creates a dedicated "gutter" on the left and right for labels. It automatically colors "Growth" lines blue and "Decline" lines grey, instantly showing winners and losers.

5. The Profit Walk (Waterfall)
Use this when: Explaining how you got from a starting number (Revenue) to a simplified ending number (Net Profit).
The Secret Sauce: Coding a waterfall chart from scratch is hard. This prompt forces the AI to do the "floating bar" math for you, coloring negative values red and positive values blue.

6. The Target Tracker (Donut)
Use this when: Showing progress toward a specific goal (e.g., % of Budget Used).
The Secret Sauce: It places the percentage text dead-center in the donut. It removes the "Legend" (which executives hate) and puts the context directly in the title.

>> How to Use These
Open Microsoft Copilot Studio (or an AI provider of your choice ;) with Code Interpreter).

Upload your data file (PDF, CSV, Excel).

Copy/Paste the full prompt from the specific file in this repo.

Watch the python magic happen.

- Graham

P.S - Before you jump in, you'll need some instructions. As of today (Jan 2026) isn't not possible to create a simple Tool in Copilot Studio and it 'just work', the outputs are in an array that Copilot Studio can't split itself, you'll need to create the Prompt tool (Code Interpreter), then build a flow with it in, then call this flow in Copilot Studio with an adative card so the chat can render it! - it's a lot of steps! Here you go:

# How to Render Python Visualizations in Copilot Studio (No Code Dumps)

This guide outlines how to build a Copilot Studio agent that uses Python (Code Interpreter) to generate a custom data visualization and display it cleanly using an Adaptive Card.

It solves two common problems:

1. **The "File Save" Crash**: Prevents the flow from failing when trying to save images to a non-existent file system.

2. **The "Base64 Wall of Text"**: Prevents the agent from dumping raw image code into the chat window.

## Architecture Overview

1. **AI Builder Prompt**: Generates the Python code to create a chart and forces a specific JSON output.

2. **Power Automate Flow**: Executes the prompt, parses the hidden JSON, and extracts the raw Base64 string.

3. **Copilot Studio Action**: Calls the flow and strictly maps the output to a variable (silencing the chat).

4. **Adaptive Card**: Renders the final image using the Base64 variable.

## Step 1: Create the Prompt Tool (AI Builder)

This prompt instructs the Code Interpreter to generate a visualization but not save it as a file. Instead, it buffers the image and returns it as a JSON object.

1. Go to **AI Builder > Prompts**.

2. Create a new prompt using **Code Interpreter** (or "Document/Image processing").

3. **Input**: Text Input.

4. **Prompt Text**: Copy the following exactly.

**Note**: The "Final Output" section is critical. It overrides the default behavior of saving files and printing markdown.
```plaintext
From this data: [[Text input]]

Role: Data Visualization Expert
Goal: Create a high-fidelity replica of the "Executive Scorecard" design shown in the attached reference.

Data Extraction Protocol:
Try extracting text using pypdf.
Fallback: If text length is 0, use pdfplumber (which handles tables better).
Final Fallback: If extraction fails, use this Hardcoded Demo Data so the visualization code can still be tested:
Data = [12, 10, 18, 12, 15, 13, 20, 25]
Hero Number = 25
Comparison = +5 vs prior

Step 1: Data Preparation
Data Slicing: Extract the last 8 numeric values from the provided dataset.
Hero Metric: The last value is the "Hero Number."
Comparison: Calculate the variance vs the previous period.
Filter Logic: Remove any values greater than 200 (to exclude totals/outliers). Only plot values between 0 and 200.

Step 2: The Side-by-Side Canvas
Initialize fig = plt.figure(figsize=(10, 5)). Use ax_main = fig.add_axes([0,0,1,1]) and turn it off.
Draw a white rectangle with a thin grey border to act as the "Card" background.

Step 3: The Left Zone (Text)
Place these using ax_main.text() at left-aligned coordinates:
Metric Name: (e.g., "QTD Sales") at x=0.08, y=0.85, Size 16, Bold.
Hero Number: (e.g., "$25M") at x=0.08, y=0.70, Size 60, Bold, Brand Blue (#0078D4).
Comparison: (e.g., "▲ $5M (25%) QoQ") at x=0.08, y=0.55, Size 14, Bold.

Step 4: The Right Zone (Bar Chart)
Create a chart axis: ax_chart = fig.add_axes([0.45, 0.45, 0.5, 0.45]).
DISABLE AXIS: ax_chart.axis('off').
Plotting:
Plot the first 7 bars in Light Grey.
Plot the 8th (last) bar in Brand Blue.
In-Chart Labels: Add the value of each bar directly above it in a small grey font.
Constraint: Ensure the bars are relatively thin with clean spacing.

Step 5: The Bottom Zone (Narrative)
Place at x=0.08, y=0.15.
Text: A 2-sentence summary starting with "By strategically prioritizing..." that explains the growth. Use a clean sans-serif font, Size 14.

Final Output:
Do NOT save the file to disk or use file streaming. Do NOT use plt.show(). Instead, perform exactly these steps: 
1. Save the figure to a io.BytesIO buffer. 
2. Encode that buffer to a Base64 string. 
3. Return a JSON object with a "files" list containing the filename, content_type "image/png", and the base64_content string. 
Return the JSON object only. Do NOT generate a Markdown image link (like ![Image](data:...)) in the text response. Do not output the Base64 string in plain text.
```

## Step 2: Build the Agent Flow (Power Automate)

This flow acts as the bridge. It runs the Python code and extracts the clean image string.

1. **Trigger**: When an agent calls the flow

2. **Input**: Text (Name it `InputText` or similar).

3. **Action**: Run a prompt (AI Builder)
   - Select the prompt created in Step 1.
   - Pass the flow trigger input into the prompt input.

4. **Action**: Parse JSON
   - **Content**: Select `predictionOutput > text` from the "Run a prompt" step.
   - **Schema**: Use the schema below. (This ensures we catch the filename correctly).
```json
{
    "type": "object",
    "properties": {
        "files": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "filename": { "type": "string" },
                    "content_type": { "type": "string" },
                    "base64_content": { "type": "string" }
                },
                "required": [
                    "filename",
                    "content_type",
                    "base64_content"
                ]
            }
        }
    }
}
```

5. **Action**: Respond to the agent
   - **Output Name**: `image` (Text type).
   - **Value**: Use this expression to grab the string from the array:
```
first(body('Parse_JSON')?['files'])?['base64_content']
```

## Step 3: Configure Copilot Studio (The "Silence" Fix)

This step prevents the bot from accidentally dumping the Base64 string into the chat window.

1. Open your **Topic**.

2. Add a **"Call an action"** node and select your Flow.

3. **Configure Output Settings** (Crucial):
   - In the Action properties, look for **Output Mode** (or "Respond to user").
   - Change it from **All** (or "Respond automatically") to **Specific Variable** (or **Variable**).

4. **Map the Output**:
   - **Flow Output**: `image`
   - **Copilot Variable**: `Topic.Output.image`

This configuration forces the bot to store the data silently instead of printing it.

## Step 4: The Adaptive Card

Finally, display the image cleanly.

1. In your Topic, add a **"Send a message"** node.

2. Select **Adaptive Card**.

3. Choose **Formula** (or **Edit JSON**) and paste the following. Note the concatenation (`&`) that combines the data prefix with your variable.
```json
{
  type: "AdaptiveCard",
  '$schema': "http://adaptivecards.io/schemas/adaptive-card.json",
  version: "1.5",
  body: [
    {
      type: "TextBlock",
      text: "Executive Summary",
      weight: "Bolder",
      size: "Medium"
    },
    {
      type: "Image",
      url: "data:image/png;base64," & Topic.Output.image,
      size: "Large",
      altText: "Executive Scorecard"
    }
  ]
}
```

## Final Result

1. The Agent runs the Python code.
2. The Python code buffers the image to memory (no file errors).
3. The Flow extracts the Base64 string.
4. Copilot Studio receives the string silently.
5. The Adaptive Card renders the visualization perfectly.



##Example - General Instructions for the Agent:

"Purpose Statement
You are an Executive Reporting Analyst specialized in transforming raw company data into high-impact, boardroom-ready insights. Your role is to analyze company performance data from the knowledge base and automatically generate clean, narrative-driven visualizations using Python-powered tools whenever metric data is present.
Core Capabilities
Data Analysis: You interpret business documents (financial reports, ESG impact reports, operational metrics) from the knowledge base.
Insight Generation: You summarize trends and explain the "why" behind the numbers.
Auto-Visualization: You actively scan for metric data (trends, comparisons, KPIs). If metric data is detected, you automatically call the appropriate visualization tool without waiting for a specific user request.
Your Knowledge Base
You have access to company documents stored in the knowledge base including:
Financial statements and P&L reports
ESG and sustainability impact reports
Quarterly business reviews
Employee engagement and HR metrics
Revenue performance data
Always cite which document you are referencing when providing insights.
Your Visualization Tools
When valid metric data is identified, you must call one of these two specialized tools:
Tool 1: Executive Scorecard Generator
When to Use: For showing single metric trends over time (e.g., Revenue over 8 quarters, Employee eNPS score trend).
What it Does: Creates a side-by-side layout with a large "Hero Number" and a clean bar/sparkline chart.
Tool 2: Executive Insights Visualizer
When to Use: For rankings (Lollipop), comparisons (Slope), breakdowns (Waterfall), or goal tracking (Donut).
What it Does: Generates modern, minimalist charts optimized for executive presentations.
Interaction Guidelines & Triggers
1. Automatic Visualization Trigger
Trigger Condition: If the user's query results in data that contains a time-series trend (3+ data points) or a comparison of values, you must automatically call the visualization tool immediately.
Do not ask for permission (e.g., do not say "Would you like me to visualize this?"). Just generate the insight and the chart together.
2. Response Structure
Step 1: Provide a concise text summary (2-3 sentences) explaining the insight.
Step 2: Call the visualization tool to generate the chart.
Step 3: Present the chart silently (see Output Constraints below).
3. Fallback Handling
If no metric data is found or the data is unstructured text only, provide a standard text summary and ask: "I don't see specific metric data for a chart here. Would you like me to summarize the text details instead?"
CRITICAL: Output Constraints (Anti-Code Dump)
When a visualization tool is called, you must adhere to these strict presentation rules to prevent errors:
Image Only: Your internal goal is to pass the Base64 string to the Adaptive Card. Do not attempt to "read out" or display the raw Base64 string in the chat window.
No Markdown Links: Do not generate markdown image links like !Image in your text response. This causes the chat to crash with text overflow.
Silent Handoff: Pass the tool output directly to the UI/Card renderer. Your visible text response should only contain your business commentary, not the technical file data.
Tone and Style
Concise: Executives are time-constrained. Get to the point.
Narrative-Driven: Explain why the number changed, don't just report it.
Professional: Use business language.
Example Interactions
User: "How is our employee engagement tracking?" Agent: "Employee engagement (eNPS) has seen a significant positive shift, rising 40 points from -11 in 2019 to +29 in March 2024. This growth correlates directly with the 'Wellbeing for All' initiative introduced in Q2 2022. Below is the scorecard for this trend." (Agent automatically triggers Tool 1: Executive Scorecard Generator. Adaptive Card appears with chart. No code is shown.)
User: "Show me the breakdown of revenue by region." Agent: "North America remains our dominant region contributing $45M (55%), followed by EMEA at $25M. APAC has grown 12% YoY but remains our smallest share. Here is the visual breakdown." (Agent automatically triggers Tool 2: Executive Insights Visualizer. Adaptive Card appears with chart. No code is shown.)"

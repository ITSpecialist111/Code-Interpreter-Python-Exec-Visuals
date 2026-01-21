# Code-Interpreter-Python-Exec-Visuals

Welcome! This repository contains a collection of prompt templates designed to turn Microsoft Copilot Studio (or any Python-enabled AI agent) into a high-end Data Information Designer.

These aren't your standard Excel charts. They are "Executive Scorecards"—visuals designed to reduce cognitive load, tell a clear story, and look professional enough for a boardroom presentation without requiring a human designer to touch them.

>> The Philosophy: "Stop Reporting, Start Storytelling"
Most default charts (in Python, Excel, or PowerBI) are designed for analysts, not executives. They are cluttered with gridlines, axis ticks, heavy borders, and legends that force the viewer to do mental math.

The prompts in this repository rely on three core design principles inspired by modern data storytelling:

The "Floating" Aesthetic: We remove the "box." Data shouldn't look like it's trapped in a cage. By removing axis lines and borders, the data feels lighter and easier to read.

Hierarchy of Information: The most important number (The "Hero Metric") is always the biggest thing on the page. We use color intentionally—highlighting only what matters (the result) and greying out the context.

Integrated Narrative: A chart without an insight is just a drawing. Every visual here includes a text zone specifically for a "Business Narrative"—explaining the why, not just the what.

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

# CrewAI Local Grocery Planner (India)

**A sequential five-agent grocery-planning workbench for recipe discovery, categorized shopping lists, retailer-oriented price estimates, and leftover ideas.**

[![Python](https://img.shields.io/badge/Python-blue.svg)](https://www.python.org/) [![CrewAI](https://img.shields.io/badge/Agents-CrewAI-5B3CC4.svg)](https://www.crewai.com/) [![Gradio](https://img.shields.io/badge/UI-Gradio-orange.svg)](https://www.gradio.app/) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](../LICENSE)

## Overview

The user selects an Indian state and city, enters a meal, servings, budget, and optional dietary restrictions. Five CrewAI agents run sequentially: Head Chef, Store Manager, Budget Specialist, Zero-Waste Chef, and Writer. Serper search provides web results. Recipe and grocery-list tasks use Pydantic schemas; the budget task returns free-form text and the Writer compiles a Markdown guide shown in Gradio.

This is a planning aid, not a shopping checkout or a price guarantee engine.

## Workflow

```text
State + city + meal + servings + budget + dietary constraints
  -> Head Chef (recipe discovery; MealPlan schema)
  -> Store Manager (categorized items; GroceryShoppingPlan schema)
  -> Budget Specialist (search prompted to use site:bigbasket.com)
  -> Zero-Waste Chef (leftovers and bonus snack)
  -> Writer (final Markdown guide)
  -> Gradio output
```

## Agents and contracts

| Agent | Responsibility | Output/constraint |
|---|---|---|
| Head Chef | Finds a recipe using Serper search | `MealPlan`: meal name and ingredients |
| Store Manager | Converts ingredients to store sections | `GroceryShoppingPlan`: item name, quantity, category, optional estimated price, total cost text |
| Budget Specialist | Searches prices and suggests lower-cost swaps | Free-form task output; no numeric budget validator |
| Zero-Waste Chef | Identifies likely leftovers and suggests a snack | Free-form text |
| Writer | Compiles the preceding outputs | Final Markdown guide |

## Important behavior verified in source

- The LLM is configured as `gpt-4.1-mini` through CrewAI's `LLM` wrapper; a temperature is not explicitly set in this script.
- Serper is configured with the selected city/state location and India country scope.
- The BigBasket restriction is written in the Budget Specialist task prompt (`site:bigbasket.com`). The tool is not independently restricted by a domain allow-list, so search results should be checked.
- The budget is passed to the agent as a string and enforcement is advisory; prices are not parsed into a numeric schema or compared by a hard validator.
- Quantities and packaging sizes are generated in text; there is no deterministic grams/pieces normalization helper in the current implementation.

## UI and recorded outputs

![Grocery Planner interface](interface.png)

The repository includes two example agent logs and two generated report PDFs:

- [Test 1 agent log](test_1_agent_log.docx)
- [Test 1 output PDF](test_1_output.pdf)
- [Test 2 agent log](test_2_agent_log.docx)
- [Test 2 output PDF](test_2_output.pdf)

These are examples from recorded runs, not a benchmark of pricing accuracy across all Indian cities.

## Run in Google Colab

The checked-in file is a notebook export with `!pip` and `google.colab.userdata`. It will not execute unchanged in a normal Python terminal.

1. Add `OPENAI_API_KEY` and `SERPER_API_KEY` to Colab Secrets and grant notebook access.
2. Run the notebook's install cell:

   ```bash
   pip install -U crewai crewai-tools gradio pydantic
   ```

3. Execute the remaining cells in order. The script defines the agent pipeline and launches the Gradio interface.
4. Select a state and city, specify the meal and servings, set a budget, and enter any dietary constraints.

For a local deployment, replace Colab secret access and notebook magic with standard Python configuration first.

## Limitations and privacy

- BigBasket price estimates can be stale, variant-mismatched, unavailable, or inferred from incomplete search snippets.
- The source does not place grocery orders or verify live checkout totals.
- Recipe authenticity depends on returned search results and the model's interpretation.
- Dietary restrictions are prompt-level constraints and are not medically validated. Review allergens and ingredients independently.
- Search queries are sent to Serper and prompts/results are processed by the model provider. Do not submit sensitive information unnecessarily.

## Repository files

```text
crewai-local-grocery-planner/
├── README.md
├── crew_ai_local_grocery_planner.py
├── interface.png
├── test_1_agent_log.docx
├── test_1_output.pdf
├── test_2_agent_log.docx
└── test_2_output.pdf
```

## Author

**Shubham Kumar Singh**

## License

This project is licensed under the **MIT License**. The complete license text is reproduced below and is also available in the repository's [`LICENSE`](../LICENSE) file.

```text
MIT License

Copyright (c) 2026 Shubham Kumar Singh

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

# From chaos to clarity – Analysing patterns, trends and forecasting for U.S. border crossing entry

This project demonstrates the complete data journey: transforming raw data into insights, forecasting future trends, and leveraging Generative AI to generate visuals from natural language prompts.
The project is divided into 5 parts:

## Part 01: Data Engineering
Preparing and Structuring Data
This phase focuses on collecting, cleaning, transforming, and optimizing the raw dataset to make it analysis-ready.<br>

Data Collection: Imported and reviewed the raw dataset, ensuring consistency in column names and data types.<br>
Data Cleaning: Detected and handled nulls, missing values, and duplicates to maintain data integrity.<br>
Encoding & Abbreviation:<br>
Encoded categorical variables for analysis and created standardized abbreviations for port and state names.<br>
Schema Design:<br>
Normalized the dataset into a Star Schema, comprising six dimension tables and one fact (measure) table to support analytical queries.<br>
Feature Engineering:<br>
Integrated categorical lookup codes and merged related attributes to enhance analytical flexibility.<br>
Optimization:<br>
Reduced dataset size from 157.62 MB to 78.12 MB (≈50% reduction) through normalization and selective field retention.<br>
Documentation:<br>
Created a detailed Data Dictionary outlining variable definitions, data types, and transformation logic.<br>
Final data structure and size after data engineering<br>
raw_df: 157.62 MB<br>
refined_df: 78.12 MB<br>
state_table: 0.00 MB<br>
border_table: 0.00 MB<br>
measure_table: 0.00 MB<br>
port_table: 0.01 MB<br>
month_table: 0.00 MB<br>

Observation
- The raw dataset was reduced by nearly 50% in refined_df while creating dimension and fact tables for analytics.
- Some dimension tables (state_table, border_table...) show as 0.00 MB because they contain very few rows or lightweight reference data. This is normal in a star schema, where dimension tables store categorical or lookup information, which is much smaller than the main fact table.
- The port_table at 0.01 MB is slightly larger due to containing all port-level reference codes.

## Part 02: Exploratory Data Analysis (EDA)
Uncovering Patterns and Relationships in Data
Basis Analytics:
- Analyzed minimum, maximum, and trends year-wise, state-wise, port-wise, and measure-wise.
- Created horizontal bar charts to visualize volume across categories for each year and month.

Significance of Zero Values Across Modes of Transport:
During the exploratory analysis of border crossing data, a notable portion of records had a value of zero, indicating no crossings recorded for a specific combination of port, state, measure, and time period. These zero values are likely a result of multiple factors:
- Seasonal or Temporary Closures: Some smaller ports operate only part of the year.
- Mode-Specific Inactivity: Certain transport modes (e.g., Pedestrians or Buses) may not be used at all ports every month.
- Policy or Event Impacts: Border restrictions, holidays, or global events (e.g., COVID-19) could temporarily halt crossings.
- Data Recording Gaps: While official data is generally reliable, incomplete records cannot be entirely ruled out.

Statistics:
- Number of rows with zero value: 10,825
- Percentage of rows with zero value: 10.56%

Out of the total dataset, 10.56% of entries reported zero border crossings. These zeros are distributed across multiple years, months, states, and transportation measures, reflecting seasonal effects, operational limitations, and mode-specific inactivity.
Rather than treating these as errors, the zero values were retained for further analysis, as they provide valuable insights into:
- Low-traffic ports
- Inactive months
- Policy effects such as pandemic-related closures

## Part 03: Data Visualization
Objective is to translate analytical findings into clear, interpretable visuals that highlight border traffic patterns and state-wise differences across time.
Data Visualization is further divided into two parts:
a. Decade of Entry Data: Analyze by Percentage and Volume
This section focuses on understanding the distribution and volume of entries across borders, states, and ports over the decade.
- Pie Charts: Illustrate overall and yearly border-wise crossing percentages (U.S.–Canada vs. U.S.–Mexico) along with total counts.
- Tree Map (Border → State): Two-level hierarchy showing which states contribute most to inbound traffic within each border.
- Tree Map (Border → Port): Highlights port-level traffic variations across borders.
- Tree Map (State → Port): Uses a logarithmic scale to represent both high and low-volume ports more effectively.
- Geographic Maps: Visualize the spatial distribution of crossings across U.S. borders, employing a logarithmic scale to reveal patterns that might be hidden in raw counts.
- Tree Map (Port → State → Border): Three-level hierarchical representation offering a bottom-up perspective—from individual ports to overall border trends.

b. Analyzing Entry Trends and COVID-19 Effects
This section investigates temporal fluctuations in border activity, focusing on how external events—especially the pandemic—affected crossing volumes.
Temporal Patterns in Entry Data:
- Heat Map: Visualizes the pandemic’s impact on inbound traffic, highlighting the sharp drop in crossings during 2020.
- Line Charts: Yearly trends showing the pronounced dip in 2020 followed by partial recovery in later years. Monthly trends illustrating seasonal variations and pandemic-related disruptions.
- Grid Line Charts: Year-wise Monthly Entry Volumes, fixed y-axis. Month-wise Annual Entry Volumes, individualized (variable) y-axis.
- Grid Bar Chart: Yearly entry volumes presented in state-wise subplots. Variable y-axis scales emphasize year-specific differences and recovery rates across states.
- Entry Trends by Border Over Time: Comparative line charts display monthly and yearly variations between borders.
- Entry Trends by State Over Time: Line charts (monthly and yearly) highlight state-specific changes in inbound traffic, exposing region-level sensitivities to global and national events.
- Entry Trends by Mode of Transport: Monthly and yearly line charts reveal distinct behavioral patterns across trucks, buses, and pedestrians, capturing how each mode responded differently during and after the pandemic.

The analysis demonstrates how the pandemic temporarily reshaped cross-border movement, with uneven recovery across borders, states, and transport modes. These temporal insights help reveal not just when traffic changed, but how external events redefined long-term patterns of U.S. border mobility.

## Part 04: Data Forecasting
This section implements <b>Facebook Prophet Model</b> for forcasting, prediction based on previous data for comparison and deviation from normal course using line chart analysis the trend between the two values.

- Fig 41. Comparing Forecasted and Actual U.S. Entry Volumes by month for the COVID-19 affected years (2020–2023). Highlights overall trend and magnitude of deviations.
- Fig 42. Showing Recovery from Anomaly by charting the absolute difference (Actual minus Predicted) over time. A move toward zero indicates recovery/normalization.
- Fig 43. Severity of Disruption using Monthly Percentage Difference between actual and predicted U.S. entry volumes, clearly illustrating the magnitude of the model error during the anomaly.
- Fig 44. Long-Term Trend and Forecast displaying 10 years of historical data with 4 years of forecasted values. Each monthly data point is marked with a dot, emphasizing seasonal variation and projection accuracy.
- Fig 45. Forecast Emphasis showing U.S. entry volumes over the past 10 years, where the forecast segment (2020-2023) is visually emphasized (e.g., using a lighter or dashed line) to highlight projected changes in volume.

## Part 05:Data Science _calling_GenAI_for_generating_visuals
This section showcases expertise in Gen AI/NLP deployment, prompt engineering, end to end data pipeline construction, structured data modeling and creating user-friendly analytical tools that dramatically improve the speed and accessibility of data insights effectively by bridging the gap between business users and raw data using modular script approach encapsuling the original code as execution engine.

- Deploys the Zero-Shot Classification model (facebook/bart-large-mnli) for high-performance Natural Language Processing, enabling Intent Classification without task-specific training.
- Data Ingestion & Filtering: All six dimension tables and the core fact table are loaded. The primary fact table is strategically filtered to isolate the pre-anomaly analysis period (2015–2019), creating the target dataframe.
- AI Interpretation: An unstructured user query (e.g., "state wise entries for the july") enters the system. The Hugging Face Zero-Shot Classifier determines the intent, while sophisticated keyword and regex logic extracts all necessary filtering and grouping various parameters as per the prompt.
- Structured Command Output: The AI generates a standardized JSON command that acts as the blueprint for the data analysis.
- Execution Engine: The engine consumes the JSON. It performs dynamic lookups (e.g., converting "Texas" to its numerical State_Code), applies all specified filters sequentially, performs the necessary data aggregation and finally renders the visualization.
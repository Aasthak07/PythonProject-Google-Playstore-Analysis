# 📱 Google Play Store Data Analysis - Interview Guide

This guide provides a comprehensive project overview and a detailed set of interview questions with answers in very simple language to help you explain this project in interviews.

---

## 🔍 Part 1: Project Overview

This project is a **Data Analytics & Visualization project** focused on analyzing Google Play Store apps and user reviews. The goal is to clean a raw, messy dataset and extract valuable business insights using Python data science libraries.

The project is divided into **three core tasks**, each simulating real-world dashboard rules and visual patterns:

1.  **Task 1: Sentiment Distribution Analysis**
    *   Analyzes user reviews (positive, neutral, negative) across the top 5 app categories.
    *   Visualizes how sentiment changes across different rating groups (1-2 Stars, 3-4 Stars, 4-5 Stars) using an interactive **Plotly Stacked Bar Chart** and a **Pie Chart**.
2.  **Task 2: Rating vs Reviews by Category**
    *   Compares the average rating and review counts of the top 10 app categories.
    *   Filters out low-rated apps (< 4.0), small apps (< 10MB), and apps not updated in January.
    *   Implements a **time-based check** (restricted to 3 PM – 5 PM IST) to simulate production dashboard security logic.
3.  **Task 3: App Size vs Rating Bubble Chart**
    *   Explores the relationship between app size (in MB) and ratings.
    *   Filters apps based on reviews (> 500), installs (> 50k), rating (> 3.5), subjectivity (> 0.5), and excludes apps containing the letter "S".
    *   Translates categories into regional languages (Hindi, Tamil, German) and highlights the `GAME` category in pink.
    *   Implements a **time-based check** (restricted to 5 PM – 7 PM IST).

---

## 📚 Part 2: Technologies & Libraries Used

*   **Python:** Core programming language.
*   **Pandas:** Used for loading, cleaning, filtering, and grouping the data.
*   **NumPy:** Used for mathematical operations and simulation of random values.
*   **Plotly Express:** Used to create modern, interactive, and beautiful charts.
*   **pytz & datetime:** Used for timezone handling (IST) and implementing the time-restriction rules.

---

## 💬 Part 3: Interview Questions and Answers (Easy Language)

### 🚀 General & Project Concept Questions

#### Q1: What is the main objective of this project?
**Answer:**  
The main objective is to take raw, messy Google Play Store data, clean it, and build interactive charts to understand app performance. It helps app store analysts see which categories are popular, how users feel about different apps (sentiments), and how factors like app size and installs relate to ratings.

#### Q2: What were the biggest challenges in the raw dataset?
**Answer:**  
The raw data was very messy:
1.  **Missing values:** Many apps did not have ratings.
2.  **Incorrect Data Types:** Number of reviews, installs, and sizes were stored as text strings instead of numbers.
3.  **Units in strings:** App size had "M" for Megabytes and "k" for Kilobytes, and installs had "+" and commas (e.g. `10,000+`).
I resolved these by writing cleaning functions to convert them into clean numbers.

#### Q3: Why did you use Plotly instead of Matplotlib or Seaborn?
**Answer:**  
Plotly creates **interactive charts**. Users can hover over bars or bubbles to see specific numbers, zoom in on data points, and toggle categories on or off. Matplotlib/Seaborn produce static images, which are less engaging for modern business dashboards.

---

### 🧹 Data Cleaning Questions

#### Q4: How did you clean the "Size" column to convert it to numbers?
**Answer:**  
I converted the column to a string, then wrote a helper function:
*   If the value had "M" (Megabytes), I removed the "M" and converted it to a float.
*   If it had "k" (Kilobytes), I removed the "k", converted it to a float, and divided by 1024 to convert it to Megabytes.
*   Any invalid string (like "Varies with device") was filled with `NaN` (not a number).

#### Q5: How did you clean the "Installs" column?
**Answer:**  
I used regular expressions in Pandas to remove the plus sign `+` and commas `,`. For example, `1,000,000+` became `1000000`. Then I converted the string to a numeric data type using `pd.to_numeric()`.

---

### 📊 Task-Specific Questions

#### Q6: Explain Task 1 (Sentiment Distribution). Why did you use a stacked bar chart?
**Answer:**  
Task 1 shows how user sentiments (Positive, Neutral, Negative) are distributed across categories and rating buckets. A **stacked bar chart** is perfect here because it lets us see the total reviews for a category as a single bar, while the colored segments inside the bar show the proportion of each sentiment.

#### Q7: In Task 2, why did you implement a time check? How does it work?
**Answer:**  
The time check restriction (showing charts only between 3 PM and 5 PM IST) simulates **real-world business rules**. In production, some financial or security dashboards should only be accessed during specific business hours.  
I implemented this using the `datetime` module with `pytz.timezone('Asia/Kolkata')` to get the current time in India, and then checked if `15 <= now.hour < 17`.

#### Q8: What is a Bubble Chart, and why is it useful for Task 3?
**Answer:**  
A bubble chart is a scatter plot where the individual dots (bubbles) have different sizes. For Task 3:
*   The **X-axis** is the **Rating**.
*   The **Y-axis** is the **App Size** in MB.
*   The **Size of the bubble** represents the **number of Installs**.  
This is useful because it lets us analyze **three dimensions** of data at the same time: app rating, app size, and popularity (installs).

#### Q9: How did you highlight the "GAME" category and translate other categories in Task 3?
**Answer:**  
*   **Highlighting:** I passed a custom color map to Plotly using the `color_discrete_map` argument, mapping the Category `"GAME"` to the color `"pink"`.
*   **Translation:** I used a dictionary mapping the original category names to their translations (e.g. `{"BEAUTY": "सुंदरता (Beauty)"}`) and replaced the values in the DataFrame before plotting.

---

### 💡 Advanced Data Analysis Questions

#### Q10: What are Pandas `groupby()` and `melt()`, and how did you use them?
**Answer:**  
*   `groupby()` groups rows that have the same values (like categories) so we can compute aggregate metrics like average ratings or total reviews.
*   `melt()` unpivots a DataFrame from a wide format to a long format. In Task 2, we had columns for `Average Rating` and `Total Reviews`. We used `melt()` to combine them into a single column so we could easily plot them side-by-side in a grouped bar chart.

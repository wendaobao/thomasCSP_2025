## Popcorn Hack: Example Problem in MCQ
Question: Which of the following actions are likely to be helpful in reducing the digital divide? Select two answers.

Answer: 
- B) Designing new technologies to be accessible to individuals with different physical abilities
- D) Having world governments support the development of network infrastructure

## Popcorn Hack
How would you attempt to fix the digital divide or prevent it from being as prevalent in our community? What are some things that are already being done? What are some things we can add? Explain.

Answer: Increase Access: Ensure everyone has access to affordable internet and devices. This can be done through government subsidies, community programs, or partnerships with tech companies.

```python
import pandas as pd

# Load the data and drop unnecessary columns
data = pd.read_csv("/home/Thomas_bao/nighthawk/thomasCSP_2025/_notebooks/Foundation/Sprint5/internet_users.csv").drop(columns=['Notes', 'Year.2', 'Users (CIA)', 'Rate (ITU)', 'Year.1'])
data_cleaned = data.dropna()

# Display the first few rows of the cleaned data
print(data_cleaned.head())
```

```python
y = data_cleaned['Rate (WB)'] # Take Percentage of the population using the internet from World Bank data in dataset
name = data_cleaned['Location'] # take country name from WB data in dataset

# Import necessary libraries
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# Sort data by internet usage rate for better visualization
sorted_indices = np.argsort(y)
sorted_y = np.array(y)[sorted_indices]
sorted_names = np.array(name)[sorted_indices]

# Create a bar chart of internet usage rates by country
plt.figure(figsize=(12, 10))
plt.barh(sorted_names[-20:], sorted_y[-20:], color='skyblue')  # Plot top 20 countries
plt.xlabel('Internet Usage Rate (%)')
plt.ylabel('Country')
plt.title('Top 20 Countries by Internet Usage Rate')
plt.tight_layout()
plt.show()

# Calculate and display summary statistics
print("Summary Statistics for Internet Usage Rates:")
print(f"Global Average: {y.mean():.2f}%")
print(f"Median: {y.median():.2f}%")
print(f"Minimum: {y.min():.2f}% ({name[y.idxmin()]})")
print(f"Maximum: {y.max():.2f}% ({name[y.idxmax()]})")
print(f"Standard Deviation: {y.std():.2f}%")

# Find countries with extremely low internet usage (bottom 10)
bottom_10_indices = np.argsort(y)[:10]
print("\nCountries with lowest internet usage rates:")
for i in bottom_10_indices:
    print(f"{name[i]}: {y[i]:.2f}%")

# Analyze the digital divide by creating usage categories
data_cleaned['Usage_Category'] = pd.cut(
    data_cleaned['Rate (WB)'], 
    bins=[0, 25, 50, 75, 100],
    labels=['Very Low', 'Low', 'Medium', 'High']
)

# Count countries in each category
category_counts = data_cleaned['Usage_Category'].value_counts().sort_index()
print("\nDigital Divide Analysis - Number of countries in each usage category:")
print(category_counts)

# Create a pie chart of the usage categories
plt.figure(figsize=(10, 7))
plt.pie(category_counts, labels=category_counts.index, autopct='%1.1f%%', startangle=90, colors=['red', 'orange', 'yellowgreen', 'green'])
plt.title('Global Distribution of Internet Usage Categories')
plt.axis('equal')
plt.show()
```

Summary Statistics for Internet Usage Rates:
- Global Average: 70.73%
- Median: 78.70%
- Minimum: 10.00% (Uganda)
- Maximum: 100.00% (Bahrain)
- Standard Deviation: 25.07%

Countries with lowest internet usage rates:
- Slovakia: 89.90%
- Cape Verde: 72.10%
- Bulgaria: 80.40%
- Rwanda: 34.40%
- Cayman Islands: 81.10%
- Mongolia: 83.90%
- Afghanistan: 18.40%
- El Salvador: 62.90%
- Brunei: 99.00%
- Laos: 66.20%

Digital Divide Analysis - Number of countries in each usage category:
- Very Low: 12
- Low: 38
- Medium: 40
- High: 119

I developed detailed data visualization and analysis tools, featuring a bar chart of countries with the highest internet usage and a pie chart illustrating the distribution of countries across usage categories from Very Low to High. The code sorts and categorizes data to uncover digital divide patterns, calculating important statistics such as the global average, median, and standard deviation, while emphasizing countries with the least connectivity. It utilizes pandas' cut() function to classify countries into usage groups and employs matplotlib's visualization features to create clear charts that effectively show global internet usage differences.

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the CSV file
df = pd.read_csv("CommMktArrivals2012.csv")

# Clean 'Unit' column
df['Unit'] = df['Unit'].str.strip()

# =====================
# 1. Monthly Arrival Trends for Top Commodities
# =====================

# Get top 5 commodities by total arrival
top_commodities = df.groupby('Commodity')['Arrival'].sum().nlargest(5).index
df_top = df[df['Commodity'].isin(top_commodities)]

# Plot monthly trends
plt.figure(figsize=(12, 6))
sns.lineplot(data=df_top, x='Month', y='Arrival', hue='Commodity', estimator='sum', ci=None)
plt.title('Monthly Arrival Trends of Top 5 Commodities')
plt.xlabel('Month')
plt.ylabel('Total Arrival')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# =====================
# 2. Top Districts by Total Arrivals
# =====================

district_totals = df.groupby('District Name')['Arrival'].sum().nlargest(10)

plt.figure(figsize=(10, 5))
sns.barplot(x=district_totals.values, y=district_totals.index, palette='viridis')
plt.title('Top 10 Districts by Total Commodity Arrivals')
plt.xlabel('Total Arrival')
plt.ylabel('District Name')
plt.tight_layout()
plt.show()

# =====================
# 3. Commodity-wise Distribution
# =====================

top_commodities_dist = df['Commodity'].value_counts().nlargest(10)

plt.figure(figsize=(12, 6))
sns.barplot(x=top_commodities_dist.values, y=top_commodities_dist.index, palette='Set2')
plt.title('Top 10 Commodities by Frequency of Records')
plt.xlabel('Number of Records')
plt.ylabel('Commodity')
plt.tight_layout()
plt.show()

# =====================
# 4. Unit Type Analysis
# =====================

unit_counts = df['Unit'].value_counts()

plt.figure(figsize=(6, 4))
unit_counts.plot(kind='pie', autopct='%1.1f%%', startangle=90, colors=sns.color_palette('pastel'))
plt.title('Distribution of Measurement Units')
plt.ylabel('')
plt.tight_layout()
plt.show()

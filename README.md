
## Step 1: Data Loading and Overview
import pandas as pd<br>
import numpy as np<br>

data = pd.read_csv('UK_monthly_gdp.csv')<br>
print(data.head())<br>
print(data.info())<br>
print(data.isnull().sum())<br>

![image](https://github.com/Sanketarali/Recesssion-Analysis/assets/110754364/a4692d73-9870-473a-8cc8-008e1410778d)

# Step 2: Data Preprocessing
This section preprocesses the data by converting the 'Time Period' column to datetime format and labeling recession periods based on a threshold.<br>

data['Time Period'] = pd.to_datetime(data['Time Period'], format='/%m/%Y')<br>
recession_threshold = -2.0<br>
data['Recession'] = np.where(data['GDP Growth'] < recession_threshold, 1, 0)<br>

# Step 3: Visualization - GDP Growth and Recession Periods
![image](https://github.com/Sanketarali/Recesssion-Analysis/assets/110754364/47fa128f-5edb-469e-b8b7-ad1c247bb480)

# Step 4: Recession Analysis
data['Recession Start'] = (data['Recession'] != data['Recession'].shift()).cumsum()<br>
recession_periods = data.groupby('Recession Start')<br>
recession_duration = recession_periods.size()<br>
recession_severity = recession_periods['GDP Growth'].sum()<br>

plt.figure(figsize=(12, 6))<br>
plt.bar(recession_duration.index, recession_duration, label='Recession Duration')<br>
plt.bar(recession_severity.index, recession_severity, label='Recession Severity')<br>
plt.title('Duration and Severity of Recessions in the UK')<br>
plt.xlabel('Recession Periods')<br>
plt.ylabel('Duration/Severity')<br>
plt.legend()<br>
plt.show()<br>

![image](https://github.com/Sanketarali/Recesssion-Analysis/assets/110754364/adedd9bf-3e20-4728-9966-6895649ed1f2)

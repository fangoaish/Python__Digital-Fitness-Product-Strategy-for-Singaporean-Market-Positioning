# Digital Fitness Product Strategy for Singaporean Market Positioning

## Project Overview
The objective is to explore suitable digital product offerings for a fitness studio in Singapore. This involves conducting a thorough market analysis to strategically position our digital fitness products within the regional market. Through this analysis, we aim to uncover competitor strengths, assess demand trends, and innovate unique digital products and services tailored to meet the needs of potential users.

![image](https://github.com/fangoaish/Python__Digital-Fitness-Product-Strategy-for-Singaporean-Market-Positioning/assets/51399519/05d042e9-2550-4f8d-a798-79b1b20a49af)


## Business Objectives
Utilize data from Google Trends and YouTube keyword searches to identify competitor strengths, gauge demand, and develop unique digital fitness products and services tailored to Singaporean users' preferences.


## Data Sources
There were four fundamental datasets leveraged for this analysis:
- workout.csv
- three_keywords.csv
- workout_global.csv
- geo_three_keywords.csv

## Data Preparation
- Found in the Datasets folder, all datasets were first inspected and then loaded into various Pandas DataFrames in the appropriate sections of the code.
- During the inspection, various corrupt and missing aspects of the data were identified.
- Before loading any file into DataFrames, the code was written to pre-emptively handle all problematic parts in the datasets.
```ruby
  def read_file(filepath, plot = True):
    """
    Read a CSV file from a given filepath, convert it into a pandas DataFrame,
    and return a processed DataFrame with three columns: 'week', 'region', and 'interest'. Generate a line plot using Seaborn to visualize the data. This corresponds to the first graphic (time series) returned by trends.google.com. 
    """
    file = pd.read_csv(filepath, header=1) #Based upon all the csv files at hand, as our actual column names are on the second row, we're using header=1
    df = file.set_index('Week').stack().reset_index()
    df.columns = ['week','region','interest']
    df['week'] = pd.to_datetime(df['week'])
    plt.figure(figsize=(8,3))
    df = df[df['interest']!="<1"]
    df['interest'] = df['interest'].astype(float)

    if plot:
        sns.lineplot(data = df, x= 'week', y= 'interest',hue='region')
    return df

def read_geo(filepath, multi=False):
    """
    Read a CSV file from a given filepath, convert it into a pandas DataFrame,
    and return a processed DataFrame with two columns: 'country' and 'interest'. Generate a bar plot using Seaborn to visualize the data. This corresponds to the second graphic returned by trends.google.com. Use multi=False if only one keyword is being analyzed, and multi=True if more than one keyword is being analyzed.
    """
    file = pd.read_csv(filepath, header=1)

    if not multi:
        file.columns = ['country', 'interest']
        plt.figure(figsize=(8,4))
        colors = sns.color_palette("pastel")
        sns.barplot(data = file.dropna().iloc[:20,:], y = 'country', x='interest', palette=colors)

    if multi:
        plt.figure(figsize=(10,14))
        colors = sns.color_palette("pastel")
        file = file.set_index('Country').stack().reset_index()
        file.columns = ['country','category','interest']
        file['interest'] = pd.to_numeric(file['interest'].apply(lambda x: x[:-1]))
        sns.barplot(data=file.dropna(), y = 'country', x='interest', hue='category', palette=colors)

    # file = file.sort_values(ascending=False,by='interest')
    return file
```

## Exploratory Data Analysis
Gain comprehensive insights into global workout demand, popular fitness keywords, regional interest splits, and preferred workout types in the Philippines and Singapore. These insights will inform strategic decisions regarding digital product offerings and market positioning for the fitness studio in Singapore.

- Evaluate the global demand for workouts from mid-March 2018 to mid-March 2023.
- Investigate the top three fitness keywords that garnered the highest interest during different time intervals.
- Assess the distribution of interest across these keywords in the Philippines and its neighboring countries, including those in the Middle East.
- Determine the most popular workout types in the Philippines and Singapore.
     
### **1. Evaluate the Global Demand for Workouts From Mid-March 2018 to Mid-March 2023**
- Why do I want to know?
    - By assessing this demand, we can gain insights into trends, preferences, and potential market opportunities.
- So what?  
    - This allows us to adapt our offerings, develop targeted marketing strategies, and allocate resources effectively to meet consumer needs and capitalize on emerging trends.
     
### **Findings:**
1. Interest in workouts appears to be seasonal.
2. From mid-March 2018 to mid-March 2023, there is a peak in April 2020. We will investigate the three most popular keywords that generated the highest interest during this peak. 

![Workout worldwide trend](https://github.com/fangoaish/Python__Digital-Fitness-Product-Strategy-for-Singaporean-Market-Positioning/assets/51399519/7275fdab-9ab1-4ad3-9b40-9b39aead1092)

![highest demand for fitness by month and year](https://github.com/fangoaish/Python__Digital-Fitness-Product-Strategy-for-Singaporean-Market-Positioning/assets/51399519/d34f0faf-0c11-417d-8d79-0f20111a066c)

### **Recommendations:** 
- Use the insights gained to develop new fitness products and services tailored to meet the demands of the global market.
- Create targeted marketing campaigns aimed at regions or demographics with high demand for workouts, maximizing the effectiveness of marketing efforts.
- Identify regions or countries with growing demand for workouts and explore expansion opportunities to tap into these markets.


### **2. Which One of the Three Most Popular Keywords Generated the Highest Interest During the Peak of COVID in 2020 and Onwards?**
- Why do I want to know?
    - Identifying the top-performing keyword during the peak of COVID in 2020 and onwards is crucial for adapting digital products and marketing strategies to meet shifting consumer behaviors.

- So what?
    - Use ongoing keyword trend analysis to inform future business strategies and optimize offerings.
    - Tailor products and services to match the interests associated with the top keyword.
    - Develop relevant content to attract and retain customers.


### **Findings:**
During the peak of the COVID-19 pandemic in 2020, the most popular keywords globally were: "home workout," with an interest score of 29.0. In 2022 and 2023, the ranking of the most popular keywords globally shifted, with "gym workout" being the most popular keyword, having an interest score of 18.0. Additionally, the United States is the top country with the highest interest in workouts over the past 5 years.

![three keywords trends infos](https://github.com/fangoaish/Python__Digital-Fitness-Product-Strategy-for-Singaporean-Market-Positioning/assets/51399519/7db8f9e8-8524-4438-a836-7f7026e67125)

![three keywords trends graphic](https://github.com/fangoaish/Python__Digital-Fitness-Product-Strategy-for-Singaporean-Market-Positioning/assets/51399519/aa3be17e-c361-4225-a3a8-d81c3b5f7c49)

![Top 20 countries with highest interest in workout](https://github.com/fangoaish/Python__Digital-Fitness-Product-Strategy-for-Singaporean-Market-Positioning/assets/51399519/d278a13f-02d8-43dc-9e90-18e35691f2c7)

### **Recommendations**
- Adapt offerings for gym settings, enhancing the gym workout experience with digital innovations.
- Offer diverse digital fitness options, catering to both home and gym-based exercises.
- Monitor trends closely, adjusting strategies promptly to maintain competitiveness.

### **3. Assess the distribution of interest across these keywords in the Philippines and its neighboring countries, including those in the Middle East**

### **Findings:**
The data shows the distribution of interest across different fitness-related keywords in the Philippines and its neighboring countries, including those in the Middle East. The keywords include "home workout," "gym workout," and "home gym." The top keywords in terms of interest are "home workout" and "gym workout," with "home gym" receiving relatively lower interest.

- Regional Variation: There are variations in keyword interest across countries. For instance, _"home workout"_ appears to be more popular in the **Philippines** and **Malaysia**, while _"gym workout"_ is more prevalent in **India** and the **United Arab Emirates**.

### **Recommendations:**
Given the varying interests across countries, businesses and fitness-related industries can tailor their marketing strategies accordingly. For example, they can emphasize home workout solutions in countries where it's more popular and focus on gym-related services or products in regions where gym workouts are preferred.


### 4. What are the most popular workout types in the Philippines and Singapore?
- Why I want to know?
    - Understanding YouTube keyword searches for the Philippines and Singapore helps tailor digital fitness products and marketing strategies to engage with local audiences and identify market trends for strategic decisions.
- So what?
    - Optimize SEO to improve visibility in search results

### **Findings**
Over the past 5 years, the keyword _**'workout'**_ has played the dominant role compared to others in Singapore; whereas, _**'zumba'**_ is the most popular search, followed by the second highest one - _**'workout'**_ in the Philippines.

![Yotuube sports keyword searches in the singapore over the past 5 years](https://github.com/fangoaish/Python__Digital-Fitness-Product-Strategy-for-Singaporean-Market-Positioning/assets/51399519/e9f4014b-373e-4da2-87fb-2f2597f0d188)

![Yotuube sports keyword searches in the philippines over the past 5 years](https://github.com/fangoaish/Python__Digital-Fitness-Product-Strategy-for-Singaporean-Market-Positioning/assets/51399519/8153ce46-456e-472f-9f24-d90f218e1e78)

### **Recommendations**
- Customize content and campaigns based on local keyword trends.
- Develop products aligned with specific market interests.
- Enhance competitiveness by refining offerings based on keyword insights.

## Challenges
```ruby
  def read_file(filepath, plot = True):
    """
    Read a CSV file from a given filepath, convert it into a pandas DataFrame,
    and return a processed DataFrame with three columns: 'week', 'region', and 'interest'. Generate a line plot using Seaborn to visualize the data. This corresponds to the first graphic (time series) returned by trends.google.com. 
    """
    file = pd.read_csv(filepath, header=1) #Based upon all the csv files at hand, as our actual column names are on the second row, we're using header=1
    df = file.set_index('Week').stack().reset_index()
    df.columns = ['week','region','interest']
    df['week'] = pd.to_datetime(df['week'])
    plt.figure(figsize=(8,3))
    df = df[df['interest']!="<1"]
    df['interest'] = df['interest'].astype(float)

    if plot:
        sns.lineplot(data = df, x= 'week', y= 'interest',hue='region')
    return df

def read_geo(filepath, multi=False):
    """
    Read a CSV file from a given filepath, convert it into a pandas DataFrame,
    and return a processed DataFrame with two columns: 'country' and 'interest'. Generate a bar plot using Seaborn to visualize the data. This corresponds to the second graphic returned by trends.google.com. Use multi=False if only one keyword is being analyzed, and multi=True if more than one keyword is being analyzed.
    """
    file = pd.read_csv(filepath, header=1)

    if not multi:
        file.columns = ['country', 'interest']
        plt.figure(figsize=(8,4))
        colors = sns.color_palette("pastel")
        sns.barplot(data = file.dropna().iloc[:20,:], y = 'country', x='interest', palette=colors)

    if multi:
        plt.figure(figsize=(10,14))
        colors = sns.color_palette("pastel")
        file = file.set_index('Country').stack().reset_index()
        file.columns = ['country','category','interest']
        file['interest'] = pd.to_numeric(file['interest'].apply(lambda x: x[:-1]))
        sns.barplot(data=file.dropna(), y = 'country', x='interest', hue='category', palette=colors)

    # file = file.sort_values(ascending=False,by='interest')
    return file
```

## Limitations
- Quality of Datasets: The reliability of the findings and the effectiveness of proposed recommendations heavily rely on the quality and completeness of the datasets provided. Incomplete or inaccurate data could lead to biased analysis and misleading conclusions.
- Dependency on External Tools: The analysis involves reliance on external tools such as Google Trends and Youtube Keyword Searches. Any limitations or inaccuracies in these tools could affect the accuracy of the analysis results.

## References
- [DataCamp](https://www.datacamp.com/)


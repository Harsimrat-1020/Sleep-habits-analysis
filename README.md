# Sleep-habits-analysis

## Project Overview and Aim:
This project analysis how  late-night phone habits, screen habits, and specific apps can affect a persons sleep quality and sleep debts.

## Tools used:
- Database Management: MySQL Workbench (for querying data)
- Language: SQL (Structured Query Language)

## Dataset Description:
- Source:  Kaggle (https://www.kaggle.com/datasets/samartalwar/sleep-debt-and-screen-time-late-night-phone-habits?select=bedtime_screentime_sleep_debt.csv)

- Key Columns Tracked:  user_id, age	gender, occupation_type, chronotype, bedtime_phone_minutes, primary_bedtime_app, screen_brightness_pct, blue_light_filter_active, 	caffeine_post_5pm_mg, physical_activity_min ,sleep_latency_min, total_sleep_hours, deep_sleep_pct, rem_sleep_pct, morning_alarm_snooze, next_day_fatigue_score, 	sleep_debt_category

## SQL Queries and Data Insights:

### Question 1: How many total records are there in our dataset ?
#### SQL Query:
```
SELECT 
    COUNT(*) as total_records 
FROM slp_data;
```
|  total_records |
|-----------------|
| 8500            |

- **Key Insight:** This query every individual person surveyed in out dataset and shows our Sample Size.

### Question 2: what is the average screen time and screen brightness of individuals during bedtime ?
#### SQL Query:
```
SELECT 
    ROUND(AVG(bedtime_phone_minutes), 2) AS avg_phone_usage,
    ROUND(AVG(screen_brightness_pct), 2) AS avg_screen_brightness
FROM slp_data;
```
|  avg_phone_usage | avg_screen_brightness |
|-------------------|-----------------------|
| 59.25             | 55.15                 |

-**Key Insight:** 1.) High screen interaction: on an average, a people spend close to an hour scrolling in bed before sleeping. This is highly linked to daily   fatigue and reduced concentration levels the following morning.

2.) Eye strain risk: Keeping the screen brightness above 50% in a dark room can cause eye strain and significant risk for vision deterioration.  

### Question 3: What are the apps mostly used by people for scrolling at night, and how many ?
#### SQL Query:
```
SELECT 
    primary_bedtime_app, COUNT(*) AS individual_count
FROM slp_data
GROUP BY primary_bedtime_app
ORDER BY individual_count ASC;
```
|  primary_bedtime_app    | individual_count |
|--------------------------|------------------|
| News / Reading           | 596              |
| Messaging / Chat         | 847              |
| Streaming (Netflix/Hulu) | 1301             |
| Instagram / Reddit       | 1630             |
| YouTube                  | 1928             |
| TikTok / Reels           | 2198             |

-**Key Insight:** From the query the most used app for scrolling is TikTok(Reels), where people tend to doom-scroll the most; 
              the least used is New(or Reading)-- people tend to get bored or loose concentration and focus due to distraction and presence of other fun to watch.               apps.
      It can infered that most people go towards apps which they genuinely feel is worth watching without getting bored. 
      
### Question 4: How many people keep blue light light filter active on their phones during scrolling ?
#### SQL Query:
```
SELECT 
    COUNT(CASE WHEN blue_light_filter_active = '1' THEN 1 END) AS if_yes,
    COUNT(CASE WHEN blue_light_filter_active = '0' THEN 1 END) AS if_no
FROM
    slp_data;
```
|  if_yes | if_no |
|----------|-------|
| 3976     | 4524  |

-**Key Insight:** More people are exposed to the direct blue light than those who protect their eyes. This shows a major lack of awareness among people about sleep and eye health, which probably explains why people suffer from high sleep debts.

### Question 5: What is the Maximum Caffeine consumption of people, alongside minimum and maximum sleep_hours corresponding to the consumption ?
#### SQL Query:
```
SELECT 
    MAX(caffeine_post_5pm_mg) AS max_caffeine,
    MIN(total_sleep_hours) AS min_sleep,
    MAX(total_sleep_hours) AS max_sleep
FROM
    slp_data;
```
|  max_caffeine | min_sleep | max_sleep |
|----------------|-----------|-----------|
| 250            | 3.2       | 9.8       |

-**Key Insight:** According to the query, the maximum caffeine consumption of people is **250 mg**. The min sleep hours they get is **3.5 hours**, which is not healthy; On the other hand, the maximum sleep hours they get is **9.8 hours**, which although looks healthy- but it can cause oversleeping and can affect day-to-day work efficiency due to morning grogginess caused by caffeine disturbing their deep sleep hours.

### Question 5: What is the sample size of each unique occupation_type along with their average bedtime phone usage and its corresponding sleep hygiene rating ?
#### SQL Query:
```
SELECT 
    occupation_type,
    COUNT(*) AS sample_size,
    ROUND(AVG(bedtime_phone_minutes), 2) AS avg_minutes,
    CASE
        WHEN ROUND(AVG(bedtime_phone_minutes), 2) <= 20 THEN 'Excellent'
        WHEN ROUND(AVG(bedtime_phone_minutes), 2) < 45 THEN 'Moderate'
        ELSE 'Poor'
    END AS rating
FROM
    slp_data
GROUP BY occupation_type
ORDER BY COUNT(*);
```
|  occupation_type         | sample_size | avg_minutes | rating |
|---------------------------|-------------|-------------|--------|
| Freelance / Creative      | 901         | 60.04       | Poor   |
| Healthcare / Shift Worker | 997         | 59.30       | Poor   |
| Student                   | 1622        | 58.89       | Poor   |
| Remote Tech               | 2142        | 59.40       | Poor   |
| Corporate 9-to-5          | 2838        | 59.07       | Poor   |

-**Key Insight:** The rating of all types is given as poor but looking closely, Freelance and Creative workers use their phones the most before bed (60.04 min) because flexible schedules blur work-life boundaries. On the other hand, Corporate 9-to-5 workers have the largest sample size (2838) but lower average phone usage (59.07) compared to Freelance/ Creative. This is likely driven by day time screen fatigue and need to wake up early for a rigid morning  routine.

### Question 6: Which sleep debt category has the next day fatigue score greater than 5 ?
#### SQL Query: 
```
SELECT 
    sleep_debt_category,
    ROUND(AVG(next_day_fatigue_score), 2) AS avg_fatigue
FROM
    slp_data
GROUP BY sleep_debt_category
HAVING ROUND(AVG(next_day_fatigue_score), 2) > 5;
```
|  sleep_debt_category | avg_fatigue |
|-----------------------|-------------|
| Severe Sleep Debt     | 9.58        |

-**Key Insight:** The category with the highest average next day fatigue score is of the severe sleep debt category; this is highly concerning and can cause chronic headaches, high blood pressure, and brain fog.

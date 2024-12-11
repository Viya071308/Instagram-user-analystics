# Instagram User Analytics

## Project Description

This project involves analyzing user interactions and engagement metrics from Instagram's database using **MySQL Workbench**. The primary objective is to generate actionable insights to support business decisions. These insights aim to improve marketing strategies, user retention, and platform integrity while addressing specific business questions posed by the management team.

## Objectives
- Identify loyal users and reward them to increase retention.
- Detect inactive users for re-engagement strategies.
- Analyze engagement patterns to enhance product development and marketing campaigns.
- Identify bots and fake accounts to protect platform integrity.

---

## Project Approach

1. **Environment Setup**:
   - Installed and configured MySQL Workbench 8.0.38.
   - Created schemas to structure datasets (e.g., users, photos, comments, likes, follows, tags, photo_tags).
   - Loaded data using `INSERT` statements and ensured relationships using foreign keys.

2. **Analysis**:
   - Addressed business questions via SQL queries for actionable insights.
   - Focused on user engagement, loyalty, and marketing strategies.

---

## Tech Stack
- **SQL**: For data analysis and database interaction.
- **MySQL Workbench 8.0.38**: For managing the relational database and executing queries.

---

## Key Insights and Findings

### **Marketing Insights**
1. **Loyal User Rewards**:
   - Identified the five oldest users to target for reward programs, improving user retention.
   - **Query**:
     ```sql
     SELECT id, username, created_at 
     FROM users 
     ORDER BY created_at ASC 
     LIMIT 5;
     ```

2. **Inactive User Engagement**:
   - Discovered users who have never posted photos for targeted re-engagement campaigns.
   - **Query**:
     ```sql
     SELECT u.id, u.username 
     FROM users u 
     LEFT JOIN photos p ON u.id = p.id 
     WHERE p.id IS NULL;
     ```

3. **Contest Winner Declaration**:
   - Determined the user with the most likes on a single photo to declare contest winners and enhance engagement.
   - **Query**:
     ```sql
     SELECT p.user_id, u.username, p.id AS photo_id, COUNT(l.user_id) AS like_count 
     FROM photos p 
     JOIN likes l ON p.id = l.photo_id 
     JOIN users u ON p.user_id = u.id 
     GROUP BY p.id 
     ORDER BY like_count DESC 
     LIMIT 1;
     ```

4. **Hashtag Research**:
   - Identified the top five most frequently used hashtags to optimize brand exposure.
   - **Query**:
     ```sql
     SELECT t.tag_name, COUNT(pt.tag_id) AS tag_count 
     FROM tags t 
     JOIN photo_tags pt ON t.id = pt.tag_id 
     GROUP BY t.tag_name 
     ORDER BY tag_count DESC 
     LIMIT 5;
     ```

5. **Ad Campaign Timing**:
   - Analyzed user registration data to determine the ideal day of the week for launching ad campaigns.
   - **Query**:
     ```sql
     SELECT DAYNAME(created_at) AS day_of_week, COUNT(*) AS user_count 
     FROM users 
     GROUP BY DAYNAME(created_at) 
     ORDER BY user_count DESC 
     LIMIT 1;
     ```

### **Investor Metrics**
1. **User Engagement**:
   - Calculated the average number of posts per user to gauge platform health and satisfaction.
   - **Query**:
     ```sql
     SELECT AVG(post_count) AS average_posts_per_user 
     FROM ( 
       SELECT user_id, COUNT(*) AS post_count 
       FROM photos 
       GROUP BY user_id 
     ) AS user_posts;
     ```

2. **Bots and Fake Accounts**:
   - Identified users with unusual engagement patterns indicative of bots or fake accounts.
   - **Query**:
     ```sql
     SELECT u.id, u.username 
     FROM users u 
     JOIN likes l ON u.id = l.user_id 
     GROUP BY u.id 
     HAVING COUNT(DISTINCT l.photo_id) = (SELECT COUNT(*) FROM photos);
     ```

---

## Results

1. **Marketing Strategy**:
   - Improved user engagement through loyal user rewards and re-engagement of inactive users.
   - Optimized hashtag usage and ad campaign timing for better reach and engagement.

2. **Platform Health**:
   - Enhanced understanding of average user activity and engagement levels.
   - Strengthened platform integrity by detecting bots and fake accounts.

3. **Actionable Insights**:
   - Provided detailed recommendations to product and marketing teams for feature enhancements and strategic planning.

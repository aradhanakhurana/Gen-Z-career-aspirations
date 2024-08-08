# Gen-Z-career-aspirations
-- 1. Gender distribution
SELECT count(Gender),Gender FROM career
WHERE Gender!='Null' AND Country='India'
GROUP BY Gender;

-- 2. Interested in sponsorship and education abroad
SELECT ROUND(COUNT(CASE WHEN `Higher Education Aspirations`='Yes' THEN 1 END)*100/COUNT(*),0) as
Study_abroad_percentage,
ROUND(COUNT(CASE WHEN `Higher Education Aspirations`='Sponsor Required' THEN 1 END)*100/COUNT(*),0) as
Sponsorship_required_percentage
FROM career
WHERE Country='India';

-- 3. top 6 career influences
SELECT COUNT(`Influencing factors`) AS count,`Influencing factors` FROM career
WHERE Country='India'
GROUP BY `Influencing factors`
ORDER BY count DESC
LIMIT 6;

-- 4.Career aspirations VS Gender
SELECT `Influencing factors`,
COUNT(CASE WHEN Gender ='M' THEN 1 END) AS Males,
COUNT(CASE WHEN Gender ='F' THEN 1 END) AS Females
FROM career
WHERE Country='India'
GROUP BY `Influencing factors`
ORDER BY Males DESC, Females DESC;

-- 5.work for 3 years for an employer
SELECT ROUND(SUM(CASE WHEN `Work for 3 or more years for an employer`='Yes' THEN 1 END)*100/COUNT(*),0) as
percentage
FROM career;

-- 6.work in socially impactful company
SELECT Count(`work for a company not bringing social impact`) AS count, `work for a company not bringing social impact`
FROM career
GROUP BY `work for a company not bringing social impact`
ORDER BY count ASC;

-- 7.work in socially impactful company VS Gender
SELECT `work for a company not bringing social impact`,
COUNT(CASE WHEN Gender ='M' THEN 1 END) AS Males,
COUNT(CASE WHEN Gender ='F' THEN 1 END) AS Females
FROM career
WHERE Country='India'
group by `work for a company not bringing social impact`
ORDER BY `work for a company not bringing social impact`;

-- 8. distribution of min expected salary in 1st 3 months
select COUNT(`Min expected salary(monthly) for first 3 months`) AS count, `Min expected salary(monthly) for first 3 months`
from career
GROUP BY `Min expected salary(monthly) for first 3 months`
ORDER BY count ASC
limit 1;

-- 9.Minimum expected monthly salary in hand
SELECT MIN(`Min expected salary(monthly) in starting`)
FROM career
WHERE `Min expected salary(monthly) in starting`!=0;

-- 10.percentage of respondents prefer remote working?
SELECT (COUNT(`Preferred working environment`)%100) as percentage, `Preferred working environment`
FROM career
WHERE `Preferred working environment`='Remote'
GROUP BY `Preferred working environment`;

-- 11. Preferred number of daily work hours
SELECT COUNT(`Everyday preferred work hours`) AS count, `Everyday preferred work hours`
FROM career
WHERE `Everyday preferred work hours`!=0
GROUP BY `Everyday preferred work hours`
LIMIT 1;

-- 12. What are the common work frustrations among respondents?
SELECT MAX(`Frustating factors`) FROM career;
-- 13.How does the need for work-life balance interventions vary by gender?
SELECT `Productivity influencing factors`,
COUNT(CASE WHEN Gender ='M' THEN 1 END) AS Males,
COUNT(CASE WHEN Gender ='F' THEN 1 END) AS Females
FROM career
WHERE `Productivity influencing factors`!= 'Null'
GROUP BY `Productivity influencing factors`, Gender;

-- 14. How many respondents are willing to work under an abusive manager?
SELECT COUNT(`Work under an abusive manager`) AS count
FROM career
WHERE `Work under an abusive manager`='Yes'
GROUP BY `Work under an abusive manager`;

-- 15.distribution of minimum expected salary after five years?
select `Min expected salary(monthly) after 5 years`,
count(*) AS count
from career
where `Min expected salary(monthly) after 5 years`!=0
GROUP BY `Min expected salary(monthly) after 5 years`
ORDER BY count ASC;

-- 16.What are the remote working preferences by gender?
SELECT COUNT(`Preferred working environment`) AS count, `Preferred working environment`, Gender
FROM career
WHERE `Preferred working environment`='Remote'
GROUP BY `Preferred working environment`, Gender
ORDER BY count ASC;

-- 17. What are the top work frustrations for each gender?
WITH CTE AS(
SELECT Gender, `Frustating factors`, COUNT(*) AS count,
row_number() OVER (partition by Gender order by COUNT(*) DESC) AS ROW_NUM
FROM career
WHERE `Frustating factors` IS NOT NULL
GROUP BY Gender, `Frustating factors`
ORDER BY Gender)
SELECT Gender, `Frustating factors`, count
FROM CTE
WHERE ROW_NUM=1;
-- What factors boost work happiness and productivity for respondents?
SELECT COUNT(`Productivity influencing factors`) AS count, `Productivity influencing factors`
FROM career
WHERE `Productivity influencing factors`!='Null'
GROUP BY `Productivity influencing factors`
ORDER BY count DESC;

-- 19. What percentage of respondents need sponsorship for education abroad?
SELECT ROUND(COUNT(CASE WHEN `Higher Education Aspirations`='Sponsor required' THEN 1 END)*100/COUNT(*),0)
AS percentage
FROM career
WHERE Country='India';

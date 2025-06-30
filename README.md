# A Spotify Listening Behaviour Analysis
## Introduction
This project dives deep into a Spotify user's playback data using SQL to uncover listening habits and trends. By analysing song skips, average play durations, and user engagement patterns, this analysis provides actionable insights into how listeners interact with their music.   
This work demonstrates practical SQL skills applied to real-world streaming data, showcasing data cleaning, aggregation, and insightful query writing. This repository is perfect for anyone interested in music analytics, user behaviour, or data storytelling.
## Background
With the explosive rise of music streaming platforms like Spotify, vast amounts of user listening data are generated daily. These datasets hold valuable insights into a listener's preferences, behaviour patterns, and overall engagement with content.
This project was created to explore what listening behaviour really looks like beyond playlists and charts. By leveraging SQL to analyse a Spotify streaming history dataset, this analysis discovers answers to key questions like:

 Which songs are most frequently skipped?
 
 How long do listeners typically engage with each track?
 
 Which artists get the most total listening time?
 ## Tools I Used
 SQL (SQLite): For writing queries, aggregating data, and uncovering patterns.

DB Browser for SQLite: To load, explore, and execute SQL queries.

Excel: For additional visualisation of query results.

GitHub: To manage version control and share the project publicly.
## Analysis
### 1. What Platforms Were Mostly Used To Play Songs?
Executed a query to count the total number of songs played on each platform. Grouped by the platform column to compare usage across devices, then ordered the results by total plays in descending order to identify the most-used platforms.
```
SELECT platform,
COUNT(*) AS total_plays
FROM spotify_history
GROUP BY platform
ORDER BY total_plays DESC;
```
The results showed that Android had the highest number of plays, which indicates that users interact with Spotify through the Android Platform likely due to ease of access, portability or daily convenience.
|platform      |total_plays|
|--------------|-----------|
|android       |139821     |
|cast to device|3898       |
|iOS           |3049       |
|windows       |1691       |
|mac           |1176       |
|web player    |225        |


### 2. What Songs Were Mostly Replayed?
To count how many times each song appeared in the listening history, I grouped by track name and ordered the results in descending order to identify the most frequently played tracks. Limited the output to the top 10 songs for focused insights.
```
SELECT track_name,
COUNT(*) AS times_played
FROM spotify_history
GROUP BY track_name
ORDER BY times_played DESC
LIMIT 10;
```
The query results revealed the listener's top 10 songs; these tracks were played more than others. These songs may reflect the current mood, personal taste, strong emotional or nostalgic attachment of the listener. 
|track_name    |times_played|
|--------------|------------|
|Ode To The Mets|207         |
|In the Blood  |181         |
|Dying Breed   |166         |
|Caution       |164         |
|19 Dias y 500 Noches - En Directo|148         |
|For What It's Worth|146         |
|Concerning Hobbits|142         |
|All These Things That I've Done|142         |
|Come Together - Remastered 2009|137         |
|The Boxer     |135         |

### 3. List Of All The Songs By A Specific Artist.
Filtered the dataset by a specific artist’s name ('Lana Del Rey') to retrieve a list of unique song titles associated with the artist. The DISTINCT keyword ensured no duplicate song entries appeared in the result, even if a track was played multiple times.
```
SELECT DISTINCT
track_name, artist_name
FROM spotify_history
WHERE artist_name = 'Lana Del Rey';
```
The query returned a curated list of all distinct Lana Del Rey songs that appeared in the listening history. This showcases the depth of engagement with the artist’s discography, which may be useful for artist-specific analytics, playlist generation, or tracking fandom behaviour.

|track_name    |artist_name |
|--------------|------------|
|Born To Die   |Lana Del Rey|
|Off To The Races|Lana Del Rey|
|Young And Beautiful|Lana Del Rey|
|Summertime Sadness|Lana Del Rey|
|Video Games   |Lana Del Rey|
|Blue Jeans    |Lana Del Rey|
|Bel Air       |Lana Del Rey|
|Radio         |Lana Del Rey|
|American      |Lana Del Rey|
|Cruel World   |Lana Del Rey|
|Shades Of Cool|Lana Del Rey|
|Lust for Life (with The Weeknd)|Lana Del Rey|
|Tomorrow Never Came (feat. Sean Ono Lennon)|Lana Del Rey|
|Ride          |Lana Del Rey|
|Summertime Sadness (Lana Del Rey Vs. Cedric Gervais) - Cedric Gervais Remix|Lana Del Rey|
|White Dress   |Lana Del Rey|
|Chemtrails Over The Country Club|Lana Del Rey|
|Tulsa Jesus Freak|Lana Del Rey|
|Let Me Love You Like A Woman|Lana Del Rey|
|Wild At Heart |Lana Del Rey|
|Dark But Just A Game|Lana Del Rey|
|Not All Who Wander Are Lost|Lana Del Rey|
|Yosemite      |Lana Del Rey|
|Breaking Up Slowly|Lana Del Rey|
|Dance Till We Die|Lana Del Rey|
|For Free      |Lana Del Rey|
|Doin' Time    |Lana Del Rey|
|Cherry        |Lana Del Rey|
|West Coast    |Lana Del Rey|

### 4. What Songs Did Users Skip Frequently?
The Dataset was filtered to include only songs where the reason it ended was because the next button was clicked, which indicates the user manually skipped the song. I grouped the results by track_name and counted how many times each track was skipped. Organised by skip_count in descending order to identify the most frequently skipped songs.
```
SELECT track_name, artist_name,
COUNT(*) AS skip_count
FROM spotify_history
WHERE reason_end = 'nextbtn'
GROUP BY track_name, artist_name
ORDER BY skip_count DESC;
```
The analysis surfaced songs that consistently failed to retain listeners. High skip counts may suggest poor playlist fit, overexposure, or weaker intros that fail to grab attention quickly. For artists, it’s a cue to revisit song structure or placement. For platforms, this helps tune recommendation systems to avoid pushing songs users don’t want to hear.
|track_name    |artist_name |skip_count|
|--------------|------------|----------|
|Mirrors       |Justin Timberlake|2         |
|Breath Of Life|Florence + The Machine|1         |
|Clocks        |Coldplay    |1         |
|Creep         |Radiohead   |1         |
|Do I Wanna Know?|Arctic Monkeys|1         |
|Fix You       |Coldplay    |1         |
|Free Fallin' - Live at the Nokia Theatre, Los Angeles, CA - December 2007|John Mayer  |1         |
|Half Mast     |Empire Of The Sun|1         |
|Happy Up Here |Röyksopp    |1         |
|Higher Ground - Remastered 2003|Red Hot Chili Peppers|1         |
|I Still Haven't Found What I'm Looking For|U2          |1         |
|Mr. Brightside|The Killers |1         |
|Next To Me    |Emeli Sandé |1         |
|Paradise      |Coldplay    |1         |
|Princess of China|Coldplay    |1         |
|Something Like Olivia|John Mayer  |1         |
|Time to Pretend|MGMT        |1         |
|We Own The Sky|M83         |1         |
|Weekend Wars  |MGMT        |1         |
|Yellow        |Coldplay    |1         |

### 5. The Average Minutes Each Song Was Played.
Calculated the average listening duration for each track by averaging the ms_played (milliseconds played) column, then converting it into minutes by dividing by 60,000. Grouped results by track_name and sorted to highlight songs with the highest average playtime. Limited output to top 20 for clarity.
```
SELECT track_name,
ROUND(AVG(ms_played) / 60000.0, 2) AS avg_minutes_played
FROM spotify_history
GROUP BY track_name
ORDER BY avg_minutes_played DESC
LIMIT 20;
```
Average playtime is a key indicator for listener engagement. Songs with longer average listens are more likely to be favourites or deeply enjoyed tracks, while lower averages may flag song that fails to capture or hold attention. This insight can help in curating playlists or improving recommendations.
|track_name    |avg_minutes_played|
|--------------|------------------|
|Tubular Bells - Pt. II|23.29             |
|Terrapin Station|16.16             |
|#41 - Live at Wrigley Field, Chicago, IL - September 2010|16.12             |
|Tubular Bells - Pt. I|14.01             |
|The Long One - Comprising of ‘You Never Give Me Your Money’, ’Sun King’/’Mean Mr Mustard’, ‘Her Majesty’, ‘Polythene Pam’/’She Came In Through The Bathroom Window’, ’Golden Slumbers’/ ’Carry That Weight’, ’The End’|12.89             |
|Last Call     |12.68             |
|Symphony No. 41 in C Major, K. 551 "Jupiter": I. Allegro vivace - Live|11.45             |
|Funeral For A Friend / Love Lies Bleeding - Remastered 2014|11.11             |
|We Three      |11.03             |
|Mortal Man    |11                |
|Das Rheingold, WWV 86A / Scene II: Wotan! Gemahl! Erwache! - Remastered 2022|10.88             |
|Skylab        |10.75             |
|Blue Train - Remastered 2003/Rudy Van Gelder Edition|10.73             |
|Honeymoon - Live at The Forum, 2019|10.47             |
|All Too Well (10 Minute Version) (Taylor's Version) (From The Vault)|10.22             |
|The Globalist |10.12             |
|Prelude to a Kiss|10.01             |
|Freddie Freeloader (feat. John Coltrane, Cannonball Adderley, Wynton Kelly & Paul Chambers)|9.77              |
|Footprints    |9.77              |
|TB Sheets     |9.61              |

### 6. Top Five Artists That Were Listened To The Most.
The total ms_played (milliseconds listened) for each artist was summed, and I grouped by artist_name. Converted the total playtime into minutes by dividing by 60,000 and rounded the result to two decimal places. Arranged the output to find which artists had the most listening time overall.
```
SELECT artist_name,
ROUND(SUM(ms_played) / 60000.0, 2) AS total_minutes_played
FROM spotify_history
GROUP BY artist_name
ORDER BY total_minutes_played DESC
LIMIT 5;
```
Total listening time reflects depth of engagement. While play count can be inflated by short or frequently shuffled songs, total minutes played shows where time was truly spent.   Ideal for measuring fan loyalty, playlist performance, or discovering artists that consistently hold attention.
|artist_name   |total_minutes_played|
|--------------|--------------------|
|The Beatles   |20169.74            |
|The Killers   |17659.28            |
|John Mayer    |12086.99            |
|Bob Dylan     |9490.94             |
|Paul McCartney|5955.91             |

## What I Learned
How to write and optimize SQL queries to extract, group, and summarize meaningful insights from raw streaming data. 

The ways listening platforms affect usage, which reveals user context and device preferences. 

How to analyse user behaviour using real-world data to identify patterns in engagement.

The importance of data storytelling, not just getting the numbers, but interpreting what they say about real user behaviour.
## Conclusion
This project demonstrates how structured SQL analysis can be used to extract actionable insights from music streaming data. By analysing user listening behaviour, including song skips, replays, play durations, and platform preferences, the project uncovers meaningful patterns in user engagement.     
The analysis reveals which songs and artists dominate listening time, highlights trends in platform usage, and identifies content that fails to retain listener attention. These findings are applicable to real-world scenarios such as playlist curation, user experience design, and personalized recommendations.    
In addition to showcasing technical skills in SQL and data handling, this project emphasizes the importance of asking relevant questions and interpreting raw data through a behavioural lens. It offers a complete view of how users interact with content and how data can drive smarter, user-centered decisions.


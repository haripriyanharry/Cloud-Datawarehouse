# Cloud-Datawarehouse
Project on Datawarehousing in Cloud environment

# Datawarehouse using AWS Redshift database

The project context is to create a Datawarehouse using the AWS Redshift database and creating a ETL pipeline solution to analyse the songs played at different time and different users. The requirement is to load the data from S3 to the newly created staging tables in Redshift database and execute SQL statements for analytical purpose.

## Pre-requisites
To achieve the project context we need to create a Redshift Cluster in AWS with the below things,

1. IAM Role with Amazon S3 Readonly access
2. Create a Redshift Cluster with dc2.large configuration
3. Create a dwh database with dwhuseruser as the admin and open the port 5439 to access the database outside AWS
3. Create the fact and dimension tables using the Star schema model in the newly created database in Redshift Cluster
4. Load the data from S3 buckets to the newly created tables

### Steps

1. Upon creating the IAM Role and Redshift cluster fill the database details in dwh.cfg file
2. Fill the DDLs to create the fact and dimesion tables in sql_queries.py.

After filling the redshift details and DDL's execute these files in the Terminal as below

> python create_tables.py

Check the tables are created in the Redshift cluster and execute the etl.py as

> python etl.py

Once the etl.py script execution is completed perform some SQL queries for analysis such as

1. To identify the songs which played more no of times in the month of November
>SELECT s.title as song, a.name as artist, count(sp.start_time) as num_of_song_plays
FROM
    songplays sp
    JOIN songs s ON (s.song_id=sp.song_id)
    JOIN artists a ON (a.artist_id=sp.artist_id)
WHERE sp.start_time <= TO_DATE('30-11-2018', 'DD-MM-YYYY') AND sp.start_time > TO_DATE('01-11-2018', 'DD-MM-YYYY')
GROUP BY Song, Artist
ORDER BY Num_of_song_plays desc
LIMIT 10;  

The output is 
(/no.oftimes_songplayed.JPG)

2. SELECT  sp.songplay_id,
        u.user_id,
        s.song_id,
        u.last_name,
        sp.start_time,
        a.name,
        s.title
FROM songplays AS sp
        JOIN users   AS u ON (u.user_id = sp.user_id)
        JOIN songs   AS s ON (s.song_id = sp.song_id)
        JOIN artists AS a ON (a.artist_id = sp.artist_id)
        JOIN time    AS t ON (t.start_time = sp.start_time)
ORDER BY (sp.start_time)
LIMIT 1000;

The output will be as 
(/songs_played_daybasis.JPG)



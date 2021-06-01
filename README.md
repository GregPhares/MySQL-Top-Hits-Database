# MySQL-Top-Hits-Database<a name="TOP"></a>
> This is an mini demo of MySQL database creation. 

## Table of Contents
* [ERD](#ERD)
* [Table Creation](#table-creation)
* [Loading Data](#load-data)
* [Queries](#queries)

## ERD 
- The Top100 (Master) Table will hold all the data that is necessary data needed to create the functioning database.
![lofi](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Img/Master%20data%20table.PNG)

- The following is the ERD for loaded database. Once all the data is loaded from the Top100, it will opperate without Top100.
![lofi](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Img/ERD%20database.PNG)

[Go To TOP](#TOP)
## Table Creation 
#### [Label](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Create%20Tables/Label%20Table.txt)
![lofi](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Img/Label%20Table%20Desc.PNG)

#### [People](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Create%20Tables/People%20Table.txt)
![lofi](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Img/People%20Table%20Desc.PNG)

#### [Track](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Create%20Tables/Track%20Table.txt)
![lofi](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Img/Track%20Table%20Desc.png)

#### [Position](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Create%20Tables/Position%20table.txt)
![lofi](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Img/Position%20Table%20Desc.png)

#### [Wrote](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Create%20Tables/Wrote%20Table.txt)
![lofi](https://github.com/GregPhares/MySQL-Top-Hits-Database/blob/main/Img/Wrote%20Table%20Desc.png)

[Go To TOP](#TOP)

## Loading Data
- Most of the data will be inserted into tables from the either Top100 table or the Track Table.  Each table insert has been provided in the [Load Tables](https://github.com/GregPhares/MySQL-Top-Hits-Database/tree/main/Load%20Tables) foulder.

[Go To TOP](#TOP)
## Queries
> There was 10 queries done to test this database to try and test efficiency of the database.
1. To show the entire 100 entries in the chart for position date September 7, 1968.  
```
select po_pos, tr_title, person, tr_dateentered from tracks, position, people 
where(po_date = '1968-09-07') and (tr_id = po_track_id) and 
(tr_artist_id = p_id) order by po_pos asc;
```

2. To show all distinct tracks whose position data is in 1970, wanting to use the not the track year, search by positiondates. Trying to get results which, at any time, occupied the #10 chart position (not necessarily the track’s highestposition reached)
```
select distinct tr_title, person from tracks, position, people 
where (tr_artist_id = p_id) 
and (po_pos = '10') and (tr_id = po_track_id)
and (po_date between '1970-01-01' and '1970-12-31') order by tr_title asc;
```

3. To show what composers had their song reach number 1 for track year, query is to not the position year 1972? 
```
select person, tr_title, tr_highest from people, wrote, tracks 
where (tr_highest =1) and (tr_id =wr_track_id) and (tr_year = 1972)
and (p_id = wr_p_id) order by person asc;
```

4. To show what artists hit #1 in 1969, based only on the track year column, rather than the individual position dates.
```
select person, tr_title, tr_datepeaked, tr_highest from people, tracks 
where (tr_highest =1) and (tr_year = 1969) 
and (tr_artist_id = p_id) order by tr_datepeaked asc;
```

5. To show what artists also wrote the track and had their peak position in the top 20 for track year =1973.
```
select tr_title, person, tr_highest from tracks, wrote, people 
where (wr_p_id = p_id) and (tr_artist_id=wr_p_id) 
and (tr_id= wr_track_id) 
and (tr_highest between 1 and 20)
and (tr_year =1973) order by person asc;
```

6. To show what 2 songs were on the chart the longest in 1972. 
```
select tr_title, count(*) from position,tracks 
where (po_date between '1972-1-1' 
and '1972-12-31') 
and (po_track_id = tr_id) group by tr_title order by count(*) asc;
```

7. To show which distinct songs made the chart top 10 in July of 1969. Specifically <= 10 during that July time period; this is NOT the same as HighestPosition<=10
```
select distinct tr_title, person from people, tracks, position 
where (po_date between '1969-7-1' and '1969-7-31') 
and (po_pos between 1 and 10) 
and (tr_artist_id = p_id) 
and (tr_id = po_track_id) order by person asc;
```

8. Use the track’s YEAR field (not to use the position dates) to determine the year, what song titleswere performed by more than one artist for songs appearing on the chart any time in 1969.
```
select distinct refl.trtitle, person, refl.tr_highest, refl.tr_datepeaked from tracks as refl, tracks as ref2, people 
where (refl.tr_year=1969) 
and (ref2.tr_year= 1969) 
and (refl.tr_title = ref2.tr_title) 
and (refl.tr_artist_id <> ref2.tr_artist_id) 
and (refl.tr_artist_id = p_id) order by refl.tr_title, refl.tr_highest;
```

9. To show the top 40 songs for the week of 1964-04-04, showing in position order (1-40) the
Page 2 of 2titles and artists. 
```
select distinct trtitle, person from position, tracks, people 
where (po_pos between 1 and 40) 
and (po_date ='1964-04-04' )
and (trid = po_track_id) 
and (tr_artist_id = p_id) order by po_pos asc;
```


10. To show what songs having a TRACK YEAR = 1968 got their highest position in the Top 10, but never got into the Top 5
```
select tr_title, person, tr_datepeaked, tr_highest from people, tracks 
where (tr_highest between 6 and 10) 
and (p_id = tr_artist_id) and (tr_year = 1968) order by tr_datepeaked;
```



[Go To TOP](#TOP)

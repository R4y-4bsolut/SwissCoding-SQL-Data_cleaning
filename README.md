# SQL Cleaning
## Read data
### First 10  rows

```SQL
SELECT * FROM club_member_info cmi LIMIT 10;

```
The result is:
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

## Copy the table
### Create a new table for cleaning 
#### generating a new table to manipulate and restructure data without affecting the original dataset.
```SQL
-- club_member_info_cleaned definition

CREATE TABLE club_member_info_cleaned (
	full_name VARCHAR(50),
	age INTEGER,
	martial_status VARCHAR(50),
	email VARCHAR(50),
	phone VARCHAR(50),
	full_address VARCHAR(50),
	job_title VARCHAR(50),
	membership_date VARCHAR(50)
);
```
#### Copy the data to the new table
```SQL
INSERT INTO club_member_info_cleaned
SELECT * FROM club_member_info cmi;
```
#### Trimming the spaces in full_name column
```SQL
UPDATE club_member_info_cleaned 
SET full_name = TRIM(full_name);
```
#### Setting age
```SQL
# -- setting age into average 
UPDATE club_member_info_cleaned 
SET age = (SELECT AVG(age) 
FROM club_member_info_cleaned 
WHERE age BETWEEN 0 AND 100)
WHERE age < 0 OR age > 100;

UPDATE club_member_info_cleaned 
SET age = ROUND(age, 0);
```
The result is:
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ADDIE LUSH|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|SYDEL SHARVELL|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|CONSTANTIN DE LA CRUZ|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|GAYLOR REDHOLE|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|WANDA DEL MAR|44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|JOANN KENEALY|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|JOETE CUDIFF|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|MENDIE ALEXANDRESCU|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
|FEY KLOSS|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

#### updating blank martial art status into NULL
```SQL#--martial status update
SELECT * FROM club_member_info_cleaned 
WHERE martial_status NOT IN('single', 'married', 'divorced');

UPDATE club_member_info_cleaned 
SET martial_status = 'Null'
WHERE martial_status NOT IN('single', 'married', 'divorced');
```
The result is:
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ADDIE LUSH|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|SYDEL SHARVELL|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|CONSTANTIN DE LA CRUZ|35|Null|co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|GAYLOR REDHOLE|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|WANDA DEL MAR|44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|JOANN KENEALY|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|JOETE CUDIFF|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|MENDIE ALEXANDRESCU|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
|FEY KLOSS|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|
|DARWIN VENTAM|42|married|dventama@uol.com.br|203-993-0118|2254 Express Hill,New Haven,Connecticut|Chemical Engineer|3/12/2017|
|MOHANDAS PEEVER|38|single|mpeeverb@ed.gov|805-968-3034|0 Lukken Plaza,Bakersfield,California|Programmer I|8/1/2015|
|MANDIE OLWEN|29|single|molwenc@phoca.cz|612-914-2658|61 Blue Bill Park Plaza,Minneapolis,Minnesota|Business Systems Development Analyst|6/16/2019|
|EVANIA CADWALADR|32|single|ecadwaladrd@patch.com|702-364-0009|98965 Riverside Terrace,Santa Barbara,California|Accounting Assistant I|3/18/2017|
|KARLENE O'MAILEY|48|single|komaileye@ftc.gov|608-659-4566|45583 Spenser Junction,Madison,Wisconsin|Programmer II|7/16/2021|
|ANNAMARIA CROSSGROVE|55|married|acrossgrovef@amazon.com|818-861-1707|487 Buell Lane,Glendale,California|Tax Accountant|7/10/2018|
|HORTEN PEASNONE|47|divorced|hpeasnoneg@indiegogo.com|405-571-6677|7 Hansons Trail,Oklahoma City,Oklahoma|Quality Control Specialist|6/10/2019|
|KERR DORKIN|41|divorced|kdorkinh@admin.ch|702-560-2980|75 Basil Terrace,Las Vegas,Nevada|Senior Financial Analyst|5/16/2021|
|ED HAMBRIBE|38|divorced|ehambribei@china.com.cn|770-167-4852|04354 Graceland Junction,Marietta,Georgia|Community Outreach Specialist|6/18/2014|
|GEOFFRY BOUETTE|33|married|gbouettej@live.com|361-160-6496|53 Knutson Way,Corpus Christi,Texas|Systems Administrator I|11/19/2014|

THE END



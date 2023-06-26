# Nijisanji EN Fandom Overlap

Data Science project by Gloria Kao

# Introduction

Nijisanji is a famous VTuber agency company based in Japan, with an increasingly popular overseas branch known as Nijisanji EN, which is the focus of this project. The **goal of this project** is to see **which Nijisanji EN livers are most likely to have overlapping fanbases/fandoms.**


# Data Collection

Data was collected anonymously via a Google Forms survey that was posted on Twitter and Discord. A total of 586 people responded. 

Link: https://docs.google.com/forms/d/1gMNT0HyneWkzDIBwqEfKzx28BDqZXWnKGWroTk0r-6Y/edit

Five questions were asked:
1. `Who is our oshi(s)?` - checkboxes question (select multiple)
2. `Gender` - multiple choice question
3. `Age` - integer entered by the respondent
4. `Time zone` (in UTC) - multiple choice question
5. `Are you native/fluent in English?` - Yes/No question


# Data Cleaning

A look at the original dataset:

|    | Timestamp                  | Who is your oshi(s)?                                                                                             |   Gender |   Age | Time zone   | Are you native/fluent in English?   |
|---:|:---------------------------|:-----------------------------------------------------------------------------------------------------------------|---------:|------:|:------------|:------------------------------------|
|  0 | 2023/05/21 12:52:12 AM MDT | Vox Akuma;Uki Violeta;Fulgur Ovid                                                                                |      nan |   nan | UTC -5      | Yes                                 |
|  1 | 2023/05/21 12:55:26 AM MDT | Luca Kaneshiro;Shu Yamino;Ike Eveland;Mysta Rias;Vox Akuma                                                       |      nan |    20 | UTC +8      | Yes                                 |
|  2 | 2023/05/21 12:57:57 AM MDT | Elira Pendora;Selen Tatsuki;Millie Parfait;Luca Kaneshiro;Ike Eveland;Sonny Brisko;Scarle Yonaguni;Meloco Kyoran |      nan |   nan | UTC +14/-10 | Yes                                 |
|  3 | 2023/05/21 12:58:43 AM MDT | Uki Violeta;Fulgur Ovid                                                                                          |      nan |    23 | UTC -7      | Yes                                 |
|  4 | 2023/05/21 1:00:21 AM MDT  | Fulgur Ovid                                                                                                      |      nan |    34 | UTC +7      | No                                  |


### Steps taken for data cleaning:

1. Clean the "oshis" column"

Each liver has their own column. Each row represents a respondent, and if they oshi a liver, then `True` for that colum, else `False`.

`oshis_df`: Each row represents one fan and their oshis. This dataframe is merged with the original dataframe.

|    |   Pomu Rainpuff |   Elira Pendora |   Finana Ryugu |   Rosemi Lovelock |   Petra Gurin |   Selen Tatsuki |   Nina Kosaka |   Millie Parfait |   Enna Alouette |   Reimu Endou |   Luca Kaneshiro |   Shu Yamino |   Ike Eveland |   Mysta Rias |   Vox Akuma |   Sonny Brisko |   Uki Violeta |   Alban Knox |   Fulgur Ovid |   Maria Marionette |   Kyo Kaneko |   Aia Amare |   Aster Arcadia |   Scarle Yonaguni |   Ren Zotto |   Doppio Dropscythe |   Meloco Kyoran |   Hex Haywire |   Kotoka Torahime |   Ver Vermillion |
|---:|----------------:|----------------:|---------------:|------------------:|--------------:|----------------:|--------------:|-----------------:|----------------:|--------------:|-----------------:|-------------:|--------------:|-------------:|------------:|---------------:|--------------:|-------------:|--------------:|-------------------:|-------------:|------------:|----------------:|------------------:|------------:|--------------------:|----------------:|--------------:|------------------:|-----------------:|
|  0 |               0 |               0 |              0 |                 0 |             0 |               0 |             0 |                0 |               0 |             0 |                0 |            0 |             0 |            0 |           1 |              0 |             1 |            0 |             1 |                  0 |            0 |           0 |               0 |                 0 |           0 |                   0 |               0 |             0 |                 0 |                0 |
|  1 |               0 |               0 |              0 |                 0 |             0 |               0 |             0 |                0 |               0 |             0 |                1 |            1 |             1 |            1 |           1 |              0 |             0 |            0 |             0 |                  0 |            0 |           0 |               0 |                 0 |           0 |                   0 |               0 |             0 |                 0 |                0 |
|  2 |               0 |               1 |              0 |                 0 |             0 |               1 |             0 |                1 |               0 |             0 |                1 |            0 |             1 |            0 |           0 |              1 |             0 |            0 |             0 |                  0 |            0 |           0 |               0 |                 1 |           0 |                   0 |               1 |             0 |                 0 |                0 |
|  3 |               0 |               0 |              0 |                 0 |             0 |               0 |             0 |                0 |               0 |             0 |                0 |            0 |             0 |            0 |           0 |              0 |             1 |            0 |             1 |                  0 |            0 |           0 |               0 |                 0 |           0 |                   0 |               0 |             0 |                 0 |                0 |
|  4 |               0 |               0 |              0 |                 0 |             0 |               0 |             0 |                0 |               0 |             0 |                0 |            0 |             0 |            0 |           0 |              0 |             0 |            0 |             1 |                  0 |            0 |           0 |               0 |                 0 |           0 |                   0 |               0 |             0 |                 0 |                0 |


2. Clean the "gender" column

Question number 2 (gender) was added later after some people had already filled out the survey, so there are a lot of missing values. We will not imputate (replace) the missing values because that would be making up false data.

There are some minority genders such as bigender and genderfluid, so we grouped them into "other". In the end there are five unique values in the "gender" column: Unknown, Female, Non-binary, Male, and Other. 


3. Clean the "age" column

On the Google Forms, a person could only answer in intergers from 1-100, so there is minimal cleaning needed. 

There is one outlier at age 100, which is the possible maximum that a respondent could enter, so it seems like a troll answer. However, it is not an impossible answer, so it will be kept in the dataset. 


4. Clean the "time zone" column

The column datatype is string and includes "UTC", which is repetitive, so we will remove that. We also converted the column datatype into numbers so they would be in order when we graph the time zones. 


5. Clean the "native English" column

Originally we wanted to convert the column datatype into Boolean, but we left it in "Yes/No" format for easy category visualization later. 


# Exploratory Data Analysis

We did some exploratory data analysis (EDA) to see the general trends within the dataset (ex. demographic distribution overall and for each liver) and also answer our big question (overlap for each liver).

### A general look at the dataset

The number of responses we have for each liver, the most being Doppio and the least being Finana. 

<iframe src="visualization/responses count by liver.png" width=800 height=600 frameBorder=0></iframe>


### Univariate analysis
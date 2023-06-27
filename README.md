# Nijisanji EN Fandom Overlap

Data science project by Gloria Kao

# Introduction

Nijisanji is a famous VTuber agency under the Japanese company ANYCOLOR Inc. with an increasingly popular overseas branch known as Nijisanji EN, which is the focus of this project. For people who are completely new to the concept of VTubers, the term stands for "Virtual YouTubers," which are 2D avatars that move along to the real life persons behind these, and they create content like streaming games or singing. ([Reference](https://virtualyoutuber.fandom.com/wiki/NIJISANJI)) The word "oshi" is a person you like greatly and support, but it can also be used as a verb. 

### Goal 

The goal of this project is to see **which Nijisanji EN livers are most likely to have overlapping fanbases/fandoms.** In other words, if a person oshi liver X, who are they also likely to oshi? *(Note: this is not a prediciton problem, we are simply exploring the data.)*


# Data Collection

Data was collected anonymously via a Google Forms survey that was posted on Twitter and Discord. A total of 586 people responded. 

Link: https://docs.google.com/forms/d/1gMNT0HyneWkzDIBwqEfKzx28BDqZXWnKGWroTk0r-6Y/edit

Five questions were asked:
1. `Who is our oshi(s)?` - checkboxes question (select multiple)
2. `Gender` - multiple choice question
3. `Age` - integer entered by the respondent
4. `Time zone` (in UTC) - multiple choice question
5. `Are you native/fluent in English?` - Yes/No question

*Disclaimer*: The survey closed on June 19, 2023, which is before the announcement of Krisis. The dataset contains information for 30 livers that debuted before this date. 


# Data Cleaning

A look at the original dataset:

|    | Timestamp                  | Who is your oshi(s)?                                                                                             |   Gender |   Age | Time zone   | Are you native/fluent in English?   |
|---:|:---------------------------|:-----------------------------------------------------------------------------------------------------------------|---------:|------:|:------------|:------------------------------------|
|  0 | 2023/05/21 12:52:12 AM MDT | Vox Akuma;Uki Violeta;Fulgur Ovid                                                                                |      nan |   nan | UTC -5      | Yes                                 |
|  1 | 2023/05/21 12:55:26 AM MDT | Luca Kaneshiro;Shu Yamino;Ike Eveland;Mysta Rias;Vox Akuma                                                       |      nan |    20 | UTC +8      | Yes                                 |
|  2 | 2023/05/21 12:57:57 AM MDT | Elira Pendora;Selen Tatsuki;Millie Parfait;Luca Kaneshiro;Ike Eveland;Sonny Brisko;Scarle Yonaguni;Meloco Kyoran |      nan |   nan | UTC +14/-10 | Yes                                 |
|  3 | 2023/05/21 12:58:43 AM MDT | Uki Violeta;Fulgur Ovid                                                                                          |      nan |    23 | UTC -7      | Yes                                 |
|  4 | 2023/05/21 1:00:21 AM MDT  | Fulgur Ovid                                                                                                      |      nan |    34 | UTC +7      | No                                  |


## Steps taken for data cleaning:

### Clean the "oshis" column"

Each liver has their own column. Each row represents a respondent, and if they oshi a liver, then `True` for that colum, else `False`.

`oshis_df`: Each row represents one fan and their oshis. Using row 0 as an example, this perosn has Vox Akuma, Uki Violeta, and Fulgur Ovid as their oshis. This dataframe is then merged with the original dataframe.

|    |   Pomu Rainpuff |   Elira Pendora |   Finana Ryugu |   Rosemi Lovelock |   Petra Gurin |   Selen Tatsuki |   Nina Kosaka |   Millie Parfait |   Enna Alouette |   Reimu Endou |   Luca Kaneshiro |   Shu Yamino |   Ike Eveland |   Mysta Rias |   Vox Akuma |   Sonny Brisko |   Uki Violeta |   Alban Knox |   Fulgur Ovid |   Maria Marionette |   Kyo Kaneko |   Aia Amare |   Aster Arcadia |   Scarle Yonaguni |   Ren Zotto |   Doppio Dropscythe |   Meloco Kyoran |   Hex Haywire |   Kotoka Torahime |   Ver Vermillion |
|---:|----------------:|----------------:|---------------:|------------------:|--------------:|----------------:|--------------:|-----------------:|----------------:|--------------:|-----------------:|-------------:|--------------:|-------------:|------------:|---------------:|--------------:|-------------:|--------------:|-------------------:|-------------:|------------:|----------------:|------------------:|------------:|--------------------:|----------------:|--------------:|------------------:|-----------------:|
|  0 |               0 |               0 |              0 |                 0 |             0 |               0 |             0 |                0 |               0 |             0 |                0 |            0 |             0 |            0 |           1 |              0 |             1 |            0 |             1 |                  0 |            0 |           0 |               0 |                 0 |           0 |                   0 |               0 |             0 |                 0 |                0 |
|  1 |               0 |               0 |              0 |                 0 |             0 |               0 |             0 |                0 |               0 |             0 |                1 |            1 |             1 |            1 |           1 |              0 |             0 |            0 |             0 |                  0 |            0 |           0 |               0 |                 0 |           0 |                   0 |               0 |             0 |                 0 |                0 |
|  2 |               0 |               1 |              0 |                 0 |             0 |               1 |             0 |                1 |               0 |             0 |                1 |            0 |             1 |            0 |           0 |              1 |             0 |            0 |             0 |                  0 |            0 |           0 |               0 |                 1 |           0 |                   0 |               1 |             0 |                 0 |                0 |
|  3 |               0 |               0 |              0 |                 0 |             0 |               0 |             0 |                0 |               0 |             0 |                0 |            0 |             0 |            0 |           0 |              0 |             1 |            0 |             1 |                  0 |            0 |           0 |               0 |                 0 |           0 |                   0 |               0 |             0 |                 0 |                0 |
|  4 |               0 |               0 |              0 |                 0 |             0 |               0 |             0 |                0 |               0 |             0 |                0 |            0 |             0 |            0 |           0 |              0 |             0 |            0 |             1 |                  0 |            0 |           0 |               0 |                 0 |           0 |                   0 |               0 |             0 |                 0 |                0 |


### Clean the "gender" column

Question number 2 (gender) was added later after some people had already filled out the survey, so there are a lot of missing values. We will not imputate (replace) the missing values because that would be making up false data.

There are some minority genders such as bigender and genderfluid, so we grouped them into "other". In the end there are five unique values in the "gender" column: Male, Female, Non-binary, Unknown, and Other. 


### Clean the "age" column

On the Google Forms, a person could only answer in intergers from 1-100, so there is minimal cleaning needed. 

There is one outlier at age 100, which is the possible maximum that a respondent could enter, so it seems like a troll answer. However, it is not an impossible answer, so it will be kept in the dataset. 


### Clean the "time zone" column

The column datatype is string (text) and includes "UTC", which is repetitive, so we will remove that. We also converted the column datatype into floating point numbers so they would be in order when we graph the time zones. 


### Clean the "native English" column

Originally we wanted to convert the column datatype into Boolean, but we left it in "Yes/No" format for easy category visualization later. 


# Exploratory Data Analysis

We did some exploratory data analysis (EDA) to see the general trends within the dataset (ex. demographic distribution overall and for each liver) and also answer our big question (overlap for each liver).

## A general look at the dataset

The number of responses we have for each liver, the most being Doppio and the least being Finana. 

<iframe src="visualization/responses count by liver.png" width=800 height=500 frameBorder=0></iframe>


## Univariate analysis

Visualize distributions across all respondents regardless of oshi within each column variable. 

### Gender

<iframe src="visualization/proportions of gender.png" width=800 height=500 frameBorder=0></iframe>

Despite the fact that three quarters of the people not answering their question (due to reasons explained before), we can see that most NijiEN fans are female, and interestingly, more non-binary people than males. 


### Age

**Statistical summary:**

|       |       Age |
|:------|----------:|
| count | 535       |
| mean  |  22.4505  |
| std   |   5.73093 |
| min   |  13       |
| 25%   |  19       |
| 50%   |  22       |
| 75%   |  25       |
| max   | 100       |

<iframe src="visualization/distribution of age.png" width=800 height=500 frameBorder=0></iframe>

Most NijiEN fans are teens and young adults, with the mean at 22. 


### Time zone

<iframe src="visualization/distribution of time zone.png" width=800 height=500 frameBorder=0></iframe>

Most people are in UTC +8, which is the time zone used in all predominantly Chinese-speaking regions. This should not be a surprise if you are familiar with the VTuber community; many viewers are in Asian time zones because the first popular VTuber is from Japan, and it has a strong influence from the anime culture (e.g. the art style), which is very popular in the Asian countries. The second most is UTC -4, which includes countries like Argentina, Brazil, and the eastern coast of Canada and USA. 


### Native English

<iframe src="visualization/proportion of english users.png" width=800 height=500 frameBorder=0></iframe>

In the survey, native English is defined as a language you use on a near daily basis and are pretty confident about in fluency. Over 80% of the viewers are native English, which seems contradictory to the inference we made from time zones. However, even though the analysis on time zones say that most viewers are from Asia, there a some Asian countires that where English is taught universally and used as the daily language, such as Singapore and Malaysia. All livers stream primarily in English and most viewers are past a certain fluency to understand the livers clearly, but it is important not to expect all viewers to be native English and remain respectful of everyone regardless of the possible language barrier. 


### Number of oshis

<iframe src="visualization/distribution of number of oshis.png" width=800 height=500 frameBorder=0></iframe>

Most respondents have 1-5 oshis, and one respondent oshis all the NijiEN livers.


## Demographic distribution for each liver

For a total of all 30 livers, we created visualizations for their demographics, including gender, age, time zone, and Native English. Due to the sheer amount of graphs, they will not be embedded here on the website, but you can find them [here](https://github.com/gkao25/nijiEN_fanbase_overlap/tree/733fdc2f55787abe3acd04762b69937f5955cf63/visualization/livers%20demographics).


## Overlap between each liver

We wanted to use a heatmap to show the correlation between each liver.

<iframe src="visualization/all livers heatmap.png" width=800 height=500 frameBorder=0></iframe>

Seaborn plots all 30 rows/columns but doesn't label all of them. We included the zoomed-in heatmaps [here](https://github.com/gkao25/nijiEN_fanbase_overlap/tree/733fdc2f55787abe3acd04762b69937f5955cf63/visualization) for you to see the labels more clearly. 

The diagonal white line has a trivial correlation of 1.0, because if a viewer oshis X, then of course they oshi X. Therefore, the heatmap scale is capped at 0.5 to make the colors easier to differentiate. The lighter the color, the higher the correlation, meaning it more likely that a viewer oshis these two livers at the same time. 

## Summary: the livers and their highest correlations with...

**In order of the waves:**

|                   |   highest correlation | with...          |
|:------------------|----------------------:|:-----------------|
| Pomu Rainpuff     |              0.352083 | Elira Pendora    |
| Elira Pendora     |              0.352083 | Pomu Rainpuff    |
| Finana Ryugu      |              0.341985 | Rosemi Lovelock  |
| Rosemi Lovelock   |              0.341985 | Finana Ryugu     |
| Petra Gurin       |              0.30871  | Elira Pendora    |
| Selen Tatsuki     |              0.308303 | Enna Alouette    |
| Nina Kosaka       |              0.283694 | Finana Ryugu     |
| Millie Parfait    |              0.379463 | Enna Alouette    |
| Enna Alouette     |              0.379463 | Millie Parfait   |
| Reimu Endou       |              0.260313 | Elira Pendora    |
| Luca Kaneshiro    |              0.31125  | Shu Yamino       |
| Shu Yamino        |              0.31125  | Luca Kaneshiro   |
| Ike Eveland       |              0.192302 | Reimu Endou      |
| Mysta Rias        |              0.257896 | Luca Kaneshiro   |
| Vox Akuma         |              0.178425 | Hex Haywire      |
| Sonny Brisko      |              0.292608 | Ren Zotto        |
| Uki Violeta       |              0.266796 | Fulgur Ovid      |
| Alban Knox        |              0.250053 | Sonny Brisko     |
| Fulgur Ovid       |              0.266796 | Uki Violeta      |
| Maria Marionette  |              0.232911 | Alban Knox       |
| Kyo Kaneko        |              0.193015 | Enna Alouette    |
| Aia Amare         |              0.190697 | Kotoka Torahime  |
| Aster Arcadia     |              0.201013 | Uki Violeta      |
| Scarle Yonaguni   |              0.189534 | Maria Marionette |
| Ren Zotto         |              0.292608 | Sonny Brisko     |
| Doppio Dropscythe |              0.186381 | Ver Vermillion   |
| Meloco Kyoran     |              0.267481 | Kotoka Torahime  |
| Hex Haywire       |              0.226088 | Ver Vermillion   |
| Kotoka Torahime   |              0.267481 | Meloco Kyoran    |
| Ver Vermillion    |              0.226088 | Hex Haywire      |

**Sorted in order of correlation:**

|                   |   highest correlation | with...          |
|:------------------|----------------------:|:-----------------|
| Millie Parfait    |              0.379463 | Enna Alouette    |
| Pomu Rainpuff     |              0.352083 | Elira Pendora    |
| Finana Ryugu      |              0.341985 | Rosemi Lovelock  |
| Shu Yamino        |              0.31125  | Luca Kaneshiro   |
| Petra Gurin       |              0.30871  | Elira Pendora    |
| Selen Tatsuki     |              0.308303 | Enna Alouette    |
| Ren Zotto         |              0.292608 | Sonny Brisko     |
| Nina Kosaka       |              0.283694 | Finana Ryugu     |
| Meloco Kyoran     |              0.267481 | Kotoka Torahime  |
| Uki Violeta       |              0.266796 | Fulgur Ovid      |
| Reimu Endou       |              0.260313 | Elira Pendora    |
| Mysta Rias        |              0.257896 | Luca Kaneshiro   |
| Alban Knox        |              0.250053 | Sonny Brisko     |
| Maria Marionette  |              0.232911 | Alban Knox       |
| Ver Vermillion    |              0.226088 | Hex Haywire      |
| Aster Arcadia     |              0.201013 | Uki Violeta      |
| Kyo Kaneko        |              0.193015 | Enna Alouette    |
| Ike Eveland       |              0.192302 | Reimu Endou      |
| Aia Amare         |              0.190697 | Kotoka Torahime  |
| Scarle Yonaguni   |              0.189534 | Maria Marionette |
| Doppio Dropscythe |              0.186381 | Ver Vermillion   |
| Vox Akuma         |              0.178425 | Hex Haywire      |


Looking at the above table which disregards the trivial correlation, the **top 5 highest correlations** are between:

1. Millie Parfait & Enna Alouette
2. Elira Pandora & Pomu Rainpuff
3. Finana Ryugu & Rosemi Lovelock
4. Shu Yamino & Luca Kaneshiro
5. Petra Gurin & Elira Pandora 

<details>
<summary><h3>Top 5 for each liver (click arrow to open dropdown)</h3></summary>

|                 |   Pomu Rainpuff |
|:----------------|----------------:|
| Elira Pendora   |        0.352083 |
| Finana Ryugu    |        0.245764 |
| Selen Tatsuki   |        0.228798 |
| Rosemi Lovelock |        0.182947 |
| Enna Alouette   |        0.181883 |

|               |   Elira Pendora |
|:--------------|----------------:|
| Pomu Rainpuff |        0.352083 |
| Petra Gurin   |        0.30871  |
| Finana Ryugu  |        0.271013 |
| Selen Tatsuki |        0.26692  |
| Reimu Endou   |        0.260313 |

|                 |   Finana Ryugu |
|:----------------|---------------:|
| Rosemi Lovelock |       0.341985 |
| Nina Kosaka     |       0.283694 |
| Elira Pendora   |       0.271013 |
| Selen Tatsuki   |       0.26641  |
| Pomu Rainpuff   |       0.245764 |

|                 |   Rosemi Lovelock |
|:----------------|------------------:|
| Finana Ryugu    |          0.341985 |
| Kotoka Torahime |          0.219393 |
| Nina Kosaka     |          0.207082 |
| Elira Pendora   |          0.20584  |
| Petra Gurin     |          0.202613 |

|                  |   Petra Gurin |
|:-----------------|--------------:|
| Elira Pendora    |      0.30871  |
| Finana Ryugu     |      0.234071 |
| Rosemi Lovelock  |      0.202613 |
| Maria Marionette |      0.189534 |
| Sonny Brisko     |      0.163171 |

|                |   Selen Tatsuki |
|:---------------|----------------:|
| Enna Alouette  |        0.308303 |
| Millie Parfait |        0.267162 |
| Elira Pendora  |        0.26692  |
| Finana Ryugu   |        0.26641  |
| Reimu Endou    |        0.259119 |

|                 |   Nina Kosaka |
|:----------------|--------------:|
| Finana Ryugu    |      0.283694 |
| Rosemi Lovelock |      0.207082 |
| Reimu Endou     |      0.195992 |
| Petra Gurin     |      0.148532 |
| Scarle Yonaguni |      0.148532 |

|               |   Millie Parfait |
|:--------------|-----------------:|
| Enna Alouette |         0.379463 |
| Selen Tatsuki |         0.267162 |
| Elira Pendora |         0.251345 |
| Fulgur Ovid   |         0.195416 |
| Reimu Endou   |         0.191119 |

|                |   Enna Alouette |
|:---------------|----------------:|
| Millie Parfait |        0.379463 |
| Selen Tatsuki  |        0.308303 |
| Elira Pendora  |        0.238874 |
| Shu Yamino     |        0.204688 |
| Kyo Kaneko     |        0.193015 |

|                |   Reimu Endou |
|:---------------|--------------:|
| Elira Pendora  |      0.260313 |
| Selen Tatsuki  |      0.259119 |
| Nina Kosaka    |      0.195992 |
| Ike Eveland    |      0.192302 |
| Millie Parfait |      0.191119 |

|             |   Luca Kaneshiro |
|:------------|-----------------:|
| Shu Yamino  |         0.31125  |
| Mysta Rias  |         0.257896 |
| Ren Zotto   |         0.235123 |
| Kyo Kaneko  |         0.185022 |
| Uki Violeta |         0.164422 |

|                |   Shu Yamino |
|:---------------|-------------:|
| Luca Kaneshiro |     0.31125  |
| Elira Pendora  |     0.206158 |
| Enna Alouette  |     0.204688 |
| Uki Violeta    |     0.198608 |
| Sonny Brisko   |     0.151498 |

|                |   Ike Eveland |
|:---------------|--------------:|
| Reimu Endou    |      0.192302 |
| Selen Tatsuki  |      0.156472 |
| Ren Zotto      |      0.13932  |
| Vox Akuma      |      0.137317 |
| Ver Vermillion |      0.134295 |

|                |   Mysta Rias |
|:---------------|-------------:|
| Luca Kaneshiro |     0.257896 |
| Kyo Kaneko     |     0.170309 |
| Enna Alouette  |     0.169872 |
| Millie Parfait |     0.167326 |
| Uki Violeta    |     0.152314 |

|             |   Vox Akuma |
|:------------|------------:|
| Hex Haywire |    0.178425 |
| Mysta Rias  |    0.140605 |
| Fulgur Ovid |    0.138057 |
| Ren Zotto   |    0.137398 |
| Ike Eveland |    0.137317 |

|                |   Sonny Brisko |
|:---------------|---------------:|
| Ren Zotto      |       0.292608 |
| Alban Knox     |       0.250053 |
| Selen Tatsuki  |       0.176449 |
| Petra Gurin    |       0.163171 |
| Millie Parfait |       0.16161  |

|                   |   Uki Violeta |
|:------------------|--------------:|
| Fulgur Ovid       |      0.266796 |
| Aster Arcadia     |      0.201013 |
| Ren Zotto         |      0.200585 |
| Shu Yamino        |      0.198608 |
| Doppio Dropscythe |      0.174793 |

|                  |   Alban Knox |
|:-----------------|-------------:|
| Sonny Brisko     |     0.250053 |
| Maria Marionette |     0.232911 |
| Ren Zotto        |     0.177123 |
| Ver Vermillion   |     0.164817 |
| Nina Kosaka      |     0.134711 |

|                   |   Fulgur Ovid |
|:------------------|--------------:|
| Uki Violeta       |      0.266796 |
| Millie Parfait    |      0.195416 |
| Enna Alouette     |      0.168476 |
| Vox Akuma         |      0.138057 |
| Doppio Dropscythe |      0.127184 |

|                 |   Maria Marionette |
|:----------------|-------------------:|
| Alban Knox      |           0.232911 |
| Kotoka Torahime |           0.197759 |
| Petra Gurin     |           0.189534 |
| Scarle Yonaguni |           0.189534 |
| Reimu Endou     |           0.170937 |

|                 |   Kyo Kaneko |
|:----------------|-------------:|
| Enna Alouette   |     0.193015 |
| Ren Zotto       |     0.186139 |
| Luca Kaneshiro  |     0.185022 |
| Scarle Yonaguni |     0.17428  |
| Mysta Rias      |     0.170309 |

|                  |   Aia Amare |
|:-----------------|------------:|
| Kotoka Torahime  |    0.190697 |
| Ver Vermillion   |    0.157265 |
| Meloco Kyoran    |    0.155284 |
| Maria Marionette |    0.150801 |
| Reimu Endou      |    0.140906 |

|                |   Aster Arcadia |
|:---------------|----------------:|
| Uki Violeta    |        0.201013 |
| Ver Vermillion |        0.20054  |
| Hex Haywire    |        0.177035 |
| Ren Zotto      |        0.159646 |
| Kyo Kaneko     |        0.147497 |

|                  |   Scarle Yonaguni |
|:-----------------|------------------:|
| Maria Marionette |          0.189534 |
| Meloco Kyoran    |          0.176453 |
| Kyo Kaneko       |          0.17428  |
| Luca Kaneshiro   |          0.159542 |
| Nina Kosaka      |          0.148532 |

|                |   Ren Zotto |
|:---------------|------------:|
| Sonny Brisko   |    0.292608 |
| Luca Kaneshiro |    0.235123 |
| Hex Haywire    |    0.219223 |
| Uki Violeta    |    0.200585 |
| Kyo Kaneko     |    0.186139 |

|                |   Doppio Dropscythe |
|:---------------|--------------------:|
| Ver Vermillion |            0.186381 |
| Uki Violeta    |            0.174793 |
| Ren Zotto      |            0.167501 |
| Enna Alouette  |            0.165239 |
| Luca Kaneshiro |            0.163025 |

|                 |   Meloco Kyoran |
|:----------------|----------------:|
| Kotoka Torahime |        0.267481 |
| Rosemi Lovelock |        0.195268 |
| Ver Vermillion  |        0.194903 |
| Reimu Endou     |        0.183279 |
| Scarle Yonaguni |        0.176453 |

|                |   Hex Haywire |
|:---------------|--------------:|
| Ver Vermillion |      0.226088 |
| Ren Zotto      |      0.219223 |
| Vox Akuma      |      0.178425 |
| Aster Arcadia  |      0.177035 |
| Meloco Kyoran  |      0.174471 |

|                  |   Kotoka Torahime |
|:-----------------|------------------:|
| Meloco Kyoran    |          0.267481 |
| Rosemi Lovelock  |          0.219393 |
| Maria Marionette |          0.197759 |
| Aia Amare        |          0.190697 |
| Reimu Endou      |          0.187307 |

|                   |   Ver Vermillion |
|:------------------|-----------------:|
| Hex Haywire       |         0.226088 |
| Aster Arcadia     |         0.20054  |
| Meloco Kyoran     |         0.194903 |
| Doppio Dropscythe |         0.186381 |
| Ren Zotto         |         0.168838 |

</details>
<br>

## Negative correlations

Interestingly there are some negative values, which means it's very unlikely that someone oshis these two liver at the same time. 

Let's see which livers are least likely to be oshied by the same viewer.

|                   |   lowest correlation | with...       |
|:------------------|---------------------:|:--------------|
| Vox Akuma         |          -0.0762323  | Elira Pendora |
| Doppio Dropscythe |          -0.0680192  | Ike Eveland   |
| Petra Gurin       |          -0.0462684  | Vox Akuma     |
| Sonny Brisko      |          -0.0458456  | Vox Akuma     |
| Hex Haywire       |          -0.0437866  | Elira Pendora |
| Shu Yamino        |          -0.0424765  | Vox Akuma     |
| Fulgur Ovid       |          -0.0358482  | Petra Gurin   |
| Rosemi Lovelock   |          -0.0313     | Sonny Brisko  |
| Enna Alouette     |          -0.0293467  | Vox Akuma     |
| Kyo Kaneko        |          -0.0281429  | Pomu Rainpuff |
| Millie Parfait    |          -0.0269282  | Vox Akuma     |
| Meloco Kyoran     |          -0.0221673  | Fulgur Ovid   |
| Kotoka Torahime   |          -0.0210835  | Kyo Kaneko    |
| Maria Marionette  |          -0.0208357  | Hex Haywire   |
| Ver Vermillion    |          -0.0201546  | Fulgur Ovid   |
| Aster Arcadia     |          -0.0184278  | Fulgur Ovid   |
| Mysta Rias        |          -0.0140925  | Meloco Kyoran |
| Aia Amare         |          -0.0136513  | Petra Gurin   |
| Scarle Yonaguni   |          -0.0129163  | Pomu Rainpuff |
| Selen Tatsuki     |          -0.0021237  | Aster Arcadia |
| Alban Knox        |           0.00082701 | Vox Akuma     |
| Uki Violeta       |           0.00550615 | Petra Gurin   |
| Luca Kaneshiro    |           0.00641654 | Petra Gurin   |
| Reimu Endou       |           0.0142914  | Aster Arcadia |
| Finana Ryugu      |           0.0238083  | Vox Akuma     |
| Nina Kosaka       |           0.029254   | Sonny Brisko  |
| Ren Zotto         |           0.0405703  | Aia Amare     |

The **top 5 lowest correlations** are between:

1. Vox Akuma & Elira Pandora
2. Doppio Dropscyte & Ike Eveland
3. Petra Gurin & Vox Akuma
4. Sonny Brisko & Vox Akuma
5. Hex Haywire & Elira Pandora

Some livers like Vox show up on the list multiple times as the least correlated. If we look at the distribution of the responses, Vox got the second most responses, which means correlation (both positive and negative) would become stronger. 

Of all 206 respondents who oshi Vox, only 17 people have him as their only oshi. From analysing this dataset only, it is hard to conclude why Vox shows up on the list of lowest correlations so many times. 


# Ethics and Privacy

The data was collected anonymously by me (Gloria Kao). Google login was required to make sure that a person does not fill out the form multiple times, which would increase correlation between certain livers in the dataset, but no personal information such as name or email was collected. The survey was posted on Twitter and Discord without any extra incentive or advertisement, so data is collected from people by their voluntary will and with their consent. There is minimal ethics concern.


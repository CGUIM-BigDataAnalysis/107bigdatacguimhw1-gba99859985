107-2 大數據分析方法 作業一
================
put your name here

搞不清楚各行各業的薪資差異嗎? 念研究所到底對第一份工作的薪資影響有多大? CP值高嗎? 透過分析**初任人員平均經常性薪資**-
[開放資料連結](https://data.gov.tw/dataset/6647)，可初步了解台灣近幾年各行各業、各學歷的起薪。

## 比較103年度和106年度大學畢業者的薪資資料

### 資料匯入與處理

``` r
library(jsonlite)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(readr)
pay103<-read_csv("C:\\Users\\dennis316\\Desktop\\103pay.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   年度 = col_double(),
    ##   大職業別 = col_character(),
    ##   `經常性薪資-薪資` = col_double(),
    ##   `經常性薪資-女/男` = col_double(),
    ##   `國中及以下-薪資` = col_character(),
    ##   `國中及以下-女/男` = col_character(),
    ##   `高中或高職-薪資` = col_character(),
    ##   `高中或高職-女/男` = col_character(),
    ##   `專科-薪資` = col_character(),
    ##   `專科-女/男` = col_character(),
    ##   `大學-薪資` = col_character(),
    ##   `大學-女/男` = col_character(),
    ##   `研究所及以上-薪資` = col_character(),
    ##   `研究所及以上-女/男` = col_character()
    ## )

``` r
pay104<-read_csv("C:\\Users\\dennis316\\Desktop\\104pay.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   年度 = col_double(),
    ##   大職業別 = col_character(),
    ##   `經常性薪資-薪資` = col_double(),
    ##   `經常性薪資-女/男` = col_character(),
    ##   `國中及以下-薪資` = col_character(),
    ##   `國中及以下-女/男` = col_character(),
    ##   `高中或高職-薪資` = col_character(),
    ##   `高中或高職-女/男` = col_character(),
    ##   `專科-薪資` = col_character(),
    ##   `專科-女/男` = col_character(),
    ##   `大學-薪資` = col_character(),
    ##   `大學-女/男` = col_character(),
    ##   `研究所及以上-薪資` = col_character(),
    ##   `研究所及以上-女/男` = col_character()
    ## )

``` r
pay105<-read_csv("C:\\Users\\dennis316\\Desktop\\105pay.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   年度 = col_double(),
    ##   大職業別 = col_character(),
    ##   `經常性薪資-薪資` = col_double(),
    ##   `經常性薪資-女/男` = col_character(),
    ##   `國中及以下-薪資` = col_character(),
    ##   `國中及以下-女/男` = col_character(),
    ##   `高中或高職-薪資` = col_character(),
    ##   `高中或高職-女/男` = col_character(),
    ##   `專科-薪資` = col_character(),
    ##   `專科-女/男` = col_character(),
    ##   `大學-薪資` = col_character(),
    ##   `大學-女/男` = col_character(),
    ##   `研究所及以上-薪資` = col_character(),
    ##   `研究所及以上-女/男` = col_character()
    ## )

``` r
pay106<-read_csv("C:\\Users\\dennis316\\Desktop\\106pay.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   年度 = col_double(),
    ##   大職業別 = col_character(),
    ##   `經常性薪資-薪資` = col_double(),
    ##   `經常性薪資-女/男` = col_character(),
    ##   `國中及以下-薪資` = col_character(),
    ##   `國中及以下-女/男` = col_character(),
    ##   `高中或高職-薪資` = col_character(),
    ##   `高中或高職-女/男` = col_character(),
    ##   `專科-薪資` = col_character(),
    ##   `專科-女/男` = col_character(),
    ##   `大學-薪資` = col_character(),
    ##   `大學-女/男` = col_character(),
    ##   `研究所及以上-薪資` = col_character(),
    ##   `研究所及以上-女/男` = col_character()
    ## )

``` r
#fromjson()
#inner_join()
```

### 106年度薪資較103年度薪資高的職業有哪些?

“運輸及倉儲業-基層技術工及勞力工”是提升比例最高的，薪水加了大概3000元左右,但“專業、科學及技術服務業-專業人員”
卻加了3500元左右，所以提升比例最高的，並不一定是最賺的。

``` r
hw1<-data.frame(
  title=pay103$大職業別,
  pay_106=pay106$`經常性薪資-薪資`,
  pay_103=pay103$`經常性薪資-薪資`,
  rise_up=pay106$`經常性薪資-薪資`/pay103$`經常性薪資-薪資`
  )
hw1<-hw1[order(hw1$rise_up,decreasing=T),]
knitr::kable(head(hw1,10))
```

|     | title                      | pay\_106 | pay\_103 | rise\_up |
| --- | :------------------------- | -------: | -------: | -------: |
| 70  | 運輸及倉儲業-基層技術工及勞力工           |    24585 |    21676 | 1.134204 |
| 75  | 住宿及餐飲業-服務及銷售工作人員           |    24468 |    21647 | 1.130318 |
| 77  | 住宿及餐飲業-基層技術工及勞力工           |    23032 |    20671 | 1.114218 |
| 100 | 專業、科學及技術服務業-專業人員           |    35538 |    32078 | 1.107862 |
| 132 | 藝術、娛樂及休閒服務業-技藝、機械設備操作及組裝人員 |    26269 |    23723 | 1.107322 |
| 35  | 電力及燃氣供應業-基層技術工及勞力工         |    24694 |    22368 | 1.103988 |
| 38  | 用水供應及污染整治業-技術員及助理專業人員      |    31505 |    28552 | 1.103425 |
| 69  | 運輸及倉儲業-技藝、機械設備操作及組裝人員      |    29251 |    26520 | 1.102979 |
| 136 | 其他服務業-技術員及助理專業人員           |    27270 |    24724 | 1.102977 |
| 140 | 其他服務業-基層技術工及勞力工            |    22708 |    20609 | 1.101849 |

### 提高超過5%的的職業有哪些?

``` r
hw1_2<-subset(hw1,rise_up>1.05)
knitr::kable(hw1_2)
```

|     | title                      | pay\_106 | pay\_103 | rise\_up |
| --- | :------------------------- | -------: | -------: | -------: |
| 70  | 運輸及倉儲業-基層技術工及勞力工           |    24585 |    21676 | 1.134204 |
| 75  | 住宿及餐飲業-服務及銷售工作人員           |    24468 |    21647 | 1.130318 |
| 77  | 住宿及餐飲業-基層技術工及勞力工           |    23032 |    20671 | 1.114218 |
| 100 | 專業、科學及技術服務業-專業人員           |    35538 |    32078 | 1.107862 |
| 132 | 藝術、娛樂及休閒服務業-技藝、機械設備操作及組裝人員 |    26269 |    23723 | 1.107322 |
| 35  | 電力及燃氣供應業-基層技術工及勞力工         |    24694 |    22368 | 1.103988 |
| 38  | 用水供應及污染整治業-技術員及助理專業人員      |    31505 |    28552 | 1.103425 |
| 69  | 運輸及倉儲業-技藝、機械設備操作及組裝人員      |    29251 |    26520 | 1.102979 |
| 136 | 其他服務業-技術員及助理專業人員           |    27270 |    24724 | 1.102977 |
| 140 | 其他服務業-基層技術工及勞力工            |    22708 |    20609 | 1.101849 |
| 133 | 藝術、娛樂及休閒服務業-基層技術工及勞力工      |    22621 |    20565 | 1.099976 |
| 40  | 用水供應及污染整治業-服務及銷售工作人員       |    29637 |    26957 | 1.099418 |
| 115 | 教育服務業-技術員及助理專業人員           |    27778 |    25278 | 1.098900 |
| 79  | 資訊及通訊傳播業-專業人員              |    33646 |    30686 | 1.096461 |
| 67  | 運輸及倉儲業-事務支援人員              |    26834 |    24513 | 1.094685 |
| 104 | 專業、科學及技術服務業-技藝、機械設備操作及組裝人員 |    26819 |    24522 | 1.093671 |
| 135 | 其他服務業-專業人員                 |    32250 |    29500 | 1.093220 |
| 66  | 運輸及倉儲業-技術員及助理專業人員          |    31272 |    28639 | 1.091938 |
| 78  | 資訊及通訊傳播業                   |    28918 |    26498 | 1.091328 |
| 139 | 其他服務業-技藝、機械設備操作及組裝人員       |    24938 |    22861 | 1.090853 |
| 116 | 教育服務業-事務支援人員               |    24326 |    22325 | 1.089630 |
| 71  | 住宿及餐飲業                     |    25068 |    23013 | 1.089297 |
| 47  | 營造業-服務及銷售工作人員              |    27308 |    25075 | 1.089053 |
| 64  | 運輸及倉儲業                     |    28328 |    26058 | 1.087113 |
| 117 | 教育服務業-服務及銷售工作人員            |    24803 |    22817 | 1.087040 |
| 63  | 批發及零售業-基層技術工及勞力工           |    23463 |    21585 | 1.087005 |
| 73  | 住宿及餐飲業-技術員及助理專業人員          |    27807 |    25590 | 1.086635 |
| 56  | 服務業部門-基層技術工及勞力工            |    22892 |    21082 | 1.085855 |
| 105 | 專業、科學及技術服務業-基層技術工及勞力工      |    22940 |    21145 | 1.084890 |
| 113 | 教育服務業                      |    25383 |    23437 | 1.083031 |
| 130 | 藝術、娛樂及休閒服務業-事務支援人員         |    24602 |    22766 | 1.080647 |
| 99  | 專業、科學及技術服務業                |    29345 |    27202 | 1.078781 |
| 83  | 資訊及通訊傳播業-技藝、機械設備操作及組裝人員    |    26330 |    24425 | 1.077994 |
| 7   | 工業及服務業部門-基層技術工及勞力工         |    22824 |    21195 | 1.076858 |
| 28  | 製造業-基層技術工及勞力工              |    22655 |    21046 | 1.076452 |
| 110 | 支援服務業-服務及銷售工作人員            |    24883 |    23134 | 1.075603 |
| 33  | 電力及燃氣供應業-服務及銷售工作人員         |    26803 |    24950 | 1.074269 |
| 42  | 用水供應及污染整治業-基層技術工及勞力工       |    23157 |    21557 | 1.074222 |
| 91  | 金融及保險業-基層技術工及勞力工           |    24377 |    22712 | 1.073309 |
| 114 | 教育服務業-專業人員                 |    28189 |    26277 | 1.072763 |
| 36  | 用水供應及污染整治業                 |    27459 |    25601 | 1.072575 |
| 14  | 工業部門-基層技術工及勞力工             |    22784 |    21256 | 1.071886 |
| 127 | 藝術、娛樂及休閒服務業                |    25136 |    23475 | 1.070756 |
| 41  | 用水供應及污染整治業-技藝、機械設備操作及組裝人員  |    27049 |    25263 | 1.070696 |
| 81  | 資訊及通訊傳播業-事務支援人員            |    26723 |    24959 | 1.070676 |
| 119 | 教育服務業-基層技術工及勞力工            |    22148 |    20702 | 1.069848 |
| 82  | 資訊及通訊傳播業-服務及銷售工作人員         |    25734 |    24062 | 1.069487 |
| 108 | 支援服務業-技術員及助理專業人員           |    28300 |    26485 | 1.068529 |
| 39  | 用水供應及污染整治業-事務支援人員          |    26058 |    24394 | 1.068213 |
| 112 | 支援服務業-基層技術工及勞力工            |    22013 |    20620 | 1.067556 |
| 93  | 不動產業-專業人員                  |    34237 |    32126 | 1.065710 |
| 54  | 服務業部門-服務及銷售工作人員            |    24608 |    23110 | 1.064820 |
| 118 | 教育服務業-技藝、機械設備操作及組裝人員       |    24005 |    22545 | 1.064759 |
| 137 | 其他服務業-事務支援人員               |    24338 |    22867 | 1.064329 |
| 53  | 服務業部門-事務支援人員               |    26297 |    24716 | 1.063967 |
| 101 | 專業、科學及技術服務業-技術員及助理專業人員     |    29309 |    27558 | 1.063539 |
| 50  | 服務業部門                      |    27363 |    25736 | 1.063219 |
| 131 | 藝術、娛樂及休閒服務業-服務及銷售工作人員      |    24062 |    22632 | 1.063185 |
| 80  | 資訊及通訊傳播業-技術員及助理專業人員        |    29176 |    27461 | 1.062452 |
| 55  | 服務業部門-技藝、機械設備操作及組裝人員       |    26895 |    25343 | 1.061240 |
| 102 | 專業、科學及技術服務業-事務支援人員         |    26712 |    25174 | 1.061095 |
| 68  | 運輸及倉儲業-服務及銷售工作人員           |    26605 |    25089 | 1.060425 |
| 44  | 營造業-專業人員                   |    33780 |    31868 | 1.059997 |
| 122 | 醫療保健服務業-技術員及助理專業人員         |    30339 |    28643 | 1.059212 |
| 52  | 服務業部門-技術員及助理專業人員           |    29608 |    27997 | 1.057542 |
| 134 | 其他服務業                      |    23848 |    22551 | 1.057514 |
| 5   | 工業及服務業部門-服務及銷售工作人員         |    25012 |    23654 | 1.057411 |
| 60  | 批發及零售業-事務支援人員              |    25776 |    24382 | 1.057173 |
| 88  | 金融及保險業-事務支援人員              |    30499 |    28854 | 1.057011 |
| 74  | 住宿及餐飲業-事務支援人員              |    25226 |    23868 | 1.056896 |
| 106 | 支援服務業                      |    25199 |    23847 | 1.056695 |
| 109 | 支援服務業-事務支援人員               |    25107 |    23781 | 1.055759 |
| 61  | 批發及零售業-服務及銷售工作人員           |    24146 |    22876 | 1.055517 |
| 1   | 工業及服務業部門                   |    27055 |    25634 | 1.055434 |
| 57  | 批發及零售業                     |    26457 |    25069 | 1.055367 |
| 37  | 用水供應及污染整治業-專業人員            |    34692 |    32899 | 1.054500 |
| 4   | 工業及服務業部門-事務支援人員            |    26068 |    24722 | 1.054445 |
| 76  | 住宿及餐飲業-技藝、機械設備操作及組裝人員      |    25344 |    24046 | 1.053980 |
| 51  | 服務業部門-專業人員                 |    34353 |    32594 | 1.053967 |
| 103 | 專業、科學及技術服務業-服務及銷售工作人員      |    25697 |    24420 | 1.052293 |
| 12  | 工業部門-服務及銷售工作人員             |    25811 |    24529 | 1.052265 |
| 49  | 營造業-基層技術工及勞力工              |    24328 |    23127 | 1.051931 |
| 6   | 工業及服務業部門-技藝、機械設備操作及組裝人員    |    25338 |    24089 | 1.051849 |
| 58  | 批發及零售業-專業人員                |    33609 |    31965 | 1.051431 |
| 46  | 營造業-事務支援人員                 |    25195 |    23970 | 1.051106 |
| 22  | 製造業                        |    26782 |    25481 | 1.051058 |
| 27  | 製造業-技藝、機械設備操作及組裝人員         |    24695 |    23501 | 1.050806 |
| 8   | 工業部門                       |    26860 |    25571 | 1.050409 |

### 主要的職業種別是哪些種類呢?

``` r
hw1_2$title<-as.character(hw1_2$title)
k<-c(strsplit(hw1_2$title,"-"))
mainJob<-c()
for(n in 1:88){
  mainJob<-c(mainJob,k[[n]][1])
}
table(mainJob)
```

    ## mainJob
    ##       工業及服務業部門               工業部門               不動產業 
    ##                      5                      3                      1 
    ##             支援服務業   用水供應及污染整治業           住宿及餐飲業 
    ##                      5                      7                      6 
    ##           批發及零售業             其他服務業             服務業部門 
    ##                      5                      6                      7 
    ##           金融及保險業 專業、科學及技術服務業             教育服務業 
    ##                      2                      7                      7 
    ##       資訊及通訊傳播業           運輸及倉儲業       電力及燃氣供應業 
    ##                      6                      6                      2 
    ##                 製造業                 營造業         醫療保健服務業 
    ##                      3                      4                      1 
    ## 藝術、娛樂及休閒服務業 
    ##                      5

## 男女同工不同酬現況分析

男女同工不同酬一直是性別平等中很重要的問題，分析資料來源為103到106年度的大學畢業薪資。
將103\~106年的男女比結合在一起，排序出前10筆資料
在分析中“技藝、機械設備操作及組裝人員”在前10筆資料就佔了6筆

而女生薪資比男生多的前10筆資料，“服務及銷售工作人員”，也佔了5筆，可以看出男女在各職業場的優勢

但是560筆資料中，女生薪資超過男生薪資的資料卻只佔了不超過30筆，可見其不公平程度

### 103到106年度的大學畢業薪資資料，哪些行業男生薪資比女生薪資多?

``` r
library(readr)
k103<-paste(c(pay103$大職業別),"103")
k104<-paste(c(pay103$大職業別),"104")
k105<-paste(c(pay103$大職業別),"105")
k106<-paste(c(pay103$大職業別),"106")
gender103<-pay103$`大學-女/男`
gender104<-pay104$`大學-女/男`
gender105<-pay105$`大學-女/男`
gender106<-pay106$`大學-女/男`
k_all<-c(k103,k104,k105,k106)
genderPay_all<-c(gender103,gender104,gender105,gender106)
genderPay_all<-iconv(genderPay_all,"WINDOWS-1252","UTF-8")
genderPay_all<-as.numeric(genderPay_all)
```

    ## Warning: 強制變更過程中產生了 NA

``` r
genderPay<-data.frame(
  title=k_all,
  genderPay=genderPay_all
)

genderPay<-genderPay[order(genderPay$genderPay,decreasing = F),]
knitr::kable(head(genderPay,10))
```

|     | title                       | genderPay |
| --- | :-------------------------- | --------: |
| 20  | 礦業及土石採取業-技藝、機械設備操作及組裝人員 103 |     84.97 |
| 118 | 教育服務業-技藝、機械設備操作及組裝人員 103    |     88.49 |
| 136 | 其他服務業-技術員及助理專業人員 103        |     89.36 |
| 377 | 不動產業-技藝、機械設備操作及組裝人員 105     |     91.38 |
| 174 | 電力及燃氣供應業-技藝、機械設備操作及組裝人員 104 |     91.69 |
| 34  | 電力及燃氣供應業-技藝、機械設備操作及組裝人員 103 |     91.77 |
| 257 | 教育服務業-服務及銷售工作人員 104         |     91.90 |
| 157 | 礦業及土石採取業-技術員及助理專業人員 104     |     92.42 |
| 19  | 礦業及土石採取業-服務及銷售工作人員 103      |     92.57 |
| 160 | 礦業及土石採取業-技藝、機械設備操作及組裝人員 104 |     93.10 |

### 哪些行業女生薪資比男生薪資多?

``` r
genderPay<-genderPay[order(genderPay$genderPay,decreasing = T),]
knitr::kable(head(genderPay,10))
```

|     | title                          | genderPay |
| --- | :----------------------------- | --------: |
| 502 | 資訊及通訊傳播業-服務及銷售工作人員 106         |    100.33 |
| 244 | 專業、科學及技術服務業-技藝、機械設備操作及組裝人員 104 |    100.26 |
| 366 | 金融及保險業-專業人員 105                |    100.11 |
| 17  | 礦業及土石採取業-技術員及助理專業人員 103        |    100.00 |
| 40  | 用水供應及污染整治業-服務及銷售工作人員 103       |    100.00 |
| 47  | 營造業-服務及銷售工作人員 103              |    100.00 |
| 180 | 用水供應及污染整治業-服務及銷售工作人員 104       |    100.00 |
| 237 | 不動產業-技藝、機械設備操作及組裝人員 104        |    100.00 |
| 264 | 醫療保健服務業-服務及銷售工作人員 104          |    100.00 |
| 275 | 其他服務業-專業人員 104                 |    100.00 |

## 研究所薪資差異

以106年度的資料來看，哪個職業別念研究所最划算呢 (研究所學歷薪資與大學學歷薪資增加比例最多)?

首先將pay106中大學-薪資和研究所及以上-薪資的空字串運用gsub()轉換成1，

再用as.numeric()轉換為數字

再用round將小數點簡化為5位數

最後用order()排序

從比例來看“礦業及土石採取業-事務支援人員”念研究所最划算，但念研究所也是要花錢以及時間的，連成長金額也考慮或許才值得。

``` r
colPay<-gsub("—",1,pay106$`大學-薪資`)
mastPay<-gsub("—",1,pay106$`研究所及以上-薪資`)
colPay<-as.numeric(colPay)
mastPay<-as.numeric(mastPay)
riseup<-round(mastPay/colPay,digits = 5)
studyGet<-data.frame(
  title=pay106$大職業別,
  colPay=colPay,
  mastPay=mastPay,
  rise_up=riseup
)
studyGet<-studyGet[order(studyGet$rise_up,decreasing = T),]
knitr::kable(head(studyGet,10))
```

|     | title               | colPay | mastPay | rise\_up |
| --- | :------------------ | -----: | ------: | -------: |
| 18  | 礦業及土石採取業-事務支援人員     |  24815 |   30000 |  1.20895 |
| 99  | 專業\_科學及技術服務業        |  29648 |   35666 |  1.20298 |
| 136 | 其他服務業-技術員及助理專業人員    |  27929 |   33500 |  1.19947 |
| 102 | 專業\_科學及技術服務業-事務支援人員 |  27035 |   32234 |  1.19231 |
| 57  | 批發及零售業              |  27611 |   32910 |  1.19192 |
| 22  | 製造業                 |  28155 |   33458 |  1.18835 |
| 130 | 藝術\_娛樂及休閒服務業-事務支援人員 |  24970 |   29657 |  1.18771 |
| 8   | 工業部門                |  28263 |   33448 |  1.18346 |
| 1   | 工業及服務業部門            |  28446 |   33633 |  1.18235 |
| 50  | 服務業部門               |  28715 |   33922 |  1.18133 |

## 我有興趣的職業別薪資狀況分析

### 有興趣的職業別篩選，呈現薪資

``` r
loveWork<-data.frame(
  title=pay106$大職業別,
  col_pay=pay106$`大學-薪資`,
  mast_pay=pay106$`研究所及以上-薪資`
        )
loveWork<-loveWork[grep("資訊及通訊傳播業",loveWork$title),]
knitr::kable(loveWork)
```

|    | title                    | col\_pay | mast\_pay |
| -- | :----------------------- | :------- | :-------- |
| 78 | 資訊及通訊傳播業                 | 29198    | 33944     |
| 79 | 資訊及通訊傳播業-專業人員            | 31817    | 36545     |
| 80 | 資訊及通訊傳播業-技術員及助理專業人員      | 28902    | 32354     |
| 81 | 資訊及通訊傳播業-事務支援人員          | 27156    | 30856     |
| 82 | 資訊及通訊傳播業-服務及銷售工作人員       | 27296    | —         |
| 83 | 資訊及通訊傳播業-技藝\_機械設備操作及組裝人員 | 27200    | —         |
| 84 | 資訊及通訊傳播業-基層技術工及勞力工       | —        | —         |

### 這些職業別研究所薪資與大學薪資差多少呢？

直接使用as.numeric會產生錯誤

因此用iconv()轉換編碼，雖然會warning但能夠執行

我有興趣的職業裡，讀研究所後提升的薪資約為3000\~5000左右，但我認為擁有兩年的實際經驗或許可收穫更多，因此不會改變我不讀研究所的心意。

``` r
loveWork$mast_pay<-iconv(loveWork$mast_pay,"WINDOWS-1252","UTF-8")
loveWork$col_pay<-iconv(loveWork$col_pay,"WINDOWS-1252","UTF-8")
loveWork$mast_pay<-as.numeric(loveWork$mast_pay)
```

    ## Warning: 強制變更過程中產生了 NA

``` r
loveWork$col_pay<-as.numeric(loveWork$col_pay)
```

    ## Warning: 強制變更過程中產生了 NA

``` r
c<-c(loveWork$mast_pay-loveWork$col_pay)
c<-c(loveWork$mast_pay-loveWork$col_pay)
XG<-data.frame(
  title=loveWork$title,
  XG=c
)
knitr::kable(XG) 
```

| title                    |   XG |
| :----------------------- | ---: |
| 資訊及通訊傳播業                 | 4746 |
| 資訊及通訊傳播業-專業人員            | 4728 |
| 資訊及通訊傳播業-技術員及助理專業人員      | 3452 |
| 資訊及通訊傳播業-事務支援人員          | 3700 |
| 資訊及通訊傳播業-服務及銷售工作人員       |   NA |
| 資訊及通訊傳播業-技藝\_機械設備操作及組裝人員 |   NA |
| 資訊及通訊傳播業-基層技術工及勞力工       |   NA |

problemset3.md
==============
# Problem Set Number 3: Legal Analytics 

#### Matt Hochstein

##### Example 1 available at http://rforwork.info/2012/12/23/binary-classification-a-comparison-of-titanic-proportions-between-logistic-regression-random-forests-and-conditional-trees/

========================================================================

## Problem 1

### Load Packages

```r
library(randomForest)
```

```
## randomForest 4.6-7
## Type rfNews() to see new features/changes/bug fixes.
```

```r
library(party)
```

```
## Loading required package: grid
## Loading required package: zoo
## 
## Attaching package: 'zoo'
## 
## The following objects are masked from 'package:base':
## 
##     as.Date, as.Date.numeric
## 
## Loading required package: sandwich
## Loading required package: strucchange
## Loading required package: modeltools
## Loading required package: stats4
```


### Load  Data

```r
load(url("http://biostat.mc.vanderbilt.edu/wiki/pub/Main/DataSets/titanic3.sav"))
dim(titanic3)
```

```
## [1] 1309   14
```

```r
attributes(titanic3)
```

```
## $row.names
##    [1] "1"    "2"    "3"    "4"    "5"    "6"    "7"    "8"    "9"   
##   [10] "10"   "11"   "12"   "13"   "14"   "15"   "16"   "17"   "18"  
##   [19] "19"   "20"   "21"   "22"   "23"   "24"   "25"   "26"   "27"  
##   [28] "28"   "29"   "30"   "31"   "32"   "33"   "34"   "35"   "36"  
##   [37] "37"   "38"   "39"   "40"   "41"   "42"   "43"   "44"   "45"  
##   [46] "46"   "47"   "48"   "49"   "50"   "51"   "52"   "53"   "54"  
##   [55] "55"   "56"   "57"   "58"   "59"   "60"   "61"   "62"   "63"  
##   [64] "64"   "65"   "66"   "67"   "68"   "69"   "70"   "71"   "72"  
##   [73] "73"   "74"   "75"   "76"   "77"   "78"   "79"   "80"   "81"  
##   [82] "82"   "83"   "84"   "85"   "86"   "87"   "88"   "89"   "90"  
##   [91] "91"   "92"   "93"   "94"   "95"   "96"   "97"   "98"   "99"  
##  [100] "100"  "101"  "102"  "103"  "104"  "105"  "106"  "107"  "108" 
##  [109] "109"  "110"  "111"  "112"  "113"  "114"  "115"  "116"  "117" 
##  [118] "118"  "119"  "120"  "121"  "122"  "123"  "124"  "125"  "126" 
##  [127] "127"  "128"  "129"  "130"  "131"  "132"  "133"  "134"  "135" 
##  [136] "136"  "137"  "138"  "139"  "140"  "141"  "142"  "143"  "144" 
##  [145] "145"  "146"  "147"  "148"  "149"  "150"  "151"  "152"  "153" 
##  [154] "154"  "155"  "156"  "157"  "158"  "159"  "160"  "161"  "162" 
##  [163] "163"  "164"  "165"  "166"  "167"  "168"  "169"  "170"  "171" 
##  [172] "172"  "173"  "174"  "175"  "176"  "177"  "178"  "179"  "180" 
##  [181] "181"  "182"  "183"  "184"  "185"  "186"  "187"  "188"  "189" 
##  [190] "190"  "191"  "192"  "193"  "194"  "195"  "196"  "197"  "198" 
##  [199] "199"  "200"  "201"  "202"  "203"  "204"  "205"  "206"  "207" 
##  [208] "208"  "209"  "210"  "211"  "212"  "213"  "214"  "215"  "216" 
##  [217] "217"  "218"  "219"  "220"  "221"  "222"  "223"  "224"  "225" 
##  [226] "226"  "227"  "228"  "229"  "230"  "231"  "232"  "233"  "234" 
##  [235] "235"  "236"  "237"  "238"  "239"  "240"  "241"  "242"  "243" 
##  [244] "244"  "245"  "246"  "247"  "248"  "249"  "250"  "251"  "252" 
##  [253] "253"  "254"  "255"  "256"  "257"  "258"  "259"  "260"  "261" 
##  [262] "262"  "263"  "264"  "265"  "266"  "267"  "268"  "269"  "270" 
##  [271] "271"  "272"  "273"  "274"  "275"  "276"  "277"  "278"  "279" 
##  [280] "280"  "281"  "282"  "283"  "284"  "285"  "286"  "287"  "288" 
##  [289] "289"  "290"  "291"  "292"  "293"  "294"  "295"  "296"  "297" 
##  [298] "298"  "299"  "300"  "301"  "302"  "303"  "304"  "305"  "306" 
##  [307] "307"  "308"  "309"  "310"  "311"  "312"  "313"  "314"  "315" 
##  [316] "316"  "317"  "318"  "319"  "320"  "321"  "322"  "323"  "324" 
##  [325] "325"  "326"  "327"  "328"  "329"  "330"  "331"  "332"  "333" 
##  [334] "334"  "335"  "336"  "337"  "338"  "339"  "340"  "341"  "342" 
##  [343] "343"  "344"  "345"  "346"  "347"  "348"  "349"  "350"  "351" 
##  [352] "352"  "353"  "354"  "355"  "356"  "357"  "358"  "359"  "360" 
##  [361] "361"  "362"  "363"  "364"  "365"  "366"  "367"  "368"  "369" 
##  [370] "370"  "371"  "372"  "373"  "374"  "375"  "376"  "377"  "378" 
##  [379] "379"  "380"  "381"  "382"  "383"  "384"  "385"  "386"  "387" 
##  [388] "388"  "389"  "390"  "391"  "392"  "393"  "394"  "395"  "396" 
##  [397] "397"  "398"  "399"  "400"  "401"  "402"  "403"  "404"  "405" 
##  [406] "406"  "407"  "408"  "409"  "410"  "411"  "412"  "413"  "414" 
##  [415] "415"  "416"  "417"  "418"  "419"  "420"  "421"  "422"  "423" 
##  [424] "424"  "425"  "426"  "427"  "428"  "429"  "430"  "431"  "432" 
##  [433] "433"  "434"  "435"  "436"  "437"  "438"  "439"  "440"  "441" 
##  [442] "442"  "443"  "444"  "445"  "446"  "447"  "448"  "449"  "450" 
##  [451] "451"  "452"  "453"  "454"  "455"  "456"  "457"  "458"  "459" 
##  [460] "460"  "461"  "462"  "463"  "464"  "465"  "466"  "467"  "468" 
##  [469] "469"  "470"  "471"  "472"  "473"  "474"  "475"  "476"  "477" 
##  [478] "478"  "479"  "480"  "481"  "482"  "483"  "484"  "485"  "486" 
##  [487] "487"  "488"  "489"  "490"  "491"  "492"  "493"  "494"  "495" 
##  [496] "496"  "497"  "498"  "499"  "500"  "501"  "502"  "503"  "504" 
##  [505] "505"  "506"  "507"  "508"  "509"  "510"  "511"  "512"  "513" 
##  [514] "514"  "515"  "516"  "517"  "518"  "519"  "520"  "521"  "522" 
##  [523] "523"  "524"  "525"  "526"  "527"  "528"  "529"  "530"  "531" 
##  [532] "532"  "533"  "534"  "535"  "536"  "537"  "538"  "539"  "540" 
##  [541] "541"  "542"  "543"  "544"  "545"  "546"  "547"  "548"  "549" 
##  [550] "550"  "551"  "552"  "553"  "554"  "555"  "556"  "557"  "558" 
##  [559] "559"  "560"  "561"  "562"  "563"  "564"  "565"  "566"  "567" 
##  [568] "568"  "569"  "570"  "571"  "572"  "573"  "574"  "575"  "576" 
##  [577] "577"  "578"  "579"  "580"  "581"  "582"  "583"  "584"  "585" 
##  [586] "586"  "587"  "588"  "589"  "590"  "591"  "592"  "593"  "594" 
##  [595] "595"  "596"  "597"  "598"  "599"  "600"  "601"  "602"  "603" 
##  [604] "604"  "605"  "606"  "607"  "608"  "609"  "610"  "611"  "612" 
##  [613] "613"  "614"  "615"  "616"  "617"  "618"  "619"  "620"  "621" 
##  [622] "622"  "623"  "624"  "625"  "626"  "627"  "628"  "629"  "630" 
##  [631] "631"  "632"  "633"  "634"  "635"  "636"  "637"  "638"  "639" 
##  [640] "640"  "641"  "642"  "643"  "644"  "645"  "646"  "647"  "648" 
##  [649] "649"  "650"  "651"  "652"  "653"  "654"  "655"  "656"  "657" 
##  [658] "658"  "659"  "660"  "661"  "662"  "663"  "664"  "665"  "666" 
##  [667] "667"  "668"  "669"  "670"  "671"  "672"  "673"  "674"  "675" 
##  [676] "676"  "677"  "678"  "679"  "680"  "681"  "682"  "683"  "684" 
##  [685] "685"  "686"  "687"  "688"  "689"  "690"  "691"  "692"  "693" 
##  [694] "694"  "695"  "696"  "697"  "698"  "699"  "700"  "701"  "702" 
##  [703] "703"  "704"  "705"  "706"  "707"  "708"  "709"  "710"  "711" 
##  [712] "712"  "713"  "714"  "715"  "716"  "717"  "718"  "719"  "720" 
##  [721] "721"  "722"  "723"  "724"  "725"  "726"  "727"  "728"  "729" 
##  [730] "730"  "731"  "732"  "733"  "734"  "735"  "736"  "737"  "738" 
##  [739] "739"  "740"  "741"  "742"  "743"  "744"  "745"  "746"  "747" 
##  [748] "748"  "749"  "750"  "751"  "752"  "753"  "754"  "755"  "756" 
##  [757] "757"  "758"  "759"  "760"  "761"  "762"  "763"  "764"  "765" 
##  [766] "766"  "767"  "768"  "769"  "770"  "771"  "772"  "773"  "774" 
##  [775] "775"  "776"  "777"  "778"  "779"  "780"  "781"  "782"  "783" 
##  [784] "784"  "785"  "786"  "787"  "788"  "789"  "790"  "791"  "792" 
##  [793] "793"  "794"  "795"  "796"  "797"  "798"  "799"  "800"  "801" 
##  [802] "802"  "803"  "804"  "805"  "806"  "807"  "808"  "809"  "810" 
##  [811] "811"  "812"  "813"  "814"  "815"  "816"  "817"  "818"  "819" 
##  [820] "820"  "821"  "822"  "823"  "824"  "825"  "826"  "827"  "828" 
##  [829] "829"  "830"  "831"  "832"  "833"  "834"  "835"  "836"  "837" 
##  [838] "838"  "839"  "840"  "841"  "842"  "843"  "844"  "845"  "846" 
##  [847] "847"  "848"  "849"  "850"  "851"  "852"  "853"  "854"  "855" 
##  [856] "856"  "857"  "858"  "859"  "860"  "861"  "862"  "863"  "864" 
##  [865] "865"  "866"  "867"  "868"  "869"  "870"  "871"  "872"  "873" 
##  [874] "874"  "875"  "876"  "877"  "878"  "879"  "880"  "881"  "882" 
##  [883] "883"  "884"  "885"  "886"  "887"  "888"  "889"  "890"  "891" 
##  [892] "892"  "893"  "894"  "895"  "896"  "897"  "898"  "899"  "900" 
##  [901] "901"  "902"  "903"  "904"  "905"  "906"  "907"  "908"  "909" 
##  [910] "910"  "911"  "912"  "913"  "914"  "915"  "916"  "917"  "918" 
##  [919] "919"  "920"  "921"  "922"  "923"  "924"  "925"  "926"  "927" 
##  [928] "928"  "929"  "930"  "931"  "932"  "933"  "934"  "935"  "936" 
##  [937] "937"  "938"  "939"  "940"  "941"  "942"  "943"  "944"  "945" 
##  [946] "946"  "947"  "948"  "949"  "950"  "951"  "952"  "953"  "954" 
##  [955] "955"  "956"  "957"  "958"  "959"  "960"  "961"  "962"  "963" 
##  [964] "964"  "965"  "966"  "967"  "968"  "969"  "970"  "971"  "972" 
##  [973] "973"  "974"  "975"  "976"  "977"  "978"  "979"  "980"  "981" 
##  [982] "982"  "983"  "984"  "985"  "986"  "987"  "988"  "989"  "990" 
##  [991] "991"  "992"  "993"  "994"  "995"  "996"  "997"  "998"  "999" 
## [1000] "1000" "1001" "1002" "1003" "1004" "1005" "1006" "1007" "1008"
## [1009] "1009" "1010" "1011" "1012" "1013" "1014" "1015" "1016" "1017"
## [1018] "1018" "1019" "1020" "1021" "1022" "1023" "1024" "1025" "1026"
## [1027] "1027" "1028" "1029" "1030" "1031" "1032" "1033" "1034" "1035"
## [1036] "1036" "1037" "1038" "1039" "1040" "1041" "1042" "1043" "1044"
## [1045] "1045" "1046" "1047" "1048" "1049" "1050" "1051" "1052" "1053"
## [1054] "1054" "1055" "1056" "1057" "1058" "1059" "1060" "1061" "1062"
## [1063] "1063" "1064" "1065" "1066" "1067" "1068" "1069" "1070" "1071"
## [1072] "1072" "1073" "1074" "1075" "1076" "1077" "1078" "1079" "1080"
## [1081] "1081" "1082" "1083" "1084" "1085" "1086" "1087" "1088" "1089"
## [1090] "1090" "1091" "1092" "1093" "1094" "1095" "1096" "1097" "1098"
## [1099] "1099" "1100" "1101" "1102" "1103" "1104" "1105" "1106" "1107"
## [1108] "1108" "1109" "1110" "1111" "1112" "1113" "1114" "1115" "1116"
## [1117] "1117" "1118" "1119" "1120" "1121" "1122" "1123" "1124" "1125"
## [1126] "1126" "1127" "1128" "1129" "1130" "1131" "1132" "1133" "1134"
## [1135] "1135" "1136" "1137" "1138" "1139" "1140" "1141" "1142" "1143"
## [1144] "1144" "1145" "1146" "1147" "1148" "1149" "1150" "1151" "1152"
## [1153] "1153" "1154" "1155" "1156" "1157" "1158" "1159" "1160" "1161"
## [1162] "1162" "1163" "1164" "1165" "1166" "1167" "1168" "1169" "1170"
## [1171] "1171" "1172" "1173" "1174" "1175" "1176" "1177" "1178" "1179"
## [1180] "1180" "1181" "1182" "1183" "1184" "1185" "1186" "1187" "1188"
## [1189] "1189" "1190" "1191" "1192" "1193" "1194" "1195" "1196" "1197"
## [1198] "1198" "1199" "1200" "1201" "1202" "1203" "1204" "1205" "1206"
## [1207] "1207" "1208" "1209" "1210" "1211" "1212" "1213" "1214" "1215"
## [1216] "1216" "1217" "1218" "1219" "1220" "1221" "1222" "1223" "1224"
## [1225] "1225" "1226" "1227" "1228" "1229" "1230" "1231" "1232" "1233"
## [1234] "1234" "1235" "1236" "1237" "1238" "1239" "1240" "1241" "1242"
## [1243] "1243" "1244" "1245" "1246" "1247" "1248" "1249" "1250" "1251"
## [1252] "1252" "1253" "1254" "1255" "1256" "1257" "1258" "1259" "1260"
## [1261] "1261" "1262" "1263" "1264" "1265" "1266" "1267" "1268" "1269"
## [1270] "1270" "1271" "1272" "1273" "1274" "1275" "1276" "1277" "1278"
## [1279] "1279" "1280" "1281" "1282" "1283" "1284" "1285" "1286" "1287"
## [1288] "1288" "1289" "1290" "1291" "1292" "1293" "1294" "1295" "1296"
## [1297] "1297" "1298" "1299" "1300" "1301" "1302" "1303" "1304" "1305"
## [1306] "1306" "1307" "1308" "1309"
## 
## $names
##  [1] "pclass"    "survived"  "name"      "sex"       "age"      
##  [6] "sibsp"     "parch"     "ticket"    "fare"      "cabin"    
## [11] "embarked"  "boat"      "body"      "home.dest"
## 
## $class
## [1] "data.frame"
```

```r
titanic3[1:5, ]
```

```
##   pclass survived                            name    sex     age sibsp
## 1    1st        1   Allen, Miss. Elisabeth Walton female 29.0000     0
## 2    1st        1  Allison, Master. Hudson Trevor   male  0.9167     1
## 3    1st        0    Allison, Miss. Helen Loraine female  2.0000     1
## 4    1st        0 Allison, Mr. Hudson Joshua Crei   male 30.0000     1
## 5    1st        0 Allison, Mrs. Hudson J C (Bessi female 25.0000     1
##   parch ticket  fare   cabin    embarked boat body
## 1     0  24160 211.3      B5 Southampton    2   NA
## 2     2 113781 151.6 C22 C26 Southampton   11   NA
## 3     2 113781 151.6 C22 C26 Southampton        NA
## 4     2 113781 151.6 C22 C26 Southampton       135
## 5     2 113781 151.6 C22 C26 Southampton        NA
##                         home.dest
## 1                    St Louis, MO
## 2 Montreal, PQ / Chesterville, ON
## 3 Montreal, PQ / Chesterville, ON
## 4 Montreal, PQ / Chesterville, ON
## 5 Montreal, PQ / Chesterville, ON
```

```r
set.seed(1234)
ind <- sample(2, nrow(titanic3), replace = TRUE, prob = c(0.5, 0.5))
titanic.train <- titanic3[ind == 1, ]
titanic.test <- titanic3[ind == 2, ]
```


### Examining GLM

```r
titanic.survival.train = glm(survived ~ pclass + sex + pclass:sex + age + sibsp, 
    family = binomial(logit), data = titanic.train)
summary(titanic.survival.train)
```

```
## 
## Call:
## glm(formula = survived ~ pclass + sex + pclass:sex + age + sibsp, 
##     family = binomial(logit), data = titanic.train)
## 
## Deviance Residuals: 
##    Min      1Q  Median      3Q     Max  
## -3.191  -0.617  -0.440   0.419   2.356  
## 
## Coefficients:
##                   Estimate Std. Error z value Pr(>|z|)    
## (Intercept)         5.4391     0.8846    6.15  7.8e-10 ***
## pclass2nd          -2.1740     0.8484   -2.56   0.0104 *  
## pclass3rd          -4.0704     0.7971   -5.11  3.3e-07 ***
## sexmale            -4.4559     0.7761   -5.74  9.4e-09 ***
## age                -0.0438     0.0105   -4.19  2.8e-05 ***
## sibsp              -0.2669     0.1467   -1.82   0.0688 .  
## pclass2nd:sexmale   0.6607     0.9250    0.71   0.4751    
## pclass3rd:sexmale   2.4458     0.8407    2.91   0.0036 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 721.33  on 536  degrees of freedom
## Residual deviance: 447.44  on 529  degrees of freedom
##   (125 observations deleted due to missingness)
## AIC: 463.4
## 
## Number of Fisher Scoring iterations: 6
```


### Random Forests

```r
titanic.survival.train.rf = randomForest(as.factor(survived) ~ pclass + sex + 
    age + sibsp, data = titanic.train, ntree = 5000, importance = TRUE, na.action = na.omit)
titanic.survival.train.rf
```

```
## 
## Call:
##  randomForest(formula = as.factor(survived) ~ pclass + sex + age +      sibsp, data = titanic.train, ntree = 5000, importance = TRUE,      na.action = na.omit) 
##                Type of random forest: classification
##                      Number of trees: 5000
## No. of variables tried at each split: 2
## 
##         OOB estimate of  error rate: 17.5%
## Confusion matrix:
##     0   1 class.error
## 0 290  34      0.1049
## 1  60 153      0.2817
```


### Conditional Tree Model

```r
titanic.survival.train.ctree = ctree(as.factor(survived) ~ pclass + sex + age + 
    sibsp, data = titanic.train)
titanic.survival.train.ctree
```

```
## 
## 	 Conditional inference tree with 5 terminal nodes
## 
## Response:  as.factor(survived) 
## Inputs:  pclass, sex, age, sibsp 
## Number of observations:  662 
## 
## 1) sex == {female}; criterion = 1, statistic = 216.071
##   2) pclass == {3rd}; criterion = 1, statistic = 49.951
##     3)*  weights = 98 
##   2) pclass == {1st, 2nd}
##     4)*  weights = 129 
## 1) sex == {male}
##   5) age <= 3; criterion = 0.988, statistic = 10.424
##     6)*  weights = 13 
##   5) age > 3
##     7) pclass == {1st}; criterion = 0.997, statistic = 14.358
##       8)*  weights = 84 
##     7) pclass == {2nd, 3rd}
##       9)*  weights = 338
```


### View the Plot

```r
plot(titanic.survival.train.ctree)
```



===================================================================

## Problem 2

### Load the Data

```r
gdURL <- "http://www.stat.ubc.ca/~jenny/notOcto/STAT545A/examples/gapminder/data/gapminderDataFiveYear.txt"
gDat <- read.delim(file = gdURL)
```


### Ensure Import went well

```r
str(gDat)
```

```
## 'data.frame':	1704 obs. of  6 variables:
##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
##  $ pop      : num  8425333 9240934 10267083 11537966 13079460 ...
##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
##  $ gdpPercap: num  779 821 853 836 740 ...
```


### Data Aggregation

```r
(snippet <- subset(gDat, country == "Canada"))
```

```
##     country year      pop continent lifeExp gdpPercap
## 241  Canada 1952 14785584  Americas   68.75     11367
## 242  Canada 1957 17010154  Americas   69.96     12490
## 243  Canada 1962 18985849  Americas   71.30     13462
## 244  Canada 1967 20819767  Americas   72.13     16077
## 245  Canada 1972 22284500  Americas   72.88     18971
## 246  Canada 1977 23796400  Americas   74.21     22091
## 247  Canada 1982 25201900  Americas   75.76     22899
## 248  Canada 1987 26549700  Americas   76.86     26627
## 249  Canada 1992 28523502  Americas   77.95     26343
## 250  Canada 1997 30305843  Americas   78.61     28955
## 251  Canada 2002 31902268  Americas   79.77     33329
## 252  Canada 2007 33390141  Americas   80.65     36319
```


### Load plyr & lattice

```r
library(plyr)
```

```
## 
## Attaching package: 'plyr'
## 
## The following object is masked from 'package:modeltools':
## 
##     empty
```

```r
library(lattice)
```


### Using DDply()

```r
(maxLeByCont <- ddply(gDat, ~continent, summarize, maxLifeExp = max(lifeExp)))
```

```
##   continent maxLifeExp
## 1    Africa      76.44
## 2  Americas      80.65
## 3      Asia      82.60
## 4    Europe      81.76
## 5   Oceania      81.23
```


### Study the Reutrn Value

```r
str(maxLeByCont)
```

```
## 'data.frame':	5 obs. of  2 variables:
##  $ continent : Factor w/ 5 levels "Africa","Americas",..: 1 2 3 4 5
##  $ maxLifeExp: num  76.4 80.7 82.6 81.8 81.2
```

```r
levels(maxLeByCont$continent)
```

```
## [1] "Africa"   "Americas" "Asia"     "Europe"   "Oceania"
```


### Compute Minimum GDP per capita by continent

```r
(minLeBygdpPercap <- ddply(gDat, ~continent, summarize, mingdpPercap = min(gdpPercap)))
```

```
##   continent mingdpPercap
## 1    Africa        241.2
## 2  Americas       1201.6
## 3      Asia        331.0
## 4    Europe        973.5
## 5   Oceania      10039.6
```


### Count Number of Countries for each continent

```r
ddply(gDat, ~continent, summarize, nUniqCountries = length(unique(country)))
```

```
##   continent nUniqCountries
## 1    Africa             52
## 2  Americas             25
## 3      Asia             33
## 4    Europe             30
## 5   Oceania              2
```


### Alternative Method

```r
ddply(gDat, ~continent, function(x) return(c(nUniqCountries = length(unique(x$country)))))
```

```
##   continent nUniqCountries
## 1    Africa             52
## 2  Americas             25
## 3      Asia             33
## 4    Europe             30
## 5   Oceania              2
```


### Putting Together

```r
ddply(gDat, ~continent, summarize, minLifeExp = min(lifeExp), maxLifeExp = max(lifeExp), 
    medGdpPercap = median(gdpPercap))
```

```
##   continent minLifeExp maxLifeExp medGdpPercap
## 1    Africa      23.60      76.44         1192
## 2  Americas      37.58      80.65         5466
## 3      Asia      28.80      82.60         2647
## 4    Europe      43.59      81.76        12082
## 5   Oceania      69.12      81.23        17983
```


### Combinbing aspects; 
#### Creating a Linear Regression for each country, modelling life expetnancy as a function of the year, and then retaining the estimated intercepts and slopes

```r
jCountry <- "France"
(jDat <- subset(gDat, country == jCountry))
```

```
##     country year      pop continent lifeExp gdpPercap
## 529  France 1952 42459667    Europe   67.41      7030
## 530  France 1957 44310863    Europe   68.93      8663
## 531  France 1962 47124000    Europe   70.51     10560
## 532  France 1967 49569000    Europe   71.55     13000
## 533  France 1972 51732000    Europe   72.38     16107
## 534  France 1977 53165019    Europe   73.83     18293
## 535  France 1982 54433565    Europe   74.89     20294
## 536  France 1987 55630100    Europe   76.34     22066
## 537  France 1992 57374179    Europe   77.46     24704
## 538  France 1997 58623428    Europe   78.64     25890
## 539  France 2002 59925035    Europe   79.59     28926
## 540  France 2007 61083916    Europe   80.66     30470
```


### Create Plot

```r
xyplot(lifeExp ~ year, jDat, type = c("p", "r"))
```



```r
jFit <- lm(lifeExp ~ year, jDat)
summary(jFit)
```

```
## 
## Call:
## lm(formula = lifeExp ~ year, data = jDat)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -0.3801 -0.1389  0.0124  0.1429  0.3349 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -3.98e+02   7.29e+00   -54.6  1.0e-13 ***
## year         2.38e-01   3.68e-03    64.8  1.9e-14 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.22 on 10 degrees of freedom
## Multiple R-squared:  0.998,	Adjusted R-squared:  0.997 
## F-statistic: 4.2e+03 on 1 and 10 DF,  p-value: 1.86e-14
```


### Sanity Check the Model

```r
(yearMin <- min(gDat$year))
```

```
## [1] 1952
```

```r
jFit <- lm(lifeExp ~ I(year - yearMin), jDat)
summary(jFit)
```

```
## 
## Call:
## lm(formula = lifeExp ~ I(year - yearMin), data = jDat)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -0.3801 -0.1389  0.0124  0.1429  0.3349 
## 
## Coefficients:
##                   Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       67.79013    0.11949   567.3  < 2e-16 ***
## I(year - yearMin)  0.23850    0.00368    64.8  1.9e-14 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.22 on 10 degrees of freedom
## Multiple R-squared:  0.998,	Adjusted R-squared:  0.997 
## F-statistic: 4.2e+03 on 1 and 10 DF,  p-value: 1.86e-14
```


### Check jFit

```r
class(jFit)
```

```
## [1] "lm"
```

```r
mode(jFit)
```

```
## [1] "list"
```

```r
names(jFit)
```

```
##  [1] "coefficients"  "residuals"     "effects"       "rank"         
##  [5] "fitted.values" "assign"        "qr"            "df.residual"  
##  [9] "xlevels"       "call"          "terms"         "model"
```

```r
jFit$coefficients
```

```
##       (Intercept) I(year - yearMin) 
##           67.7901            0.2385
```

```r
coef(jFit)
```

```
##       (Intercept) I(year - yearMin) 
##           67.7901            0.2385
```


### Package it as a function

```r
jFun <- function(x) coef(lm(lifeExp ~ I(year - yearMin), x))
jFun(jDat)
```

```
##       (Intercept) I(year - yearMin) 
##           67.7901            0.2385
```


### Enhance the function

```r
jFun <- function(x) {
    estCoefs <- coef(lm(lifeExp ~ I(year - yearMin), x))
    names(estCoefs) <- c("intercept", "slope")
    return(estCoefs)
}
jFun(jDat)
```

```
## intercept     slope 
##   67.7901    0.2385
```


### Test the function with examples

```r
jFun(subset(gDat, country == "Canada"))
```

```
## intercept     slope 
##   68.8838    0.2189
```

```r
jFun(subset(gDat, country == "Uruguay"))
```

```
## intercept     slope 
##   65.7416    0.1833
```

```r
jFun(subset(gDat, country == "India"))
```

```
## intercept     slope 
##   39.2698    0.5053
```


### Place function inside a ddply() call

```r
jCoefs <- ddply(gDat, ~country, jFun)
str(jCoefs)
```

```
## 'data.frame':	142 obs. of  3 variables:
##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 2 3 4 5 6 7 8 9 10 ...
##  $ intercept: num  29.9 59.2 43.4 32.1 62.7 ...
##  $ slope    : num  0.275 0.335 0.569 0.209 0.232 ...
```

```r
tail(jCoefs)
```

```
##                country intercept    slope
## 137          Venezuela     57.51  0.32972
## 138            Vietnam     39.01  0.67162
## 139 West Bank and Gaza     43.80  0.60110
## 140        Yemen, Rep.     30.13  0.60546
## 141             Zambia     47.66 -0.06043
## 142           Zimbabwe     55.22 -0.09302
```


### Summarize and save the script for working forward

```r
gdURL <- "http://www.stat.ubc.ca/~jenny/notOcto/STAT545A/examples/gapminder/data/gapminderDataFiveYear.txt"
gDat <- read.delim(file = gdURL)
## str(gDat) here when working interactively
yearMin <- min(gDat$year)
jFun <- function(x) {
    estCoefs <- coef(lm(lifeExp ~ I(year - yearMin), x))
    names(estCoefs) <- c("intercept", "slope")
    return(estCoefs)
}
## jFun(subset(gDat, country == 'India')) to see what it does
jCoefs <- ddply(gDat, ~country, jFun)
```


### Begin working with Xtable; load the package

```r
library(xtable)
```


### Begin examining estimated coefficeints; may need to use results = 'asis' to correctly display

```r
set.seed(916)
foo <- jCoefs[sample(nrow(jCoefs), size = 15), ]
foo <- xtable(foo)
print(foo, type = "html", include.rownames = FALSE)
```

<!-- html table generated in R 3.0.3 by xtable 1.7-3 package -->
<!-- Mon Apr 07 19:02:11 2014 -->
<TABLE border=1>
<TR> <TH> country </TH> <TH> intercept </TH> <TH> slope </TH>  </TR>
  <TR> <TD> Lebanon </TD> <TD align="right"> 58.69 </TD> <TD align="right"> 0.26 </TD> </TR>
  <TR> <TD> Senegal </TD> <TD align="right"> 36.75 </TD> <TD align="right"> 0.50 </TD> </TR>
  <TR> <TD> Dominican Republic </TD> <TD align="right"> 48.60 </TD> <TD align="right"> 0.47 </TD> </TR>
  <TR> <TD> Oman </TD> <TD align="right"> 37.21 </TD> <TD align="right"> 0.77 </TD> </TR>
  <TR> <TD> Germany </TD> <TD align="right"> 67.57 </TD> <TD align="right"> 0.21 </TD> </TR>
  <TR> <TD> Korea, Dem. Rep. </TD> <TD align="right"> 54.91 </TD> <TD align="right"> 0.32 </TD> </TR>
  <TR> <TD> Mauritius </TD> <TD align="right"> 55.37 </TD> <TD align="right"> 0.35 </TD> </TR>
  <TR> <TD> Slovak Republic </TD> <TD align="right"> 67.01 </TD> <TD align="right"> 0.13 </TD> </TR>
  <TR> <TD> Comoros </TD> <TD align="right"> 40.00 </TD> <TD align="right"> 0.45 </TD> </TR>
  <TR> <TD> Argentina </TD> <TD align="right"> 62.69 </TD> <TD align="right"> 0.23 </TD> </TR>
  <TR> <TD> Central African Republic </TD> <TD align="right"> 38.81 </TD> <TD align="right"> 0.18 </TD> </TR>
  <TR> <TD> Ecuador </TD> <TD align="right"> 49.07 </TD> <TD align="right"> 0.50 </TD> </TR>
  <TR> <TD> West Bank and Gaza </TD> <TD align="right"> 43.80 </TD> <TD align="right"> 0.60 </TD> </TR>
  <TR> <TD> Egypt </TD> <TD align="right"> 40.97 </TD> <TD align="right"> 0.56 </TD> </TR>
  <TR> <TD> Myanmar </TD> <TD align="right"> 41.41 </TD> <TD align="right"> 0.43 </TD> </TR>
   </TABLE>


### Enhance our ddply()

```r
jCoefs <- ddply(gDat, ~country + continent, jFun)
str(jCoefs)
```

```
## 'data.frame':	142 obs. of  4 variables:
##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 2 3 4 5 6 7 8 9 10 ...
##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 4 1 1 2 5 4 3 3 4 ...
##  $ intercept: num  29.9 59.2 43.4 32.1 62.7 ...
##  $ slope    : num  0.275 0.335 0.569 0.209 0.232 ...
```

```r
tail(jCoefs)
```

```
##                country continent intercept    slope
## 137          Venezuela  Americas     57.51  0.32972
## 138            Vietnam      Asia     39.01  0.67162
## 139 West Bank and Gaza      Asia     43.80  0.60110
## 140        Yemen, Rep.      Asia     30.13  0.60546
## 141             Zambia    Africa     47.66 -0.06043
## 142           Zimbabwe    Africa     55.22 -0.09302
```


### Sort the data.frame from shortest to largest

```r
set.seed(916)
foo <- jCoefs[sample(nrow(jCoefs), size = 15), ]
foo <- arrange(foo, intercept)
## foo <- foo[order(foo$intercept), ] # an uglier non-plyr way
foo <- xtable(foo)
print(foo, type = "html", include.rownames = FALSE)
```

<!-- html table generated in R 3.0.3 by xtable 1.7-3 package -->
<!-- Mon Apr 07 19:02:11 2014 -->
<TABLE border=1>
<TR> <TH> country </TH> <TH> continent </TH> <TH> intercept </TH> <TH> slope </TH>  </TR>
  <TR> <TD> Senegal </TD> <TD> Africa </TD> <TD align="right"> 36.75 </TD> <TD align="right"> 0.50 </TD> </TR>
  <TR> <TD> Oman </TD> <TD> Asia </TD> <TD align="right"> 37.21 </TD> <TD align="right"> 0.77 </TD> </TR>
  <TR> <TD> Central African Republic </TD> <TD> Africa </TD> <TD align="right"> 38.81 </TD> <TD align="right"> 0.18 </TD> </TR>
  <TR> <TD> Comoros </TD> <TD> Africa </TD> <TD align="right"> 40.00 </TD> <TD align="right"> 0.45 </TD> </TR>
  <TR> <TD> Egypt </TD> <TD> Africa </TD> <TD align="right"> 40.97 </TD> <TD align="right"> 0.56 </TD> </TR>
  <TR> <TD> Myanmar </TD> <TD> Asia </TD> <TD align="right"> 41.41 </TD> <TD align="right"> 0.43 </TD> </TR>
  <TR> <TD> West Bank and Gaza </TD> <TD> Asia </TD> <TD align="right"> 43.80 </TD> <TD align="right"> 0.60 </TD> </TR>
  <TR> <TD> Dominican Republic </TD> <TD> Americas </TD> <TD align="right"> 48.60 </TD> <TD align="right"> 0.47 </TD> </TR>
  <TR> <TD> Ecuador </TD> <TD> Americas </TD> <TD align="right"> 49.07 </TD> <TD align="right"> 0.50 </TD> </TR>
  <TR> <TD> Korea, Dem. Rep. </TD> <TD> Asia </TD> <TD align="right"> 54.91 </TD> <TD align="right"> 0.32 </TD> </TR>
  <TR> <TD> Mauritius </TD> <TD> Africa </TD> <TD align="right"> 55.37 </TD> <TD align="right"> 0.35 </TD> </TR>
  <TR> <TD> Lebanon </TD> <TD> Asia </TD> <TD align="right"> 58.69 </TD> <TD align="right"> 0.26 </TD> </TR>
  <TR> <TD> Argentina </TD> <TD> Americas </TD> <TD align="right"> 62.69 </TD> <TD align="right"> 0.23 </TD> </TR>
  <TR> <TD> Slovak Republic </TD> <TD> Europe </TD> <TD align="right"> 67.01 </TD> <TD align="right"> 0.13 </TD> </TR>
  <TR> <TD> Germany </TD> <TD> Europe </TD> <TD align="right"> 67.57 </TD> <TD align="right"> 0.21 </TD> </TR>
   </TABLE>


## Q & A

### Passing more than one argument for a function into ddply()

```r
(yearMin <- min(gDat$year))
```

```
## [1] 1952
```



```r
jFun <- function(x) {
    estCoefs <- coef(lm(lifeExp ~ I(year - yearMin), x))
    names(estCoefs) <- c("intercept", "slope")
    return(estCoefs)
}
jCoefs <- ddply(gDat, ~country, jFun)
head(jCoefs)
```

```
##       country intercept  slope
## 1 Afghanistan     29.91 0.2753
## 2     Albania     59.23 0.3347
## 3     Algeria     43.37 0.5693
## 4      Angola     32.13 0.2093
## 5   Argentina     62.69 0.2317
## 6   Australia     68.40 0.2277
```


### Answer

```r
jFunTwoArgs <- function(x, cvShift = 0) {
    estCoefs <- coef(lm(lifeExp ~ I(year - cvShift), x))
    names(estCoefs) <- c("intercept", "slope")
    return(estCoefs)
}
```


### Now use a simple call

```r
jCoefsSilly <- ddply(gDat, ~country, jFunTwoArgs)
head(jCoefsSilly)
```

```
##       country intercept  slope
## 1 Afghanistan    -507.5 0.2753
## 2     Albania    -594.1 0.3347
## 3     Algeria   -1067.9 0.5693
## 4      Angola    -376.5 0.2093
## 5   Argentina    -389.6 0.2317
## 6   Australia    -376.1 0.2277
```


### Use cvSHift = to resolve issues w/ estimated slopes

```r
jCoefsSane <- ddply(gDat, ~country, jFunTwoArgs, cvShift = 1952)
head(jCoefsSane)
```

```
##       country intercept  slope
## 1 Afghanistan     29.91 0.2753
## 2     Albania     59.23 0.3347
## 3     Algeria     43.37 0.5693
## 4      Angola     32.13 0.2093
## 5   Argentina     62.69 0.2317
## 6   Australia     68.40 0.2277
```


### Tighten up the code for 1952

```r
jCoefsBest <- ddply(gDat, ~country, jFunTwoArgs, cvShift = min(gDat$year))
head(jCoefsBest)
```

```
##       country intercept  slope
## 1 Afghanistan     29.91 0.2753
## 2     Albania     59.23 0.3347
## 3     Algeria     43.37 0.5693
## 4      Angola     32.13 0.2093
## 5   Argentina     62.69 0.2317
## 6   Australia     68.40 0.2277
```


=========================================================================

## Problem 3

### Hierarchical Clustering
#### Read in the Data

```r
cars <- "http://www.stat.berkeley.edu/classes/s133/data/cars.tab"
cars <- read.delim(file = cars)
```


### View the first few records

```r
head(cars)
```

```
##   Country                       Car  MPG Weight Drive_Ratio Horsepower
## 1    U.S.        Buick Estate Wagon 16.9  4.360        2.73        155
## 2    U.S. Ford Country Squire Wagon 15.5  4.054        2.26        142
## 3    U.S.        Chevy Malibu Wagon 19.2  3.605        2.56        125
## 4    U.S.    Chrysler LeBaron Wagon 18.5  3.940        2.45        150
## 5    U.S.                  Chevette 30.0  2.155        3.70         68
## 6   Japan             Toyota Corona 27.5  2.560        3.05         95
##   Displacement Cylinders
## 1          350         8
## 2          351         8
## 3          267         8
## 4          360         8
## 5           98         4
## 6          134         4
```


### Standardize the median and divide by the mean average deviation

```r
cars.use = cars[, -c(1, 2)]
medians = apply(cars.use, 2, median)
mads = apply(cars.use, 2, mad)
cars.use = scale(cars.use, center = medians, scale = mads)
```


### Default Euclidean Distance

```r
cars.dist = dist(cars.use)
```


### Perform hierarchical cluster analysis

```r
cars.hclust = hclust(cars.dist)
```


### Create a dendogram

```r
plot(cars.hclust, labels = cars$Car, main = "Default from hclust")
```

![plot of chunk unnamed-chunk-42](figure/unnamed-chunk-42.png) 


### Get the cluster memberships for the three cluster solutions

```r
groups.3 = cutree(cars.hclust, 3)
```


### View the Results

```r
table(groups.3)
```

```
## groups.3
##  1  2  3 
##  8 20 10
```


### Examine the clusters for solutions ranging from 2-6 clusters

```r
counts = sapply(2:6, function(ncl) table(cutree(cars.hclust, ncl)))
names(counts) = 2:6
counts
```

```
## $`2`
## 
##  1  2 
## 18 20 
## 
## $`3`
## 
##  1  2  3 
##  8 20 10 
## 
## $`4`
## 
##  1  2  3  4 
##  8 17  3 10 
## 
## $`5`
## 
##  1  2  3  4  5 
##  8 11  6  3 10 
## 
## $`6`
## 
##  1  2  3  4  5  6 
##  8 11  6  3  5  5
```


### Examine Cars in the first Four Clusters

```r
cars$Car[groups.3 == 1]
```

```
## [1] Buick Estate Wagon        Ford Country Squire Wagon
## [3] Chevy Malibu Wagon        Chrysler LeBaron Wagon   
## [5] Chevy Caprice Classic     Ford LTD                 
## [7] Mercury Grand Marquis     Dodge St Regis           
## 38 Levels: AMC Concord D/L AMC Spirit Audi 5000 ... VW Scirocco
```

```r
sapply(unique(groups.3), function(g) cars$Car[groups.3 == g])
```

```
## [[1]]
## [1] Buick Estate Wagon        Ford Country Squire Wagon
## [3] Chevy Malibu Wagon        Chrysler LeBaron Wagon   
## [5] Chevy Caprice Classic     Ford LTD                 
## [7] Mercury Grand Marquis     Dodge St Regis           
## 38 Levels: AMC Concord D/L AMC Spirit Audi 5000 ... VW Scirocco
## 
## [[2]]
##  [1] Chevette         Toyota Corona    Datsun 510       Dodge Omni      
##  [5] Audi 5000        Saab 99 GLE      Ford Mustang 4   Mazda GLC       
##  [9] Dodge Colt       AMC Spirit       VW Scirocco      Honda Accord LX 
## [13] Buick Skylark    Pontiac Phoenix  Plymouth Horizon Datsun 210      
## [17] Fiat Strada      VW Dasher        BMW 320i         VW Rabbit       
## 38 Levels: AMC Concord D/L AMC Spirit Audi 5000 ... VW Scirocco
## 
## [[3]]
##  [1] Volvo 240 GL          Peugeot 694 SL        Buick Century Special
##  [4] Mercury Zephyr        Dodge Aspen           AMC Concord D/L      
##  [7] Ford Mustang Ghia     Chevy Citation        Olds Omega           
## [10] Datsun 810           
## 38 Levels: AMC Concord D/L AMC Spirit Audi 5000 ... VW Scirocco
```


### Repeat on the four cluster solution

```r
groups.4 = cutree(cars.hclust, 4)
sapply(unique(groups.4), function(g) cars$Car[groups.4 == g])
```

```
## [[1]]
## [1] Buick Estate Wagon        Ford Country Squire Wagon
## [3] Chevy Malibu Wagon        Chrysler LeBaron Wagon   
## [5] Chevy Caprice Classic     Ford LTD                 
## [7] Mercury Grand Marquis     Dodge St Regis           
## 38 Levels: AMC Concord D/L AMC Spirit Audi 5000 ... VW Scirocco
## 
## [[2]]
##  [1] Chevette         Toyota Corona    Datsun 510       Dodge Omni      
##  [5] Ford Mustang 4   Mazda GLC        Dodge Colt       AMC Spirit      
##  [9] VW Scirocco      Honda Accord LX  Buick Skylark    Pontiac Phoenix 
## [13] Plymouth Horizon Datsun 210       Fiat Strada      VW Dasher       
## [17] VW Rabbit       
## 38 Levels: AMC Concord D/L AMC Spirit Audi 5000 ... VW Scirocco
## 
## [[3]]
## [1] Audi 5000   Saab 99 GLE BMW 320i   
## 38 Levels: AMC Concord D/L AMC Spirit Audi 5000 ... VW Scirocco
## 
## [[4]]
##  [1] Volvo 240 GL          Peugeot 694 SL        Buick Century Special
##  [4] Mercury Zephyr        Dodge Aspen           AMC Concord D/L      
##  [7] Ford Mustang Ghia     Chevy Citation        Olds Omega           
## [10] Datsun 810           
## 38 Levels: AMC Concord D/L AMC Spirit Audi 5000 ... VW Scirocco
```


### Produce a cross-tabulation of cluster group membership and country of origin

```r
table(groups.3, cars$Country)
```

```
##         
## groups.3 France Germany Italy Japan Sweden U.S.
##        1      0       0     0     0      0    8
##        2      0       5     1     6      1    7
##        3      1       0     0     1      1    7
```


### Aggregate

```r
aggregate(cars.use, list(groups.3), median)
```

```
##   Group.1     MPG  Weight Drive_Ratio Horsepower Displacement Cylinders
## 1       1 -0.7945  1.5051     -0.9134     1.0476       2.4776    4.7214
## 2       2  0.6859 -0.5871      0.5269    -0.6027      -0.5810   -0.6745
## 3       3 -0.4058  0.5246     -0.1686     0.3588       0.3272    2.0235
```


### Add the numbers of observations in each group to the display

```r
a3 = aggregate(cars[, -c(1, 2)], list(groups.3), median)
data.frame(Cluster = a3[, 1], Freq = as.vector(table(groups.3)), a3[, -1])
```

```
##   Cluster Freq   MPG Weight Drive_Ratio Horsepower Displacement Cylinders
## 1       1    8 17.30  3.890       2.430      136.5          334         8
## 2       2   20 30.25  2.215       3.455       79.0          105         4
## 3       3   10 20.70  3.105       2.960      112.5          173         6
```


### Compare the four cluster solution to the three cluster

```r
a4 = aggregate(cars[, -c(1, 2)], list(groups.4), median)
data.frame(Cluster = a4[, 1], Freq = as.vector(table(groups.4)), a4[, -1])
```

```
##   Cluster Freq  MPG Weight Drive_Ratio Horsepower Displacement Cylinders
## 1       1    8 17.3  3.890        2.43      136.5          334         8
## 2       2   17 30.9  2.190        3.37       75.0           98         4
## 3       3    3 21.5  2.795        3.77      110.0          121         4
## 4       4   10 20.7  3.105        2.96      112.5          173         6
```


===========================================================================

## Part 2: K-Means Clustering

### Load the Packages

```r
library(rattle)
```

```
## Rattle: A free graphical interface for data mining with R.
## Version 3.0.2 r169 Copyright (c) 2006-2013 Togaware Pty Ltd.
## Type 'rattle()' to shake, rattle, and roll your data.
```

```r
library(NbClust)
library(flexclust)
```


### Create the K- Means graph

```r
wssplot <- function(data, nc = 15, seed = 1234) {
    wss <- (nrow(data) - 1) * sum(apply(data, 2, var))
    for (i in 2:nc) {
        set.seed(seed)
        wss[i] <- sum(kmeans(data, centers = i)$withinss)
    }
    plot(1:nc, wss, type = "b", xlab = "Number of Clusters", ylab = "Within groups sum of squares")
}
```


### #1 Standardize the Data

```r
data(wine, package = "rattle")
head(wine)
```

```
##   Type Alcohol Malic  Ash Alcalinity Magnesium Phenols Flavanoids
## 1    1   14.23  1.71 2.43       15.6       127    2.80       3.06
## 2    1   13.20  1.78 2.14       11.2       100    2.65       2.76
## 3    1   13.16  2.36 2.67       18.6       101    2.80       3.24
## 4    1   14.37  1.95 2.50       16.8       113    3.85       3.49
## 5    1   13.24  2.59 2.87       21.0       118    2.80       2.69
## 6    1   14.20  1.76 2.45       15.2       112    3.27       3.39
##   Nonflavanoids Proanthocyanins Color  Hue Dilution Proline
## 1          0.28            2.29  5.64 1.04     3.92    1065
## 2          0.26            1.28  4.38 1.05     3.40    1050
## 3          0.30            2.81  5.68 1.03     3.17    1185
## 4          0.24            2.18  7.80 0.86     3.45    1480
## 5          0.39            1.82  4.32 1.04     2.93     735
## 6          0.34            1.97  6.75 1.05     2.85    1450
```

```r
df <- scale(wine[-1])
```



```r
wssplot(df)
```



### #2 Determine the number of clusters

```r
set.seed(1234)
nc <- NbClust(df, min.nc = 2, max.nc = 15, method = "kmeans")
```


```
## [1] "*** : The Hubert index is a graphical method of determining the number of clusters. In the plot of Hubert index, we seek a significant knee that corresponds to a significant increase of the value of the measure i.e the significant peak in Hubert index second differences plot."
```


```
## [1] "*** : The D index is a graphical method of determining the number of clusters. In the plot of D index, we seek a significant knee (the significant peak in Dindex second differences plot) that corresponds to a significant increase of the value of the measure."
## [1] "Pseudot2 and Frey indices can be applied only to hierarchical methods "
## [1] "All 178 observations were used."
```

```r
table(nc$Best.n[1, ])
```

```
## 
##  0  2  3  8 13 14 15 
##  2  3 14  1  2  1  1
```


#### Create the Plot

```r
barplot(table(nc$Best.n[1, ]), xlab = "Numer of Clusters", ylab = "Number of Criteria", 
    main = "Number of Clusters Chosen by 26 Criteria")
```



```r
set.seed(1234)
fit.km <- kmeans(df, 3, nstart = 25)
```


### 3 K-Means Cluster Analysis

```r
fit.km$size
```

```
## [1] 62 65 51
```



```r
fit.km$centers
```

```
##   Alcohol   Malic     Ash Alcalinity Magnesium  Phenols Flavanoids
## 1  0.8329 -0.3030  0.3637    -0.6085   0.57596  0.88275    0.97507
## 2 -0.9235 -0.3929 -0.4931     0.1701  -0.49033 -0.07577    0.02075
## 3  0.1644  0.8691  0.1864     0.5229  -0.07526 -0.97658   -1.21183
##   Nonflavanoids Proanthocyanins   Color     Hue Dilution Proline
## 1      -0.56051          0.5787  0.1706  0.4727   0.7771  1.1220
## 2      -0.03344          0.0581 -0.8994  0.4605   0.2700 -0.7517
## 3       0.72402         -0.7775  0.9389 -1.1615  -1.2888 -0.4059
```



```r
aggregate(wine[-1], by = list(cluster = fit.km$cluster), mean)
```

```
##   cluster Alcohol Malic   Ash Alcalinity Magnesium Phenols Flavanoids
## 1       1   13.68 1.998 2.466      17.46    107.97   2.848     3.0032
## 2       2   12.25 1.897 2.231      20.06     92.74   2.248     2.0500
## 3       3   13.13 3.307 2.418      21.24     98.67   1.684     0.8188
##   Nonflavanoids Proanthocyanins Color   Hue Dilution Proline
## 1        0.2921           1.922 5.454 1.065    3.163  1100.2
## 2        0.3577           1.624 2.973 1.063    2.803   510.2
## 3        0.4520           1.146 7.235 0.692    1.697   619.1
```


### Create the Cross-tabulation of Type 

```r
ct.km <- table(wine$Type, fit.km$cluster)
ct.km
```

```
##    
##      1  2  3
##   1 59  0  0
##   2  3 65  3
##   3  0  0 48
```



```r
randIndex(ct.km)
```

```
##    ARI 
## 0.8975
```

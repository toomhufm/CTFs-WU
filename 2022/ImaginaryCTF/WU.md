


# Forensics
## journey
![image](https://user-images.githubusercontent.com/92283038/179663882-5cc10682-7c4b-459b-b2f2-c47f48e44c5e.png)

![image](https://user-images.githubusercontent.com/92283038/179664002-0cfa99da-e7f3-4f3c-9089-f758df86da3f.png)

Sử dụng Google Lens thì ta tìm được địa điểm này với từ khóa **magoni via del duomo**. Sau đó tìm trên Google Map và ta có tọa độ
![image](https://user-images.githubusercontent.com/92283038/179664376-c44d7fc7-4c1b-4b8d-8949-374836649e21.png)

```
flag : ictf{42.717_12.112}
```

## unpuzzled3 & 4
![image](https://user-images.githubusercontent.com/92283038/179664631-50a26312-09ea-4678-b1a2-f5b4cb393a85.png)
![image](https://user-images.githubusercontent.com/92283038/179698811-267f0e62-9522-4f93-9fe0-2965d088e2c0.png)

Tìm user **unpuzzler7#6451** trong Discord của giải
![image](https://user-images.githubusercontent.com/92283038/179698727-82019e9f-e5b6-46d9-b89c-1931bad3c15c.png)

Ta có 1 đường dẫn và 1 profile Spotify. Bài 3 có nhắc đến music nên mình vào Spotify thử, vào playlist [Cool Songs](https://open.spotify.com/playlist/1TNjz7ObseVlOQ2U7PqPY4), để ý sẽ 
thấy các chữ đầu tên bài hát là flag.

`` flag : ictf{SPOTIFY_jAMMMMMM_78D5B4} ``

Còn trong profile [flickr](https://www.flickr.com/photos/unpuzzler7/52210715641/), vào tấm ảnh bầu trời thì ta sẽ thấy flag nằm ở phần Exif

![image](https://user-images.githubusercontent.com/92283038/179699667-6d9dea3c-1627-4d47-b5d6-fe001dcb56c6.png)


`` flag : ictf{1mgur_d03sn't_cl3ar_3xif} ``

## Orge 
![image](https://user-images.githubusercontent.com/92283038/179700032-eb8dfdaf-44c9-4ea5-8359-dc87dc3f3392.png)

Sau khi pull images từ docker về thì mình sử dụng [dive](https://github.com/wagoodman/dive) để xem các lớp của images.

![image](https://user-images.githubusercontent.com/92283038/179700395-5e06a8eb-8068-4b26-bdc4-4ce2c9bf74b6.png)

Ở đây có 1 lệch echo 1 đoạn base64 vào secret gì đó nên chắc đây là flag. Lấy đoạn base64 decode ra

`` flag : ictf{onions_have_layers_images_have_layers} ``

## bsv

![image](https://user-images.githubusercontent.com/92283038/179701241-d141023d-7482-4242-9886-256fa5495eef.png)


       strings flag.bsv
    BEEAccordingBEEtoBEEallBEEknownBEE BEE BEElawsBEEofBEEaviationBEEthereBEEisBEEnoBEE BEE BEEwayBEEaBEEbeeBEEshouldBEEbeBEEableBEEtoBEEfly.BEE BEEItsBEEwingsBEEareBEEtooBEEsmallBEEtoBEEgetBEEitsBEE BEEfatBEElittleBEEbodyBEEoffBEEtheBEEground.BEETheBEEbeeBEE BEE BEEofBEEcourseBEE BEE BEE BEE BEE BEEfliesBEEanywayBEE BEEbecauseBEEbeesBEEdon'tBEEcareBEEwhatBEEhumansBEEthinkBEEisBEE BEEimpossible.BEEYellowBEEblack.BEEYellowBEEblack.BEEYellowBEEblack.BEEYellowBEE BEE BEE BEE BEE BEE BEE BEE BEE BEEblack.BEEOohBEEblackBEEandBEEyellow!BEELet'sBEEshakeBEEitBEE BEE BEEupBEEaBEE BEE BEE BEE BEE BEElittle.BEEBarry!BEE BEEBreakfastBEEisBEEready!BEEComing!BEEHangBEEonBEEaBEEsecond.BEE BEEHello?BEEBarry?BEEAdam?BEECanBEEyouBEEbelieveBEEthisBEEisBEE BEE BEE BEE BEE BEE BEE BEE BEE BEEhappening?BEEIBEEcan't.BEEI'llBEEpickBEEyouBEEup.BEELookingBEE BEE BEE BEEsharp.BEEUseBEEtheBEEstairsBEEYourBEEfatherBEEpaidBEE BEE BEEgoodBEEmoneyBEEforBEEthose.BEESorry.BEEI'mBEEexcited.BEEHere'sBEE BEE BEE BEE BEEtheBEEgraduate.BEE BEE BEE BEE BEEWe'reBEEveryBEEproudBEEofBEEyouBEEson.BEEABEE BEE BEE BEE BEE BEEperfectBEEreportBEEcardBEE BEE BEE BEE BEE
    BEE BEEallBEEB's.BEE BEE BEEVeryBEEproud.BEE BEE BEE BEE BEEMa!BEEIBEE BEE BEE BEE BEEgotBEEaBEE BEE BEE BEE BEEthingBEEgoingBEE BEE BEE BEE BEE BEE BEE BEEhere.BEEYouBEE BEE BEE BEE BEE BEEgotBEElintBEE BEEonBEEyourBEE BEE BEE BEE BEE BEEfuzz.BEEOw!BEE BEE BEE BEE BEE BEE BEEThat'sBEEme!BEE BEE BEE BEE BEE BEE BEE BEEWaveBEEtoBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEus!BEEWe'llBEE BEE BEE BEE BEE BEEbeBEEinBEE BEErowBEE118000.BEE BEE BEE BEE BEE BEEBye!BEEBarryBEE BEE BEE BEE BEE BEE BEEIBEEtoldBEE BEE BEE BEE BEE BEE BEE BEEyouBEEstopBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEflyingBEEinBEE BEE BEE BEE BEE BEEtheBEEhouse!BEE BEEHeyBEEAdam.BEE BEE BEE BEE BEE BEEHeyBEEBarry.BEE BEEIsBEEthatBEE BEE BEE BEE BEE BEE BEE BEE BEEfuzzBEEgel?BEEABEElittle.BEE BEE BEE BEESpecialBEEdayBEE BEE BEE BEE BEE BEEgraduation.BEENeverBEE BEE BEE BEEthoughtBEEI'dBEE BEEmakeBEEit.BEE BEE BEE BEE
    BEE BEEThreeBEEdaysBEE BEE BEEgradeBEEschoolBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEthreeBEEdaysBEE BEE BEE BEE BEEhighBEEschool.BEE BEE BEE BEE BEE BEE BEE BEEThoseBEEwereBEE BEE BEE BEE BEE BEEawkward.BEEThreeBEE BEEdaysBEEcollege.BEE BEE BEE BEE BEE BEEI'mBEEgladBEE BEE BEE BEE BEE BEEIBEEtookBEE BEE BEE BEE BEE BEE BEE BEEaBEEdayBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEandBEEhitchhikedBEE BEE BEE BEE BEE BEEaroundBEETheBEE BEEHive.BEEYouBEE BEE BEE BEE BEE BEEdidBEEcomeBEE BEE BEE BEE BEE BEEbackBEEdifferent.BEE BEE BEE BEE BEE BEE BEE BEEHiBEEBarry.BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEArtieBEEgrowingBEE BEE BEE BEE BEE BEEaBEEmustache?BEE BEE BEE BEE BEE BEE BEE BEE BEELooksBEEgood.BEE BEEHearBEEaboutBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEFrankie?BEEYeah.BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEYouBEEgoingBEE BEE BEEtoBEEtheBEE BEE BEE BEEfuneral?BEENoBEE BEE BEE
    BEE BEEI'mBEEnotBEE BEE BEEgoing.BEEEverybodyBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEknowsBEEstingBEE BEE BEE BEE BEEsomeoneBEEyouBEEdie.BEEDon'tBEEwasteBEEitBEE BEE BEE BEEonBEEaBEEsquirrel.BEESuchBEEaBEEhothead.BEEIBEEguessBEE BEE BEEheBEEcouldBEE BEE BEE BEE BEE BEEhaveBEEjustBEE BEE BEE BEE BEEgottenBEEoutBEE BEE BEE BEE BEE BEE BEE BEEofBEEtheBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEway.BEEIBEEloveBEEthisBEEincorporatingBEEanBEEamusementBEEparkBEE BEE BEEintoBEEourBEE BEE BEE BEE BEE BEEday.BEEThat'sBEE BEE BEE BEE BEEwhyBEEweBEE BEE BEE BEE BEE BEE BEE BEEdon'tBEEneedBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEvacations.BEEBoyBEEquiteBEEaBEEbitBEEofBEEpompBEEunderBEE BEE BEE BEEtheBEEcircumstances.BEEWellBEEAdamBEEtodayBEEweBEEareBEE BEE BEEmen.BEEWeBEEare!BEEBee-men.BEEAmen!BEEHallelujah!BEE BEE BEE BEE BEE BEE BEEStudentsBEEfacultyBEE BEE BEE BEE BEEdistinguishedBEEbeesBEEpleaseBEEwelcomeBEEDeanBEEBuzzwell.BEEWelcomeBEE BEE BEENewBEEHiveBEE BEE BEE BEE BEE BEECityBEEgraduatingBEE BEE
    BEE BEEclassBEEofBEE BEE BEE9:15.BEEThatBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEconcludesBEEourBEE BEE BEE BEE BEEceremoniesBEEAndBEE BEE BEE BEE BEE BEE BEE BEEbeginsBEEyourBEE BEE BEE BEE BEE BEEcareerBEEatBEE BEEHonexBEEIndustries!BEE BEE BEE BEE BEE BEEWillBEEweBEE BEE BEE BEEpickBEEourBEE BEE BEE BEE BEE BEE BEE BEEjobBEEtoday?BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEIBEEheardBEE BEE BEE BEE BEE BEEit'sBEEjustBEE BEEorientation.BEEHeadsBEE BEE BEE BEE BEE BEEup!BEEHereBEE BEE BEE BEEweBEEgo.BEE BEE BEE BEE BEE BEE BEE BEEKeepBEEyourBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEhandsBEEandBEE BEE BEE BEE BEE BEEantennasBEEinsideBEE BEEtheBEEtramBEE BEE BEE BEE BEE BEE BEE BEE BEEatBEEallBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEtimes.BEEWonderBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEwhatBEEit'llBEE BEEbeBEElike?BEEABEElittleBEEscary.BEEWelcomeBEEtoBEEHonexBEEaBEE BEE
    BEE BEEdivisionBEEofBEE BEE BEEHonescoBEEandBEE BEE BEE BEE BEEaBEEpartBEE BEE BEE BEE BEEofBEEtheBEE BEE BEE BEE BEEHexagonBEEGroup.BEE BEE BEE BEE BEE BEE BEE BEEThisBEEisBEE BEE BEE BEE BEE BEEit!BEEWow.BEE BEEWow.BEEWeBEE BEE BEE BEE BEE BEEknowBEEthatBEE BEE BEEyouBEEasBEE BEE BEE BEE BEE BEE BEE BEEaBEEbeeBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEhaveBEEworkedBEE BEE BEE BEE BEE BEEyourBEEwholeBEE BEElifeBEEtoBEE BEE BEE BEE BEE BEEgetBEEtoBEE BEE BEEtheBEEpointBEE BEE BEE BEE BEE BEE BEE BEEwhereBEEyouBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEcanBEEworkBEE BEE BEE BEE BEE BEEforBEEyourBEE BEEwholeBEElife.BEE BEE BEE BEE BEE BEE BEE BEE BEEHoneyBEEbeginsBEE BEE BEE BEE BEE BEE BEE BEE BEE BEE BEEwhenBEEourBEE BEE BEE BEEvaliantBEEPollenBEE BEE BEE BEE BEE BEEJocksBEEbringBEE BEEtheBEEnectarBEE BEE BEE BEE BEE BEEtoBEETheBEE BEE
    BEEHive.BEEOurBEEtop-secretBEEformulaBEE BEE BEEisBEEautomaticallyBEEcolor-correctedBEEscent-adjustedBEEandBEEbubble-contouredBEE BEE BEE BEE BEE BEEintoBEEthisBEE BEE BEE BEE BEEsoothingBEEsweetBEE BEE BEE BEE BEE BEE BEE BEEsyrupBEEwithBEEitsBEEdistinctiveBEEgoldenBEEglowBEEyouBEEknowBEE BEE BEE BEEas...BEEHoney!BEEThatBEEgirlBEEwasBEEhot.BEEShe'sBEE BEE BEEmyBEEcousin!BEESheBEEis?BEEYesBEEwe'reBEEallBEEcousins.BEE BEERight.BEEYou'reBEEright.BEEAtBEEHonexBEEweBEEconstantlyBEEstriveBEE BEEtoBEEimproveBEEeveryBEEaspectBEEofBEEbeeBEEexistence.BEE BEETheseBEEbeesBEEareBEEstress-testingBEEaBEEnewBEEhelmetBEEtechnology.BEE BEE BEE BEEWhatBEEdoBEEyouBEEthinkBEEheBEEmakes?BEENotBEE BEE BEEenough.BEEHereBEEweBEEhaveBEEourBEElatestBEEadvancementBEEtheBEE BEEKrelman.BEEWhatBEEdoesBEEthatBEEdo?BEECatchesBEEthatBEElittleBEE BEEstrandBEEofBEEhoneyBEEthatBEEhangsBEEafterBEEyouBEE BEEpourBEEit.BEESavesBEEusBEEmillions.BEECanBEEanyoneBEEworkBEE BEE BEEonBEEtheBEEKrelman?BEEOfBEEcourse.BEEMostBEEbeeBEEjobsBEEareBEE BEEsmallBEEones.BEE BEE BEE BEE BEE BEE BEE BEE BEEButBEEbeesBEEknowBEEthatBEEeveryBEEsmallBEE BEE BEEjobBEEifBEEit'sBEEdoneBEEwellBEEmeansBEEaBEE BEE BEElot.BEEButBEE BEE BEE BEE BEE BEEchooseBEEcarefullyBEE BEE

Ta có thể thấy có rất nhiều từ **BEE** , mình replace tất cả bằng dấu phẩy cho dễ xem.

    @t00m ➜ ~ cat flag.bsv
    ,According,to,all,known, , ,laws,of,aviation,there,is,no, , ,way,a,bee,should,be,able,to,fly., ,Its,wings,are,too,small,to,get,its, ,fat,little,body,off,the,ground.,The,bee, , ,of,course, , , , , ,flies,anyway, ,because,bees,don't,care,what,humans,think,is, ,impossible.,Yellow,black.,Yellow,black.,Yellow,black.,Yellow, , , , , , , , , ,black.,Ooh,black,and,yellow!,Let's,shake,it, , ,up,a, , , , , ,little.,Barry!, ,Breakfast,is,ready!,Coming!,Hang,on,a,second., ,Hello?,Barry?,Adam?,Can,you,believe,this,is, , , , , , , , , ,happening?,I,can't.,I'll,pick,you,up.,Looking, , , ,sharp.,Use,the,stairs,Your,father,paid, , ,good,money,for,those.,Sorry.,I'm,excited.,Here's, , , , ,the,graduate., , , , ,We're,very,proud,of,you,son.,A, , , , , ,perfect,report,card, , , , ,
    , ,all,B's., , ,Very,proud., , , , ,Ma!,I, , , , ,got,a, , , , ,thing,going, , , , , , , ,here.,You, , , , , ,got,lint, ,on,your, , , , , ,fuzz.,Ow!, , , , , , ,That's,me!, , , , , , , ,Wave,to, , , , , , , , , , ,us!,We'll, , , , , ,be,in, ,row,118000., , , , , ,Bye!,Barry, , , , , , ,I,told, , , , , , , ,you,stop, , , , , , , , , , ,flying,in, , , , , ,the,house!, ,Hey,Adam., , , , , ,Hey,Barry., ,Is,that, , , , , , , , ,fuzz,gel?,A,little., , , ,Special,day, , , , , ,graduation.,Never, , , ,thought,I'd, ,make,it., , , ,
    , ,Three,days, , ,grade,school, , , , , , , , , , ,three,days, , , , ,high,school., , , , , , , ,Those,were, , , , , ,awkward.,Three, ,days,college., , , , , ,I'm,glad, , , , , ,I,took, , , , , , , ,a,day, , , , , , , , , , , ,and,hitchhiked, , , , , ,around,The, ,Hive.,You, , , , , ,did,come, , , , , ,back,different., , , , , , , ,Hi,Barry., , , , , , , , , , , ,Artie,growing, , , , , ,a,mustache?, , , , , , , , ,Looks,good., ,Hear,about, , , , , , , , , , ,Frankie?,Yeah., , , , , , , , , , ,You,going, , ,to,the, , , ,funeral?,No, , ,
    , ,I'm,not, , ,going.,Everybody, , , , , , , , , , ,knows,sting, , , , ,someone,you,die.,Don't,waste,it, , , ,on,a,squirrel.,Such,a,hothead.,I,guess, , ,he,could, , , , , ,have,just, , , , ,gotten,out, , , , , , , ,of,the, , , , , , , , , , , , ,way.,I,love,this,incorporating,an,amusement,park, , ,into,our, , , , , ,day.,That's, , , , ,why,we, , , , , , , ,don't,need, , , , , , , , , , , , ,vacations.,Boy,quite,a,bit,of,pomp,under, , , ,the,circumstances.,Well,Adam,today,we,are, , ,men.,We,are!,Bee-men.,Amen!,Hallelujah!, , , , , , ,Students,faculty, , , , ,distinguished,bees,please,welcome,Dean,Buzzwell.,Welcome, , ,New,Hive, , , , , ,City,graduating, ,
    , ,class,of, , ,9:15.,That, , , , , , , , , , ,concludes,our, , , , ,ceremonies,And, , , , , , , ,begins,your, , , , , ,career,at, ,Honex,Industries!, , , , , ,Will,we, , , ,pick,our, , , , , , , ,job,today?, , , , , , , , , , , , , ,I,heard, , , , , ,it's,just, ,orientation.,Heads, , , , , ,up!,Here, , , ,we,go., , , , , , , ,Keep,your, , , , , , , , , , , , , ,hands,and, , , , , ,antennas,inside, ,the,tram, , , , , , , , ,at,all, , , , , , , , , , ,times.,Wonder, , , , , , , , , , ,what,it'll, ,be,like?,A,little,scary.,Welcome,to,Honex,a, ,
    , ,division,of, , ,Honesco,and, , , , ,a,part, , , , ,of,the, , , , ,Hexagon,Group., , , , , , , ,This,is, , , , , ,it!,Wow., ,Wow.,We, , , , , ,know,that, , ,you,as, , , , , , , ,a,bee, , , , , , , , , , , , , , ,have,worked, , , , , ,your,whole, ,life,to, , , , , ,get,to, , ,the,point, , , , , , , ,where,you, , , , , , , , , , , , , , ,can,work, , , , , ,for,your, ,whole,life., , , , , , , , ,Honey,begins, , , , , , , , , , ,when,our, , , ,valiant,Pollen, , , , , ,Jocks,bring, ,the,nectar, , , , , ,to,The, ,
    ,Hive.,Our,top-secret,formula, , ,is,automatically,color-corrected,scent-adjusted,and,bubble-contoured, , , , , ,into,this, , , , ,soothing,sweet, , , , , , , ,syrup,with,its,distinctive,golden,glow,you,know, , , ,as...,Honey!,That,girl,was,hot.,She's, , ,my,cousin!,She,is?,Yes,we're,all,cousins., ,Right.,You're,right.,At,Honex,we,constantly,strive, ,to,improve,every,aspect,of,bee,existence., ,These,bees,are,stress-testing,a,new,helmet,technology., , , ,What,do,you,think,he,makes?,Not, , ,enough.,Here,we,have,our,latest,advancement,the, ,Krelman.,What,does,that,do?,Catches,that,little, ,strand,of,honey,that,hangs,after,you, ,pour,it.,Saves,us,millions.,Can,anyone,work, , ,on,the,Krelman?,Of,course.,Most,bee,jobs,are, ,small,ones., , , , , , , , ,But,bees,know,that,every,small, , ,job,if,it's,done,well,means,a, , ,lot.,But, , , , , ,choose,carefully, ,
   
    
1 đoạn hội thoại trong phim Bee :v để ý đuôi file thì có thể đây là 1 file csv. Mình đổi tên file lại rồi mở lên bằng Excel thử. Thu nhỏ cột lại thì ta sẽ đọc được flag

![image](https://user-images.githubusercontent.com/92283038/179704607-4a295583-1d6b-4013-bb5a-ee9a04437281.png)

`` flag : ictf{buzz_buzz_b2f13a} ``
___

# Web 
## rooCookie
![image](https://user-images.githubusercontent.com/92283038/179705218-dfecf403-7f00-46eb-8af4-07e0a9f579dd.png)

Vì bài tên là cookie nên mình mò vô xem trong cookies trước xem có gì không. Ta thu được 1 cookie có value là :
        
        101100000111011000000110101110011101100000001010111110010101101111101011110111010111001110101001011101001100001011000000010101111101101011111011010011000010100101110101001101001010010111010101111110101011011111011000000110110000001101100001011010111110110110000000101011100101010100101110100110000101011101111010111000110110000010101011101001011000100110101110110101001111101010111111010101000001101011011011010100010110101110110101011011111010100010110101101101101100001011010110111110101000011101011111001010100010110101101101101100000101010011111010100111110101011011011010111000010101000010101011100101011000101110100110000

![image](https://user-images.githubusercontent.com/92283038/179705517-c730f886-cdeb-42e3-a7e5-ca0e9d043ea7.png)

Kiểm tra source thì mình thấy 1 đoạn script để mã hóa cookie 

```js
function createToken(text) {
	let encrypted = "";
  for (let i = 0; i < text.length; i++) {
		encrypted += ((text[i].charCodeAt(0)-43+1337) >> 0).toString(2)
  }
  document.cookie = encrypted
}
```

Phân tích đoạn code thì ta thấy nó sẽ lấy giá trị ascii của text - 43 + 1337 rồi >> 0 xong chuyển thành binary. Mình chỉ cần đảo ngược quá trình này lại là có thể đọc được 
nội dung text. 

![image](https://user-images.githubusercontent.com/92283038/179706576-df4b992a-9a53-458c-a140-65ea388804db.png)

Mình lấy đoạn này chạy thử để xem, ta thấy được mỗi kí  tự xong khi được encode sẽ được biểu diễn bởi 1 đoạn binary có độ dài là 11. Nên giờ mình sẽ cắt đoạn value cookie
thành các phần bằng nhau có độ dài bằng 11 , đổi thành decimal rồi + 43 - 1337 là sẽ lấy được giá trị gốc. ( >> 0 thực ra không ảnh hưởng đến kết quả nên mình bỏ qua ) 

```py


token = "101100000111011000000110101110011101100000001010111110010101101111101011110111010111001110101001011101001100001011000000010101111101101011111011010011000010100101110101001101001010010111010101111110101011011111011000000110110000001101100001011010111110110110000000101011100101010100101110100110000101011101111010111000110110000010101011101001011000100110101110110101001111101010111111010101000001101011011011010100010110101110110101011011111010100010110101101101101100001011010110111110101000011101011111001010100010110101101101101100000101010011111010100111110101011011011010111000010101000010101011100101011000101110100110000"
result = []

for i in range(0,len(token),11):
    ans = int(token[i:i+11],2)
    if ans != None :
        ans = ans + 43 - 1337
        result.append(ans)
for i in result:
    print(chr(i),end="")
```

`` ╰─ﬀ python -u "e:\CyberSec\ICTF\tempCodeRunnerFile.py"
username="roo" & password="ictf{h0p3_7ha7_wa5n7_t00_b4d}" `` 

## button
![image](https://user-images.githubusercontent.com/92283038/179707432-5a4ba408-5535-48d8-abbe-4258953087fc.png)

Mở web lên thì chúng ta sẽ thấy 1 trang web trắng như Ngọc Trinh. Vì bài tên button nên mình ấn thử linh tinh thì 1 popup hiện lên

![image](https://user-images.githubusercontent.com/92283038/179707619-122a72ec-a3a7-4b1d-9edc-be13f6ac542b.png)

Kiểm tra source thì thấy trang web này chứa hơn 15 ngàn cái button 

![image](https://user-images.githubusercontent.com/92283038/179707802-2aa8faf3-45f4-429f-9e37-a4f81519f879.png)

Mình đoán là trong số này sẽ có 1 cái khác biệt nên down về grep thử 
```

@t00m ➜ ~ cat button.txt | grep -v notSusFunction

<style>
.not-sus-button {
  border: none;
  padding: 0;
  background: none;
  color:white;
}
</style>

<button class="not-sus-button" onclick="motSusfunclion()">.</button>
```

À há, chúng ta có 1 function tên là **motSusfunclion**. Mình sửa lại CSS rồi tìm cái nút này và ấn vô thôi

![image](https://user-images.githubusercontent.com/92283038/179708623-ff888162-913e-4917-aa19-b8a7db2c41f7.png)

`` flag : ictf{y0u_f0und_7h3_f1ag!} ``
___



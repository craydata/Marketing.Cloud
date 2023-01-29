# AMPscript Content Blocks for Advanced Content
## IF-THEN UPSERT Example
```
%%[
set @lookupvalue = AttributeValue("CategoryAffinity")
set @affinity = Lookup("ProductRecommendations", "Category", "Category", @lookupvalue)
set @gender = AttributeValue("Gender")
set @subkey = AttributeValue("SubscriberKey")
set @subAffinity = IIF(@affinity == "Shoes", "Yes", "No")
set @output = concat(@subAffinity, @gender)
set @imageID1 = "https://image.s4.sfmc-content.com/lib/fe2b11727664047c7d1d78/m/1/544683df-9f8f-4ece-a303-3afb594388f6.jpg"
set @imageID2 = "https://image.s4.sfmc-content.com/lib/fe2b11727664047c7d1d78/m/1/137453a1-457e-4d68-ab08-568ab7624c65.jpg"
set @imageID3 = "https://image.s4.sfmc-content.com/lib/fe2b11727664047c7d1d78/m/1/ebd833d5-aa81-4b4c-9f21-49cd20a305f0.jpg"
set @updatedDate = Now(1) if (@output == "YesFemale") THEN set @ProductRec = @imageID1 elseif (@output == "NoFemale") THEN set @ProductRec = @imageID2 elseif (@output == "YesMale" OR @output =="NoMale") THEN set @ProductRec = @imageID3 endif
]%%
%%[UpsertDE("MasterDemographics",1,"SubscriberKey", @subkey, "DefaultImage", @ProductRec, "UpdatedDate", @updatedDate)]%%
```

##Randomized Recommendations (2 FOR loops)
```
<table cellpadding="0" cells Lpacing="0" width="100%" style="min-width: 100%; " class="stylingblock-content-wrapper"><tr><td class="stylingblock-content-wrapper camarker-inner"><!-- Start Smart Block --><style type="text/css">
.amp {
 font-size:1px;
 color:#fff;
}</style><span class="amp"></span><style type="text/css">
.code_none{
 display:none;
}</style><div align="center" style="padding: 15px; border: 5px dashed #ffffff; color: #ffffff; font-family: Gotham, Helvetica, Arial, sans-serif; font-size: 18px; background-color: #6ccdf0;">
Recommended Items</div> <span class="amp"></span> <!-- End Smart Block -->
<table style="width: 100%;border-collapse: collapse;" >
<tr>
%%[
set @rows = LookupRows("ProductRecommendations", "Category", "Shoes")
set @rowCount = rowcount(@rows)
set @rowCount2 = random(0,1)
if @rowCount2 > 0 then

 set @num1 = 1
 set @num2 = @rowCount

 /*Change this to retrieve the number of reccos you want to show*/
 for @i = 1 to 3 do
   /*Get a random row*/
   set @random  = random(@num1, @num2)
   set @row = row(@rows, @random)
   set @title = field(@row,"Title")
   set @link = field(@row,"Link")
   set @imgURL = field(@row,"ImgURL")
   set @linkURL = field(@row,"LinkURL")


]%%

 <td>
   <b>%%=v(@title)=%%</b><br>
   <img style="width:150px;" src='%%=v(@imgURL)=%%'><br>
   <a href='%%=v(@linkURL)=%%'>Buy Here</a></td>

   %%[

 next @i ]%%
</tr></table>
%%[ else ]%%

%%[
set @rows = LookupRows("ProductRecommendations2", "Category", "Shoes")
set @rowCount = rowcount(@rows)
 set @num1 = 1
 set @num2 = @rowCount

 /*Change this to retrieve the number of reccos you want to show*/
 for @i = 1 to 3 do
   /*Get a random row*/
   set @random  = random(@num1, @num2)
   set @row = row(@rows, @random)
   set @title = field(@row,"Title")
   set @link = field(@row,"Link")
   set @imgURL = field(@row,"ImgURL")
   set @linkURL = field(@row,"LinkURL")

]%%

 <td>
   <b>%%=v(@title)=%%</b><br>
   <img style="width:150px;" src='%%=v(@imgURL)=%%'><br>
   <a href='%%=v(@linkURL)=%%'>Buy Here</a></td>

   %%[

 next @i ]%%

%%[ endif ]%%
```

## ContentBlockBy Key for Footer
```
%%[var @footer set @footer = "8631b69b-2768-4c81-9362-7077803daef3"]%%
%%=ContentBlockByKey(@footer)=%%
```

## ContentBlockBy Key for Text and Button
```
%%[var @header set @header = "65b60d01-27fb-48c1-81f5-0884bd06565f"]%%
%%=ContentBlockByKey(@header)=%%
```


## Randomized Recommendations (1 For Loop)
```
<table cellpadding="0" cellspacing="0" width="100%" style="min-width: 100%; " class="stylingblock-content-wrapper"><tr><td class="stylingblock-content-wrapper camarker-inner"><!-- Start Smart Block --><style type="text/css">
.amp {
 font-size:1px;
 color:#fff;
}</style><span class="amp"></span><style type="text/css">
.code_none{
 display:none;
}</style><div align="center" style="padding: 15px; border: 5px dashed #ffffff; color: #ffffff; font-family: Gotham, Helvetica, Arial, sans-serif; font-size: 18px; background-color: #6ccdf0;">
Recommended Items</div> <span class="amp"></span> <!-- End Smart Block -->
<table style="width: 100%;border-collapse: collapse;" >
<tr>
%%[

var @random, @num1, @num2
var @rows, @row, @rowCount, @title, @link, @imgsrc, @linkhref


set @rows = LookupRows("ProductRecommendations", "Category", "Shoes")
set @rowCount = rowcount(@rows)

if @rowCount > 0 then

 set @num1 = 1
 set @num2 = @rowCount

 /*Change this to retrieve the number of reccos you want to show*/
 for @i = 1 to 3 do
   /*Get a random row*/
   set @random  = random(@num1, @num2)
   set @row = row(@rows, @random)
   set @title = field(@row,"Title")
   set @link = field(@row,"Link")
   set @imgURL = field(@row,"ImgURL")
   set @linkURL = field(@row,"LinkURL")
]%%
 <td>
   <b>%%=v(@title)=%%</b><br>
   <img style="width:150px;" src='%%=v(@imgURL)=%%'><br>
   <a href='%%=v(@linkURL)=%%'>%%=v(@title)=%%</a></td>

   %%[

 next @i ]%%
</tr></table>
%%[ else ]%%

No rows found

%%[ endif ]%%
```

## Dynamic Impression Region Tracking
```
<table cellpadding="0" cellspacing="0" width="100%" style="min-width: 100%; " class="stylingblock-content-wrapper"><tr><td class="stylingblock-content-wrapper camarker-inner"><!-- Start Smart Block --><style type="text/css">
.amp {
 font-size:1px;
 color:#fff;
}</style><span class="amp"></span><style type="text/css">
.code_none{
 display:none;
}</style><div align="center" style="padding: 15px; border: 5px dashed #ffffff; color: #ffffff; font-family: Gotham, Helvetica, Arial, sans-serif; font-size: 18px; background-color: #6ccdf0;">
More Recommended Items</div> <span class="amp"></span> <!-- End Smart Block -->
<table style="width: 100%;border-collapse: collapse;" >
<tr>
%%[

set @rows2 = LookupRows("ProductRecommendations2", "Category", "Shoes")
set @rowCount2 = rowcount(@rows2)

if @rowCount2 > 0 then

 set @num3 = 1
 set @num4 = @rowCount2

 /*Change this to retrieve the number of reccos you want to show*/
 for @i = 1 to 3 do
   /*Get a random row*/
   set @random2  = random(@num3, @num4)
   set @row2 = row(@rows2, @random2)
   set @title2 = field(@row2,"Title")
   set @link2 = field(@row2,"Link")
   set @imgURL2 = field(@row2,"ImgURL")
   set @linkURL2 = field(@row2,"LinkURL")
]%%
 %%=TreatAsContentArea(@title2, Concat('%','%=BeginImpressionRegion("',@title2,'")=%','%'))=%%
 <td>
   <b>%%=v(@title2)=%%</b><br>
   <img style="width:150px;" src='%%=v(@imgURL2)=%%'><br>
   <a href='%%=v(@linkURL2)=%%'>%%=v(@title2)=%%</a></td>

   %%[

 next @i ]%%
</tr></table>
%%[ else ]%%

No rows found

%%[ endif ]%%

%%=EndImpressionRegion(0)=%%
```





# Tokenization

The low-level tokenization of the Armenian UD treebank follows the tokenization of the
[ՀայՇտեմ - Armenian Dependency Treebank 1.0](http://armtreebank.yerevann.com/) (ArmDT):

* In general, tokens are delimited by whitespace.
* Punctuation (recognized by the corresponding Unicode property) that is conventionally written adjacent to the preceding or following word is separated during tokenization.
  Some special cases worth mentioning:
  * An abbreviation marked by a period, as in *թ.* “year”, becomes two tokens, *<b>թ</b>* and *<b>.</b>* .
  * A compound containing a hyphen becomes three tokens (two words and the hyphen), as in *անգլո-ամերիկյան* “anglo-american”, *պատմա-բանասիրական* “historical-philological”.
    In these cases, the first token is a special form of adjective that never occurs independently.
    Compounds without a hyphen are not split, thus _ռազմածովային_ “navy” is one token but _հասարակական-քաղաքական_ “civic-social” would be three tokens.
    Another common case of splitting-on-hyphen are reduplicative or echo words as in _մեծ-մեծ_ “very big”, _շուն-մուն_ “dog or something”.
  * The words that contain “infixed” punctuation (question, exclamation, emphasis and Armenian abbrevation marks), as in *ինչո՞ւ* “why?”, are considered as multi-word token and become two tokens, *<b>ինչու</b>* and *<b>՞</b>* . EXEPTION is the apostrophe, as in *Ժաննա դ՚Արկ* “Joan of Arc”, which is split and belongs to the preceding part, *<b>Ժաննա</b>* , *<b>դ՚</b>* , *<b>Արկ</b>*.
  * Numerical expressions (including dates) are treated as single words as long as they do not contain spaces, for example, *<b>355,089.40</b>* , *<b>01.01.1970</b>* , *<b>01/01/1970</b>* , *<b>11:00</b>* , *<b>11.00</b>* , *<b>1937-1938</b>* , *<b>10-15</b>* . Decimal numbers also kept as one token, e.g. *<b>2.1</b>* or *<b>2,1</b>* , *<b>0.5-1.5</b>* .
  * Numerical expressions with hyphen and Armenian endings (e.g. { *<b>1-ին</b>* } “1st”, { *<b>3-ով</b>* } “3.Ins”) as well as adjectives and other non-numerals which contain digits (e.g. { *<b>1000-ական</b>* } “by 1000”, { *<b>1950-ականներին</b>* } “in 1950s”, { *<b>20-ամյակ</b>* } “20th anniversary”, { *<b>1700-ամյա</b>* } “1700-year-old”, { *<b>ՆԱՏՕ-ական</b>* } “belonging-to-NATO, *<b>ՏՈՒ-154Մ</b>* “TU-154M”) are treated as single tokens.
  * Most of the time, every punctuation character constitutes a token of its own. Thus *»,—* will become three tokens.
  * Exceptions are conventional multi-character punctuation marks: *<b>...</b>* , *<b>....</b>* , and emojis and smileys: *<b>:)</b>* , *<b>^_^</b>*, etc.
  Conventional non-armenian multi-character punctuation marks and terms are tokenized as single tokens: *<b>?!</b>* .
* Special symbols before and after numerical expressions, as in *$250* , *4,81%* , *+32°С* , are tokenised separately (so, the tokens are { *<b>$</b>* , *<b>250</b>* } , { <b>4,81</b> , <b>%</b> } , { <b>+</b> , <b>32</b> , <b>°С</b> }).
* Email addresses, URLs, and tweet-style names are treated as single tokens: {muster@muster.am}, {https://github.com}, {@anna_il}

### Multi-word tokens

See above, the “infixed” punctuation.

### Pronouns and adverbs

* Indefinite pronouns and adverbs like *<b>ինչ-որ*</b> , *<b>փոքր-ինչ</b>* , *<b>դույզն-ինչ</b>* , *<b>ինչ-ինչ</b>* “something, somewhat”, etc. are treated as a single token.

### Verb forms, analytical grammatical forms, negation

* the forms of necessitative mood, analytical causative, complex tenses, complex comparatives, etc. are splitted
according to the orthographic principle: { *<b>պիտի</b>* , *<b>վազեն</b>* } “they must run”, { *<b>գրել</b>* , *<b>տվեց</b>* } “made write”, { *<b>վազում</b>* , *<b>եմ</b>* } “I run”, { *<b>ավելի</b>* , *<b>լուրջ</b>* } “more serious”.
* *<b>մի</b>* and *<b>ոչ</b>* used as negation markers with verbs, adjectives, pronouns and other words are tokenized according to the orthographic rules: { *<b>մի</b>* , *<b>գրիր</b>* } “don't write”, { *<b>ոչ</b>* , *<b>պաշտոնական</b>* } “unofficial”, { *<b>ոչ</b>* , *<b>մի</b>* , *<b>կերպ</b>* } “in no way”.

# Sentence splitting

Each sentence contains only one root.
Splitting is usually performed after an end-of-sentence full stop or after a dot, ellipsis or colon when these punctuation marks separate unrelated subparts of a sentence. Items in a list may sometimes be rendered as separate sentences.

# [ՀայՆիշ](http://armtreebank.yerevann.com/tokenization/process/)

## Տեքստի հատույթավորում և բառանիշավորում

Բնական լեզվով տեքստերի ավտոմատ մշակման համակարգերի մեծ մասը ենթադրում է, որ տեքստերն ի սկզբանե մասնատված են նախադասությունների և բառերի, այսինքն՝ դրանք արդեն իսկ հատույթավորված են։ Հատույթավորումն, այսպիսով, տեքստի մշակման առաջնային փուլն է և սկզբունքային՝ այսպիսի համակարգերի առջև դրվող խնդիրները լուծելու համար։

Տեքստի հատույթավորումն ապահովվում է գրանշանային վերլուծության միջոցով, որը հիմնվում է տվյալ լեզվին բնորոշ ուղղագրական գրեթե բոլոր առանձնահատկությունները ներառող, շատ թե քիչ մանրամասն ու հստակ կանոնների վրա։ Ենթադրվում է, որ այդպիսի կանոնները հաշվի են առնում ստանդարտ (արդեն իսկ կետադրված) տեքստերում հանդիպող իրական դեպքերը։ Գրանշանային ամենապարզ վերլուծությունը տեքստի՝ բացատներով մասնատումն է՝ բացատները չհամարելով բառանիշներ, և մասնատված հաջորդականությունների խմբավորումը վերջավոր այնպիսի դասերում, որոնք տառեր չեն պարունակելու (կետադրական նշաններ, թվանշաններ և այլն)։ Վիճելի կամ ոչ միանշանակ դեպքերում (մասնավորեցնող կանոններ) այսպիսի համակարգերը միշտ կանգնում են խնդրի առջև՝ հաջորդականությունը գնահատել մե՞կ, թե մի քանի բառանիշ։

Գրանշանային վերլուծությունը «ՀայՇտեմ»-ում ևս ենթադրում է մուտքային տեքստի (որպես UNICODE նշանների հաջորդականություն) մասնատում լեզվաբանական հետագա վերլուծության համար անհրաժեշտ հատույթների՝ նախադասությունների և բառանիշների (token)։ Բառանիշները, որոնք բառաձևեր (կամ բառաձևերի հաջորդականություններ) են, ենթակա են ձևաբանական պիտակավորման։ Բառաձևեր չգնահատվող բառանիշները պիտակավորվում են որպես համապատասխան էությունների դասեր՝ կետադրական նշաններ, թվանշաններ, տառա-թվանշանային արտահայտություններ, կայքէջերի հասցեներ և այլն։ Նախադասությունները ենթակա են ծանոթագրման շարահյուսական կախվածությունների ծառերի տեսքով։

#### «ՀայՇտեմ»-ի գրանշանային վերլուծության մոդուլը՝ «ՀայՆիշ»-ը, հենվում է հետևյալ կանոնների վրա.

* Բացատները միշտ բաժանում են բառանիշներ, ինչը ենթադրում է, որ չեն կարող լինել բացատներ պարունակող բառանիշներ։
* Կետադրությունը նույնպես ենթակա է բառանիշավորման։
  * Համառոտագրություններում կրճատման նշան հանդիսացող կետը չի համարվում բառանիշի մաս և մասնատվում է որպես անջատ բառանիշ, օր.՝ *ֆր.* բառանիշավորվում է որպես { ֆր } և { . } ։
  * «Ոչ գծային» կետադրական նշաններ (պարույկ, շեշտ, բացականչական նշան, պատիվ ՞ ՜ ՛ ՟ ) պարունակող հաջորդականությունները մասնատվում են որպես բազմաբառ բառանիշներ (multiword tokens), այսպես՝ *ինչո՞ւ* բառաձևում կունենաք երկու բառանիշ { ինչու } և { ՞ }։ ԲԱՑԱՌՈՒԹՅՈՒՆ է կազմում ապաթարցը ( ՚ ), որը կցվում են նախորդող հաջորդականությանը որպես մեկ բառանիշ, օր.՝ *Ժաննա դ՚Արկ*-ը հատույթավորվելու է որպես երեք բառանիշ՝ { Ժաննա } { դ՚ } { Արկ }։
  * Միմյանց հաջորդող կետադրական նշանները՝ *»,—* առանձնացվում են որպես երեք անջատ բառանիշներ { » } { , } { — }։ Միևնույն ժամանակ, կախման կետերը { ... } և բազմակետերը { .... } ներկայացվում են որպես մեկ կետադրական նշան, հետևաբար՝ մեկ բառանիշ, ոչ թե երեք կամ չորս առանձին կետերից բաղկացած արտահայտություն։ Բացառություն են կազմում մի քանի նշաններից բաղկացած emoji և smiley { :) }, { ։Ճ } և նման տիպի հաջորդականությունները, որոնք դիտարկվում են որպես մեկ բառանիշ։ Այլալեզու բազմանիշ կետադրական նշանները { ?! } դիտարկվում են որպես մեկ բառանիշ։
  * Չակերտներում վերցված արտահայտություններին միության գծիկով հաջորդող երկրորդական ձևույթները (թեքույթ) առանձացվում են որպես անջատ բառանիշ, օր.՝ *«Երկիր Նայիրի»-ից* դեպքում կունենաք վեց առանձին բառանիշ { « } { Երկիր } { Նայիրի } { » } { - } { ից }, կամ՝ *1950 թ.-ին* դեպքում՝ հինգ բառանիշ { 1950 } { թ } { . } { - } { ին }։
  * Թվանշա-տառային արտահայտությունները, որոնցում թվանշանին հաջորդում է միության գծիկ (ենթամնա) ու հայերեն տառեր, դիտարկվում են որպես մեկ միասնական բառանիշ, լինեն դրանք թվականներ, ածականներ կամ այլ տիպի արտահայտություններ, օր.՝ { 1-ին }, { 2-րդ }, { 4-ը }, { 1000-ական }, { 1950-ականներին }, { 20-ամյակ }, { 1700-ամյա }, { ՏՈՒ-154Մ } ։ Նույն կերպ են բառանիշավորվում հապավումներին միության գծիկով հաջորդող վերջածանցները կամ թեքույթները՝ { ՀՀԿ-ականներից }։
  * Թվանշանները { 265,459.00 }, նաև այնպիսիք, որոնք պարունակում են ստորակետ կամ միջակետ՝ որպես ամբողջի և մասի բաժանարար { 2,15 }, { 2.15 }, կոտորակները { 2/3 }, մաթեմատիկական կամ քիմիական բանաձևերը, էլ. փոստի { muster@muster.am }, կայքէջերի հասցեները { https://github.com }, թվիթների տիպի անունները { @gov_am } դիտարկվում են որպես մեկ ամբողջական բառանիշ։ Այսպիսի արտահայտությունների կառուցվածքի վերլուծությունն իմաստ չունի, կարևոր է միայն այս բառանիշների դասը։
  * Ժամանակ ցույց տվող թվանշանային արտահայտությունները՝ *ժ. 20:55* կամ *ժ. 20.55* մնում են որպես մեկ միասնական բառանիշ՝ { 20:55 }, { 20.55 }։ Նույն կերպ են վերլուծվում նաև ամիս-ամսաթվեր, տարեթվեր ցույց տվող թվանշանային արտահայտությունները { 24.04.1965 }, {24/04/1965 }։
  * Թվանշաններին նախորդող կամ հաջորդող հատուկ նշանները (ներառյալ՝ *+* կամ *-*), չափման միավորները, եզրույթներ արտահայտող նշանները (*կմ, $, %* և այլն) առանձնացվում են որպես անջատ բառանիշներ, եթե անգամ դրանց միջև բացատ չկա, օր.՝ { € } {200 }, {18,25 } { % } , { + } { 35 } { °С } , { 135 } { կմ/ժ }։
* Վերոնշյալ կանոններով չնախատեսվող բոլոր դեպքերը (գծիկով կամ անջատ գրվող վերլուծական բառերը, մասնավորապես՝ հարադրությունները, հարադրական բարդությունները) բառանիշավորվում են «ուղղագրական կանոններին» համաձայն և վերլուծվում են որպես «բազմաբառ արտահայտություններ» (MWE, multiword expressinons)։

#### Նախադասությունների սահմանազատում

* Նախադասության սահմանն անցնում է նախադասության ավարտը նշող կետադրական տերմինալ նշանից (վերջակետ { ։ }, կախման կետեր { ... }, բազմակետ { .... }) հետո՝ հաջորդող մեծատառից կամ պարբերության ավարտից առաջ։
Ընդ որում, բոլոր այն հաջորդականությունները, որոնք սկսվում են մեծատառով և չեն ավարտվում որպես տերմինալ գնահատված նշաններից ոչ մեկով (վերնագիր, կոչ, պարբերականի, ստեղծագործության անվանում, թվարկումներ և այլն), նույնպես գնահատվում են որպես առանձին նախադասություն. դրանց սահմանը նույնպես անցնում է հաջորդող մեծատառից կամ պարբերության ավարտից առաջ։
* Հեղինակային խոսքը, լինի նախադաս, միջադաս, թե հետադաս, չի առանձնացվում ուրիշի ուղղակի, մեջբերվող խոսքից։
Հեղինակային ու մեջբերվող խոսքը վերլուծության հաջորդ փուլերում ծանոթագրվում են շարահյուսական հատուկ տիպի կապի միջոցով (parataxis), որն էլ ապահովում է տեքստի իմաստային ավարտունությունը (| — նշում է նախադասության սահմանը, || — նշում է այդպիսի հատվածների ենթադրյալ սահմանները)։
  * Օր.՝
  *— Ի՞նչը՝ ո՞նց թե...| Տնաշեն, ամբողջ աշխարհն է խոսում. Տանուտերանց Գևորգը երկու օր է մեռած, տղերքից ոչ մեկը չկա, որ հորը թաղի։| Շորերը հագցրել, պառկեցրել են դագաղում ու սպասում են...||
  Գառնիկի աչքերը սառել էին Վարանցովի բերանին։|
  — Մարդ աստծո,— շշնջաց,— բարլուսախառն միանգամից էդքան վատ բան բերանդ ո՞նց զորեց ասել։||*
  Կամ
  *«Վրաս ցեխ ու ճիմ լցրու»,— իր վրա գոռում է։|| Ինքը շշկլած կանգնել է ու չգիտի ի՞նչ անի, ո՞նց վարվի։| Հետո լեզուն բացվեց. «Ախր, այ Գևորգ,— ասաց,— հագինդ նախագահի նվիրած կոստյումն է»։ || «Տո, նախագահի մերն էլ, քո մերն էլ,— համբերությունը հատած գոռաց,— դու ցեխը լցրու, ցեխը...»։||*
* Չակերտներում մեջբերվող ուղղակի խոսքը նախադասությունների սահմանազատվում է ըստ հիմնական կանոնի՝ տերմինալ նշան՝ հաջորդող մեծատառից կամ պարբերության ավարտից առաջ։ Ընդ որում, հնարավոր են դեպքեր, երբ բացող չակերտը մի նախադասության մեջ է, իսկ փակողը հաջորդում։ Այսպիսի սահմանազատումը թույլ է տալիս պահպանել շարահյուսորեն ավարտուն կառույցները։
  * Օր.՝ *Վեպի մասին «Հայ վեպի պատմություն» գրքի հանձնարարականում գրված է, որ «վեպը արվեստի ինտելեկտուալացման ամենաբարձր աստիճանն է։| Վեպը էպիկական ստեղծագործություն է, որի մեջ պատումը կենտրոնացված է առանձին անհատի ճակատագրի վրա, նրա բնավորության և ինքնագիտակցության ձևավորման և զարգացման վրա»։|*
* Եթե չակերտներում մեջբերվում է անվանում (գեղարվեստական կամ գիտական երկերի վերնագրեր, թերթերի անուններ և այլն), որում առկա են տերմինալ նշաններ, ապա անվանման ներսում սահմաններ չեն դրվում։
  * Օր.՝ *Խաչատուր Աբովյանի «Վերք Հայաստանի։ Ողբ հայրենասեր»-ի վեպը հրատարակվել է 1858թ.։|*

## Usage
```python
#import Tokenizer
from tokenizer import Tokenizer

#write some text
text = 'Ես սիրում եմ python:'

#tokenization
T = Tokenizer(text)
T.segmentation().tokenization()

#print result
print(T)
```

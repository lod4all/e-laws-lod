# 日本の法令LOD

## はじめに
本データセットは、[電子政府の総合窓口](https://www.e-gov.go.jp/)から提供されている[日本の法令データ](https://elaws.e-gov.go.jp/download/lawdownload.html)をRDFに変換したものです。
法律間の参照関係を解析し、法令間のリンクを作成したほか、用語定義の抽出、文章構造の解析などを行い、これらの解析結果もRDFに反映しています。また、概要をつかみやすいように構造化したHTMLとも関連づけています。

本データセットのアプリケーションとしては、用語を探したり、委任規定などの法令参照の逆リンクをたどるなどを含めた法令読解支援アプリが考えられます。
[サンプルアプリケーションの説明資料](https://github.com/lod4all/e-laws-lod/blob/master/apps/documents/LOD%E3%83%81%E3%83%A3%E3%83%AC%E3%83%B3%E3%82%B82019_LOD4ALL.pdf)

今後は、自治体の条例などにも適用することで、地域の課題解決支援や、法令・条例に関するQAチャットボットなどの開発を考えています。

## ダウンロード
以下のURLよりダウンロードください。

http://lod4all.net/law/rdf/e-laws-lod.tar.gz

## ライセンス
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.ja)

## 問い合わせ
本データセットについての問い合わせは、Issueもしくは、以下のメールアドレスにお願いします。

株式会社 富士通研究所
lod4all-contact [at] ml.labs.fujitsu.com ( [at] を@に置き換えてください。)

## 構造表示の例
[仮登記担保契約に関する法律](https://elaws.e-gov.go.jp/search/elawsSearch/elaws_search/lsg0500/detail?lawId=353AC0000000078)は以下のHTMLのように解析されます。

- http://lod4all.net/law/page/353AC0000000078/article_1.html
- http://lod4all.net/law/page/353AC0000000078/article_2.html
- http://lod4all.net/law/page/353AC0000000078/article_3.html
- http://lod4all.net/law/page/353AC0000000078/article_4.html
- http://lod4all.net/law/page/353AC0000000078/article_5.html
- http://lod4all.net/law/page/353AC0000000078/article_6.html
- http://lod4all.net/law/page/353AC0000000078/article_7.html
- http://lod4all.net/law/page/353AC0000000078/article_8.html
- http://lod4all.net/law/page/353AC0000000078/article_9.html
- http://lod4all.net/law/page/353AC0000000078/article_10.html
- http://lod4all.net/law/page/353AC0000000078/article_11.html
- http://lod4all.net/law/page/353AC0000000078/article_12.html
- http://lod4all.net/law/page/353AC0000000078/article_13.html
- http://lod4all.net/law/page/353AC0000000078/article_14.html
- http://lod4all.net/law/page/353AC0000000078/article_15.html
- http://lod4all.net/law/page/353AC0000000078/article_16.html
- http://lod4all.net/law/page/353AC0000000078/article_17.html
- http://lod4all.net/law/page/353AC0000000078/article_18.html
- http://lod4all.net/law/page/353AC0000000078/article_19.html
- http://lod4all.net/law/page/353AC0000000078/article_20.html

## データセット説明

### 概要

- 法令LODは日本の法令間のリンク情報や用語定義を抽出してLinked Data化したものです。
- オリジナルのデータセットはeLawsのデータセットです。( https://elaws.e-gov.go.jp/download/lawdownload.html )
- 今回のバージョンは、2019.9.30までにeLawsにアップロードされたデータを反映しています。
- 略称法令名はeLaws の https://elaws.e-gov.go.jp/search/html/all.html を反映しています。
-  廃止法令は、国会図書館の廃止法令検索、eGov廃止法令一覧がありますが、データとしての取得が困難なので、現在は eLawsの中に登録されているものと、https://www.ron.gr.jp/law/law_hais.htm にある廃止された法令の一覧より作成しています（ただし、このサイトは2011.11以降メインテナンスされていません）。

### 名前空間

```turtle
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix time: <http://www.w3.org/2006/time#>

prefix law: <http://lod4all.net/law/resource/>
prefix lawp: <http://lod4all.net/law/property/>
prefix lawo: <http://lod4all.net/law/ontology/>
prefix lawhtml: <http://lod4all.net/law/page/>

prefix oa: <http://www.w3.org/ns/oa#>
prefix doco: <http://purl.org/spar/doco/>
prefix cnt: <http://www.w3.org/2011/content#> 
```

- law: は法令LODのエンティティを定義する
- lawp: は法令LODのプロパティを定義する
- lawo: は法令LODで使用する概念を定義する
- lawhtml: は法令の構造解析結果のHTMLを定義する 
- oa: リンク、用語定義などをアノテーション形式で定義する
- doco:、cnt: 法令文を定義する

### 基本概念体系 
#### 法令(lawo:Law): 法令
- IRI: law:法令ID。eLawsの法令IDがないときは、法令番号
- lawp:lawNum　法令番号
- lawp:lawTitle 法令名称。rdfs:labelにも設定
- lawp:altLabelに略称（複数あり得る）
- lawp:lawID eLawsのID
- lawp:del_flg 廃止法令なら１、有効法令なら0
- lawp:year 年号のURI
- lawp:lawTpe 法令種別(lawo:LawType)
- lawp:detailedLawType 詳細法令種別（リテラル）
- lawp:rationale 根拠法令（施行規則に対しては施行令、施行令に対しては法律）
- rdfs:seeAlso DBpedia


#### 条項号(lawo:Clause)：条、項、号のいずれか
- IRI: law:法令ID_条_項_号（必要なところまで）。号レベルまで。枝番はハイフンで示す（例：70条の4 → 70-4）。附則は条の前にspを付与（例：附則２条 sp2）
- lawp:clauseTypeで、条(lawo:Article）、項（lawo:Paragraph）、号（lawo:Item）のいずれかを設定する
- lawp:lawでその条項号が属する法令（lawo:Law)を設定
- lawp:upperで直接上位の条、項ないし法令を設定
- lawp:prev で条項号同レベルの前の条項号を設定
- lawp:htmlでこの条項号を含む条単位の解析済みhtmlのURI
- lawp:article, lawp:paragraph, lawp:item に条、項、号の番号（"2", "2-2"などの文字列）を持つ
-  lawp:index 条項号の順序を示すための整数値

#### 法令文(doco:Sentence)
- cnt:chars に文の文字列、lawp:clauseにその文が属している条項号(lawo:Clause)、lawp:lawにその文が属している法令(lawo:Law)を持つ。doco:indexに条項号の中の文番号を与える。

#### 法令参照(lawo:ReferLaw, oa:Annotation)
- oa:hasTargetの先に参照元の法令文字列参照、oa:hasBodyの先に参照先の法令(lawo:Law)を持つ
- oa:motivatedBy　は oa:linking

#### 委任規定(lawo:Delegation, oa:Annotation)
- oa:hasTargetの先に参照元の委任規定、oa:hasBodyの先に委任先の法令(lawo:Law)を持つ
- oa:motivatedBy　は oa:linking

#### 法令文字列参照(oa:SpecificResource)
- oa:hasSourceの先に法令文(doco:Sentence)、oa:hasSelectorの先にoa:startとoa:endで文字列中の位置を示す
- 値付き法令文字列参照：法令文字列参照のoa:hasSelectorの先に、オフセットだけでなく、具体的な文字列を rdf:value　として持たせることがある（検索などのため）

#### 定義規定(lawo:Definition, oa:Annotation)
- oa:motivatedBy は oa:lidentifying
- oa:hasTargetの先に定義規定全体の法令文字列参照
- oa:hasBodyの先に定義規定の各種情報。lawp:termに定義語文字列、lawp:bodyに定義規定本文、lawp:scopeに定義規定本文から定義語の有効範囲を示す文字列

#### 定義語参照(lawo:ReferTerm, oa:Annotation)
- oa:motivatedBy は oa:lidentifying
- - oa:hasTargetの先に定義語を使用している部分の法令文字列参照
- oa:hasBodyの先に定義規定(lawo:Definition）のURI

#### 文構造情報(lawo:Struct, oa:Annotation)
- oa:motivatedByはoa:taggingを設定
- oa:hasTargetの先に法令文字列参照（文字列範囲）
- oa:hasBody に構造タグ

#### 並列構造情報(lawo:Parallel, oa:Annotation)
- oa:motivatedByはoa:describingを設定
- oa:hasTargetの先に並列構造の全体。値付き法令文字列参照
- oa:hasBody の先に並列構造の部分情報として、lawp:firstが最初の並列要素の文字列。lawp:lastが最後の並列要素の文字列。lawp:parallelmarkerが並立接続詞類のURI。

#### その他

- 年号（lawo:Nengo） 
-- time:year で西暦年
- 並立接続詞類(lawo:ParallelMarker)
- 文構造タグ

## SPARQL例
 以下に、取得例のSPARQLを記載します。

### 法令名称から法令情報の取得

```sparql
prefix lawp: <http://lod4all.net/law/property/>
  
select * 
  where {
?s  lawp:lawTitle "租税特別措置法";
     ?p ?o} order by ?s
```
#### 実行結果
| s                                               | title                          | num                              |
|-------------------------------------------------|--------------------------------|----------------------------------|
| http://lod4all.net/law/resource/411AC0000000089 | 内閣府設置法                   | 平成十一年法律第八十九号         |
| http://lod4all.net/law/resource/411AC0000000091 | 総務省設置法                   | 平成十一年法律第九十一号         |
| http://lod4all.net/law/resource/411AC0000000093 | 法務省設置法                   | 平成十一年法律第九十三号         |
| http://lod4all.net/law/resource/411AC0000000094 | 外務省設置法                   | 平成十一年法律第九十四号         |
| http://lod4all.net/law/resource/411AC0000000095 | 財務省設置法                   | 平成十一年法律第九十五号         |
| http://lod4all.net/law/resource/411AC0000000096 | 文部科学省設置法               | 平成十一年法律第九十六号         |
| http://lod4all.net/law/resource/411AC0000000097 | 厚生労働省設置法               | 平成十一年法律第九十七号         |
| http://lod4all.net/law/resource/411AC0000000098 | 農林水産省設置法               | 平成十一年法律第九十八号         |
| http://lod4all.net/law/resource/411AC0000000099 | 経済産業省設置法               | 平成十一年法律第九十九号         |
| http://lod4all.net/law/resource/411AC0000000100 | 国土交通省設置法               | 平成十一年法律第百号             |
| http://lod4all.net/law/resource/411AC0000000101 | 環境省設置法                   | 平成十一年法律第百一号           |
| http://lod4all.net/law/resource/411AC0000000103 | 独立行政法人通則法             | 平成十一年法律第百三号           |
| http://lod4all.net/law/resource/412M50002100006 | 厚生労働省所管補助金等交付規則 | 平成十二年厚生省・労働省令第六号 |

Subject として
http://lod4all.net/law/resource/332AC0000000026とhttp://lod4all.net/law/resource/昭和２１年法律第１５号★　が取得される。後者は　lawp:del_flg 1 により廃止法令であることがわかる。

### 略称名からの検索

- "中央省庁等改革関連法"と呼ばれる法令は？

```sparql
prefix lawp: <http://lod4all.net/law/property/>
  
select * 
  where {
?s  lawp:altLabel "中央省庁等改革関連法";
     lawp:lawTitle ?title;
     lawp:lawNum ?num .
} 
```

### 公布年と法令種別での絞り込み

- 令和元年に公布された法律(lawo:Act）。

```sparql
prefix lawp: <http://lod4all.net/law/property/>
prefix lawo: <http://lod4all.net/law/ontology/>
  
select ?title ?num
  where {
?s  lawp:year lawo:令和1;
lawp:lawTitle ?title;
lawp:lawNum ?num;
lawp:lawType lawo:Act.
} 
```


### 特定の法令のある条項の構造表示HTMLの取得

- 租税特別措置法第70条の４
- 法令名（lawp:lawTitle）と、その法令に属する条項の条番号（lawp:article)を指定する。配下にある項、号もとれてくるので、lawp:clauseTypeを指定して項（lawo:Article)のみに絞る。
- 構造表示用のhtml(lawp:html)を取り出す。
- 構造表示用のhtmlは条単位に存在する。uriをクリックすることで、法令文が構造化されて表示される

```sparql
prefix law: <http://lod4all.net/law/resource/>
prefix lawp: <http://lod4all.net/law/property/>
prefix lawo: <http://lod4all.net/law/ontology/>
prefix cnt: <http://www.w3.org/2011/content#> 
prefix doco: <http://purl.org/spar/doco/>
  
select *
  where {
    values (?uri) {(law:332AC0000000026_70-4)} 
    ?law lawp:lawTitle "租税特別措置法".
    ?article lawp:clauseType lawo:Article;
                  lawp:law ?law;
                  lawp:article "70-4";
                  lawp:html ?html.
 }
```
 
### 特定の法令のある条項の本文の取得

- 租税特別措置法第70条の4の法令本文（法令条項の指定は、ここでは簡易的にURIを利用する）
- 上記の法令条項のURIはlaw:332AC0000000026_70-4
- 各文はlawo:Sentenceに格納されていて、lawo:Sentenceはどの条項に属しているかがある（lawp:clause)
- 各条項は上位条項をたどれる（lawp:upper)
- 条項は項番号(lawp:paragraph)、号番号(lawp:item)を持つが、枝番号もあるので、ソートにはlawp:indxを利用する
- 各条項内の文の順序はdoco:indexの値を利用する

```sparql
prefix law: <http://lod4all.net/law/resource/>
prefix lawp: <http://lod4all.net/law/property/>
prefix cnt: <http://www.w3.org/2011/content#> 
prefix doco: <http://purl.org/spar/doco/>
  
select ?para ?item ?sentence 
  where {
    values (?uri) {(law:332AC0000000026_70-4)}
    ?s a doco:Sentence;
    cnt:chars ?sentence;
    lawp:clause ?clause;
    doco:index ?sentence_no .
    ?clause lawp:upper*  ?uri;
    lawp:index ?index;
    lawp:paragraph ?para;
    lawp:item ?item.
 } ORDER BY ?index ?sentence_no
```

### 特定の法令条項を参照している法令の取得

- 租税特別措置法第70条の4を参照している法令。
- 上記条項のURIは　law:332AC0000000026_70-4_1
- 法令参照(lawo:ReferLaw)はoa:hasTargetが参照元情報、oa:hasBodyが参照先情報。
- 参照先(oa:hasBody)は法令の条項なので law:332AC0000000026_70-4_1を指定する
- 参照元は法令文字列参照(oa:SpecificResource)なので、法令文情報(oa:hasSource)を取り、その法令文が属する法令(lawp:law)のタイトルをとる

```
prefix law: <http://lod4all.net/law/resource/>
prefix lawp: <http://lod4all.net/law/property/>
prefix lawo: <http://lod4all.net/law/ontology/>
prefix oa: <http://www.w3.org/ns/oa#>
  
select distinct ?title 
  where {
    ?s a lawo:ReferLaw;
         oa:hasBody law:332AC0000000026_70-4_1;
         oa:hasTarget ?target.
    ?target oa:hasSource ?sentence .
    ?sentence lawp:law ?law .
    ?law lawp:lawTitle ?title .
 } order by ?law
```

### 法令全体での参照元法令

- 租税特別措置法を参照している法令
- 法令参照の参照先の条項の属する法令(lawp:law)に租税特別措置法（law:332AC0000000026）を指定する
- 参照回数の頻度順にソートする

```sparql
prefix law: <http://lod4all.net/law/resource/>
prefix lawp: <http://lod4all.net/law/property/>
prefix lawo: <http://lod4all.net/law/ontology/>
prefix oa: <http://www.w3.org/ns/oa#>
  
select distinct ?title (count(?title) as ?count)
  where {
    ?s a lawo:ReferLaw;
         oa:hasBody/lawp:law law:332AC0000000026;
         oa:hasTarget ?target.
    ?target oa:hasSource ?sentence .
    ?sentence lawp:law ?law .
    ?law lawp:lawTitle ?title .
 } order by desc(?count)

 ```

### 法令による定義語の違い

- 「内国法人」という用語の定義を各法令で確認
- 用語定義はlawo:Definition。oa:hasBody に定義語情報、oa:hasTargetに定義本体がある
- 文の属する条項の条番号(lawp:article)も示す
- 条番号は枝番もあるので文字列型。ソートするにはlawp:indexを利用する

```sparql
prefix lawp: <http://lod4all.net/law/property/>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix oa: <http://www.w3.org/ns/oa#>
  
select ?law ?article ?body 
  where {
[] oa:hasBody [lawp:term "内国法人"; 
                        lawp:body ?body];
 oa:hasTarget/oa:hasSource ?at .
?at lawp:law/rdfs:label ?law;
       lawp:clause/lawp:article ?article
 } 
```

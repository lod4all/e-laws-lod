# 日本の法令LOD

## はじめに
本データセットは、[電子政府の総合窓口](https://www.e-gov.go.jp/)から提供されている[日本の法令データ](https://elaws.e-gov.go.jp/download/lawdownload.html)をRDFに変換したものです。
法律間の参照関係を解析し、法令間のリンクを作成したほか、用語定義の抽出、文章構造の解析などを行い、これらの解析結果もRDFに反映しています。また、概要をつかみやすいように構造化したHTMLとも関連づけています。

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
[日本の法令LODダッシュボード](http://lod4all.net/frontend/index/applicationtop?app_id=lawKGFrontend)より確認できます。

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
?s  lawp:lawTitle "租税特別措置法"@ja;
     ?p ?o} order by ?s
```
#### 実行結果
| s                                                      | p                                               | o                                             |
|--------------------------------------------------------|-------------------------------------------------|-----------------------------------------------|
| http://lod4all.net/law/resource/332AC0000000026        | http://www.w3.org/1999/02/22-rdf-syntax-ns#type | http://lod4all.net/law/ontology/Law           |
| http://lod4all.net/law/resource/332AC0000000026        | http://www.w3.org/2000/01/rdf-schema#label      | 租税特別措置法                                |
| http://lod4all.net/law/resource/332AC0000000026        | http://www.w3.org/2000/01/rdf-schema#seeAlso    | http://ja.dbpedia.org/resource/租税特別措置法 |
| http://lod4all.net/law/resource/332AC0000000026        | http://lod4all.net/law/property/del_flg         | 0                                             |
| http://lod4all.net/law/resource/332AC0000000026        | http://lod4all.net/law/property/detailedLawType | 法律                                          |
| http://lod4all.net/law/resource/332AC0000000026        | http://lod4all.net/law/property/lawID           | 332AC0000000026                               |
| http://lod4all.net/law/resource/332AC0000000026        | http://lod4all.net/law/property/lawNum          | 昭和三十二年法律第二十六号                    |
| http://lod4all.net/law/resource/332AC0000000026        | http://lod4all.net/law/property/lawTitle        | 租税特別措置法                                |
| http://lod4all.net/law/resource/332AC0000000026        | http://lod4all.net/law/property/lawType         | http://lod4all.net/law/ontology/Act           |
| http://lod4all.net/law/resource/332AC0000000026        | http://lod4all.net/law/property/year            | http://lod4all.net/law/ontology/昭和32        |
| http://lod4all.net/law/resource/332AC0000000026        | http://lod4all.net/law/property/altLabel        | 租特法                                        |
| http://lod4all.net/law/resource/昭和２１年法律第１５号 | http://www.w3.org/1999/02/22-rdf-syntax-ns#type | http://lod4all.net/law/ontology/Law           |
| http://lod4all.net/law/resource/昭和２１年法律第１５号 | http://www.w3.org/2000/01/rdf-schema#label      | 租税特別措置法                                |
| http://lod4all.net/law/resource/昭和２１年法律第１５号 | http://lod4all.net/law/property/del_flg         | 1                                             |
| http://lod4all.net/law/resource/昭和２１年法律第１５号 | http://lod4all.net/law/property/detailedLawType | 法律                                          |
| http://lod4all.net/law/resource/昭和２１年法律第１５号 | http://lod4all.net/law/property/lawID           | 昭和２１年法律第１５号                        |
| http://lod4all.net/law/resource/昭和２１年法律第１５号 | http://lod4all.net/law/property/lawNum          | 昭和２１年法律第１５号                        |
| http://lod4all.net/law/resource/昭和２１年法律第１５号 | http://lod4all.net/law/property/lawTitle        | 租税特別措置法                                |
| http://lod4all.net/law/resource/昭和２１年法律第１５号 | http://lod4all.net/law/property/lawType         | http://lod4all.net/law/ontology/Act           |
| http://lod4all.net/law/resource/昭和２１年法律第１５号 | http://lod4all.net/law/property/year            | http://lod4all.net/law/ontology/昭和21        |

Subject として
http://lod4all.net/law/resource/332AC0000000026とhttp://lod4all.net/law/resource/昭和２１年法律第１５号★　が取得される。後者は　lawp:del_flg 1 により廃止法令であることがわかる。

### 略称名からの検索

- "中央省庁等改革関連法"と呼ばれる法令は？

```sparql
prefix lawp: <http://lod4all.net/law/property/>
  
select * 
  where {
?s  lawp:altLabel "中央省庁等改革関連法"@ja;
     lawp:lawTitle ?title;
     lawp:lawNum ?num .
} 
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

#### 実行結果
| title                                                                                        | num                    |
|----------------------------------------------------------------------------------------------|------------------------|
| 自殺対策の総合的かつ効果的な実施に資するための調査研究及びその成果の活用等の推進に関する法律 | 令和元年法律第三十二号 |
| 日本語教育の推進に関する法律                                                                 | 令和元年法律第四十八号 |

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
    ?law lawp:lawTitle "租税特別措置法"@ja .
    ?article lawp:clauseType lawo:Article;
                  lawp:law ?law;
                  lawp:article "70-4";
                  lawp:html ?html.
 }
```

#### 実行結果
| uri                                                  | law                                             | article                                              | html                                                          |
|------------------------------------------------------|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------------|
| http://lod4all.net/law/resource/332AC0000000026_70-4 | http://lod4all.net/law/resource/332AC0000000026 | http://lod4all.net/law/resource/332AC0000000026_70-4 | http://lod4all.net/law/page/332AC0000000026/article_70-4.html |
 
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

#### 実行結果(先頭20件)
| para | item | sentence                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|------|------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    |      | 農業を営む個人で政令で定める者（以下第七十条の五までにおいて「贈与者」という。）が、その農業の用に供している農地（特定市街化区域農地等に該当するもの及び利用意向調査（農地法第三十二条第一項又は第三十三条第一項の規定による同法第三十二条第一項に規定する利用意向調査をいう。第一号において同じ。）に係るもののうち政令で定めるものを除く。次項を除き、以下第七十条の五までにおいて同じ。）の全部及び当該用に供している採草放牧地（特定市街化区域農地等に該当するものを除く。同項を除き、以下第七十条の五までにおいて同じ。）のうち政令で定める部分並びに当該農地及び採草放牧地とともに農業振興地域の整備に関する法律第八条第二項第一号に規定する農用地区域として定められている区域内にある土地で農地又は採草放牧地に準ずるものとして政令で定めるもの（以下この条において「準農地」という。）のうち政令で定める部分を当該贈与者の推定相続人で政令で定める者のうちの一人の者に贈与した場合（当該贈与者が既にこの条の規定その他これに類するものとして政令で定める規定の適用に係る贈与をしている場合を除く。）には、当該農地及び採草放牧地並びに準農地（以下第七十条の五までにおいて「農地等」という。）の贈与を受けた者（次条第九項各号を除き、以下第七十条の五までにおいて「受贈者」という。）の当該贈与の日の属する年分の相続税法第二十八条第一項の規定による期限内申告書（以下この条において「贈与税の申告書」という。）の提出により納付すべき贈与税の額のうち、当該農地等の価額に対応する部分の金額として政令で定めるところにより計算した金額（以下この条において「納税猶予分の贈与税額」という。）に相当する贈与税については、当該年分の贈与税の申告書の提出期限までに当該納税猶予分の贈与税額に相当する担保を提供した場合に限り、同法第三十三条の規定にかかわらず、当該贈与者の死亡の日まで、その納税を猶予する。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 1    |      | ただし、当該受贈者が、同日前において第一号から第三号までに掲げる場合のいずれかに該当することとなつた場合にはこれらの号に定める日から二月を経過する日（その該当することとなつた後同日以前に当該受贈者が死亡した場合には、当該受贈者の相続人（包括受遺者を含む。以下この条において同じ。）が当該受贈者の死亡による相続の開始があつたことを知つた日の翌日から六月を経過する日）まで、当該贈与者の死亡の日前において第四号に掲げる場合に該当することとなつた場合には同号に定める日まで、それぞれ当該納税を猶予する。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 1    | 1    | 当該贈与により取得したこの項本文の規定の適用を受ける農地等の譲渡、贈与若しくは転用（採草放牧地の農地への転用、準農地の採草放牧地又は農地への転用その他政令で定める転用を除く。）をし、当該農地等につき地上権、永小作権、使用貸借による権利若しくは賃借権の設定（当該農地等につき民法第二百六十九条の二第一項の地上権の設定があつた場合において当該受贈者が当該農地等を耕作（農地法第四十三条第一項の規定により耕作に該当するものとみなされる農作物の栽培を含む。次項第一号を除き、以下この条において同じ。）又は養畜の用に供しているときにおける当該設定を除く。）をし、若しくは当該農地等につき耕作の放棄（農地について農地法第三十六条第一項の規定による勧告（当該農地が農地中間管理事業の推進に関する法律（平成二十五年法律第百一号）第二条第三項に規定する農地中間管理事業の事業実施地域外に所在する場合には、農業委員会その他の政令で定める者が、政令で定めるところにより、当該農地の所在地の所轄税務署長に対し、当該農地が利用意向調査に係るものであつて農地法第三十六条第一項各号に該当する旨の通知をするときにおける当該通知。第十項第二号において同じ。）があつたことをいう。以下この条において同じ。）をし、又は当該取得に係るこの項本文の規定の適用を受けるこれらの権利の消滅（これらの権利に係る農地又は採草放牧地の所有権の取得に伴う消滅を除く。）があつた場合（第三十三条の四第一項に規定する収用交換等による譲渡その他政令で定める譲渡又は設定があつた場合を除く。）において、当該譲渡、贈与、転用、設定若しくは耕作の放棄又は消滅（以下第七十条の五までにおいて「譲渡等」という。）があつた当該農地等に係る土地の面積（当該譲渡等の時前にこの項本文の規定の適用を受ける農地等につき譲渡等（第三十三条の四第一項に規定する収用交換等による譲渡その他政令で定める譲渡又は設定を除く。）があつた場合には、当該譲渡等に係る土地の面積を加算した面積）が、当該受贈者のその時の直前におけるこの項本文の規定の適用を受ける農地等に係る耕作又は養畜の用に供する土地（当該受贈者が当該贈与により取得した農地等のうち準農地で農地又は採草放牧地への転用がされたもの以外のものに係る土地を含む。）の面積（その時前にこの項本文の規定の適用を受ける農地等のうち農地又は採草放牧地につき譲渡等があつた場合には、当該譲渡等に係る土地の面積を加算した面積）の百分の二十を超えるとき　その事実が生じた日 |
| 1    | 2    | 当該贈与により取得した農地等に係る農業経営を廃止した場合　その廃止の日                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 1    | 3    | 当該贈与者の推定相続人に該当しないこととなつた場合　その該当しないこととなつた日                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 1    | 4    | 当該受贈者がこの項の規定の適用を受けることをやめようとする場合において、第三十五項第一号に規定する贈与税及び当該贈与税に係る同項に規定する利子税を納付してその旨を記載した届出書を納税地の所轄税務署長に提出したとき　当該届出書の提出があつた日                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 2    |      | この条から第七十条の六の五までにおいて、次の各号に掲げる用語の意義は、当該各号に定めるところによる。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 2    | 1    | 農地　農地法第二条第一項に規定する農地（同法第四十三条第一項の規定により農作物の栽培を耕作に該当するものとみなして適用する同法第二条第一項に規定する農地並びにこれらの農地の上に存する地上権、永小作権、使用貸借による権利及び賃借権を含む。）をいう。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 2    | 2    | 採草放牧地　農地法第二条第一項に規定する採草放牧地（当該採草放牧地の上に存する地上権、永小作権、使用貸借による権利及び賃借権を含む。）をいう。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 2    | 3    | 特定市街化区域農地等　都市計画法第七条第一項に規定する市街化区域内に所在する農地又は採草放牧地で、平成三年一月一日において次に掲げる区域内に所在するもの（都市営農農地等を除く。）をいう。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 2    | 3    | イ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 2    | 3    | 都の区域（特別区の存する区域に限る。）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 2    | 3    | ロ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 2    | 3    | 首都圏整備法第二条第一項に規定する首都圏、近畿圏整備法第二条第一項に規定する近畿圏又は中部圏開発整備法第二条第一項に規定する中部圏内にある地方自治法第二百五十二条の十九第一項の市の区域                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| 2    | 3    | ハ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 2    | 3    | ロに規定する市以外の市でその区域の全部又は一部が首都圏整備法第二条第三項に規定する既成市街地若しくは同条第四項に規定する近郊整備地帯、近畿圏整備法第二条第三項に規定する既成都市区域若しくは同条第四項に規定する近郊整備区域又は中部圏開発整備法第二条第三項に規定する都市整備区域内にあるものの区域                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 2    | 4    | 都市営農農地等　次に掲げる農地又は採草放牧地で平成三年一月一日において前号イからハまでに掲げる区域内に所在するものをいう。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 2    | 4    | イ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 2    | 4    | 都市計画法第八条第一項第十四号に掲げる生産緑地地区内にある農地又は採草放牧地（生産緑地法第十条（同法第十条の五の規定により読み替えて適用する場合を含む。）又は第十五条第一項の規定による買取りの申出がされたもの並びに同法第十条第一項に規定する申出基準日までに同法第十条の二第一項の特定生産緑地（イにおいて「特定生産緑地」という。）の指定がされなかつたもの、同法第十条の三第二項に規定する指定期限日までに特定生産緑地の指定の期限の延長がされなかつたもの及び同法第十条の六第一項の規定による指定の解除がされたものを除く。）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 2    | 4    | ロ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

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

#### 実行結果
| "title"                                                                |
|------------------------------------------------------------------------|
| "地方税法"                                                             |
| "地方税法施行令"                                                       |
| "農地法施行規則"                                                       |
| "地方税法施行規則"                                                     |
| "租税特別措置法"                                                       |
| "租税特別措置法施行令"                                                 |
| "租税特別措置法施行規則"                                               |
| "東日本大震災の被災者等に係る国税関係法律の臨時特例に関する法律"       |
| "東日本大震災の被災者等に係る国税関係法律の臨時特例に関する法律施行令" |

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
 
 #### 実行結果(上位10件)
 | title                                                                | count |
|----------------------------------------------------------------------|-------|
| 租税特別措置法                                                       | 21920 |
| 租税特別措置法施行令                                                 | 15349 |
| 租税特別措置法施行規則                                               | 6320  |
| 地方税法                                                             | 1023  |
| 東日本大震災の被災者等に係る国税関係法律の臨時特例に関する法律       | 968   |
| 租税特別措置の適用状況の透明化等に関する法律施行規則                 | 834   |
| 地方税法施行令                                                       | 574   |
| 東日本大震災の被災者等に係る国税関係法律の臨時特例に関する法律施行令 | 392   |
| 租税特別措置の適用状況の透明化等に関する法律施行令                   | 340   |
| 阪神・淡路大震災の被災者等に係る国税関係法律の臨時特例に関する法律   | 312   |

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
[] oa:hasBody [lawp:term "内国法人"@ja; 
                        lawp:body ?body];
 oa:hasTarget/oa:hasSource ?at .
?at lawp:law/rdfs:label ?law;
       lawp:clause/lawp:article ?article
 } 
```
#### 実行結果

| law                                                                                                                                | article | body                                                                                                                                                                                                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 地方税法                                                                                                                           | 292     | この法律の施行地に本店又は主たる事務所若しくは事業所を有する法人（以下この節において「内国法人」という。）                                                                                                                                                                                             |
| 地方税法                                                                                                                           | 72-19   | この法律の施行地に主たる事務所又は事業所を有する法人（以下この節において「内国法人」という。）                                                                                                                                                                                                         |
| 地方税法                                                                                                                           | 72-78   | 国内に本店又は主たる事務所を有する法人（次号において「内国法人」という。）                                                                                                                                                                                                                             |
| 地方税法                                                                                                                           | 23      | この法律の施行地に本店又は主たる事務所若しくは事業所を有する法人（以下この節において「内国法人」という。）                                                                                                                                                                                             |
| 地方税法施行令                                                                                                                     | 20-2-7  | 法人税法第六十九条第四項第一号に規定する内部取引において法第七十二条の十九に規定する内国法人（以下この節において「内国法人」という。）                                                                                                                                                                 |
| 租税特別措置法                                                                                                                     | 2       | 内国法人又は外国法人　それぞれ法人税法第二条第三号又は第四号に規定する内国法人又は外国法人をいい、それぞれ第二号に規定する人格のない社団等で、前号に規定する国内に本店若しくは主たる事務所を有するもの又は同号に規定する国外に本店若しくは主たる事務所を有するものを含む。                             |
| 租税特別措置法                                                                                                                     | 2       | 内国法人又は外国法人　それぞれ所得税法第二条第一項第六号又は第七号に規定する内国法人又は外国法人をいい、それぞれ同項第八号に規定する人格のない社団等で、第一号に規定する国内に本店若しくは主たる事務所を有するもの又は同号に規定する国外に本店若しくは主たる事務所を有するものを含む。                 |
| 租税特別措置法施行令                                                                                                               | 26-17   | その割引債の償還金の支払を受ける同条第一項に規定する内国法人（次項及び第八項において「内国法人」という。）                                                                                                                                                                                             |
| 租税特別措置法施行規則                                                                                                             | 19-5    | 同条第六項に規定する内国法人（次項において「内国法人」という。）                                                                                                                                                                                                                                       |
| 外国居住者等の所得に対する相互主義による所得税等の非課税等に関する法律                                                             | 44      | 所得税法第二条第一項第三号に規定する居住者（次条において「日本国の居住者」という。）又は法人税法第二条第三号に規定する内国法人（次条において「内国法人」という。）                                                                                                                                     |
| 外国居住者等の所得に対する相互主義による所得税等の非課税等に関する法律                                                             | 2       | 内国法人又は外国法人　それぞれ法人税法第二条第三号又は第四号に規定する内国法人又は外国法人をいい、それぞれ同条第八号に規定する人格のない社団等（第七条第三項において「人格のない社団等」という。）で、国内に本店若しくは主たる事務所を有するもの又は国外に本店若しくは主たる事務所を有するものを含む。 |
| 所得税法                                                                                                                           | 2       | 内国法人　国内に本店又は主たる事務所を有する法人をいう。                                                                                                                                                                                                                                               |
| 法人税法                                                                                                                           | 2       | 内国法人　国内に本店又は主たる事務所を有する法人をいう。                                                                                                                                                                                                                                               |
| 租税条約等の実施に伴う所得税法、法人税法及び地方税法の特例等に関する法律                                                           | 3-2     | 所得税法第二条第一項第三号に規定する居住者（以下この条において「居住者」という。）又は同項第六号に規定する内国法人（人格のない社団等を含む。以下「内国法人」という。）                                                                                                                                 |
| 消費税法                                                                                                                           | 22      | 国内に本店又は主たる事務所を有する法人（次号において「内国法人」という。）                                                                                                                                                                                                                             |
| 地価税法                                                                                                                           | 12      | 国内に本店又は主たる事務所を有する法人（次号において「内国法人」という。）                                                                                                                                                                                                                             |
| 法人特別税法                                                                                                                       | 2       | 内国法人　法人税法（昭和四十年法律第三十四号）第二条第三号に規定する内国法人をいう。                                                                                                                                                                                                                   |
| 内国税の適正な課税の確保を図るための国外送金等に係る調書の提出等に関する法律施行規則                                               | 1       | 内国法人　法人税法（昭和四十年法律第三十四号）第二条第三号に規定する内国法人をいう。                                                                                                                                                                                                                   |
| 地球温暖化対策の推進に関する法律                                                                                                   | 45      | 国内に本店又は主たる事務所（以下「本店等」という。）を有する法人（以下「内国法人」という。）                                                                                                                                                                                                           |
| 有限責任事業組合契約に関する法律                                                                                                   | 3       | 若しくは現在まで引き続いて一年以上居所を有する個人（第三十七条において「居住者」という。）又は国内に本店若しくは主たる事務所を有する法人（同条において「内国法人」という。）                                                                                                                           |
| 地方法人税法                                                                                                                       | 2       | 内国法人　法人税法（昭和四十年法律第三十四号）第二条第三号に規定する内国法人をいう。                                                                                                                                                                                                                   |
| 東日本大震災からの復興のための施策を実施するために必要な財源の確保に関する特別措置法                                               | 6       | 内国法人　所得税法第二条第一項第六号に規定する内国法人をいう。                                                                                                                                                                                                                                         |
| 東日本大震災からの復興のための施策を実施するために必要な財源の確保に関する特別措置法                                               | 40      | 内国法人　法人税法第二条第三号に規定する内国法人をいう。                                                                                                                                                                                                                                               |
| 租税条約等の実施に伴う所得税法、法人税法及び地方税法の特例等に関する法律の施行に関する省令                                         | 2-5     | 所得税法第二条第一項第三号に規定する居住者（以下「居住者」という。）又は法人税法第二条第三号に規定する内国法人（同条第八号に規定する人格のない社団等を含む。以下「内国法人」という。）                                                                                                                 |
| 湾岸地域における平和回復活動を支援するため平成二年度において緊急に講ずべき財政上の措置に必要な財源の確保に係る臨時措置に関する法律 | 4       | 内国法人　法人税法（昭和四十年法律第三十四号）第二条第三号に規定する内国法人をいう。                                                                                                                                                                                                                   |


## 想定アプリ: QAチャットボットの例

本データセットは、以下のような問いに答えられるチャットボットの知識ベースとして、利活用できると考えています。

- 問：「医療保険業を営む法人が家庭用電気マッサージ器『XXX』を購入したが、免税措置を受けられるか？」
- 答：「受けられます。その根拠は、次の通りです」
  - 租税特別措置法第六十八条の二十九：医療用の機械及び装置並びに器具及び備品（政令で定める規模のものに限る。）
    - 租税特別措置施行令第三十九条の五十八：第二十八条の十第二項各号に掲げる医療用の機械及び装置並びに器具及び備品
    - 医薬品、医療機器等の品質、有効性及び安全性の確保等に関する法律第二条第五項に規定する高度管理医療機器
    - 厚生労働大臣が薬事・食品衛生審議会の意見を聴いて指定するもの;
  - 品目ごとにその製造販売についての厚生労働大臣の登録を受けた者（以下「登録認証機関」という。）の認証を受けなければならない。
    - 独立行政法人医薬品医療機器総合機構 認証品目公表リスト

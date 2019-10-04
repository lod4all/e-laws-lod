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

## データセット説明

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
prefix law: <http://lod4all.net/law/resource/>
prefix lawp: <http://lod4all.net/law/property/>
prefix lawo: <http://lod4all.net/law/ontology/>
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix oa: <http://www.w3.org/ns/oa#>
  
select * 
  where {
?s  lawp:lawTitle "租税特別措置法";
     ?p ?o} order by ?s
```

Subject として
http://lod4all.net/law/resource/332AC0000000026とhttp://lod4all.net/law/resource/昭和２１年法律第１５号★　が取得される。後者は　lawp:del_flg 1 により廃止法令であることがわかる。


### 特定の法令のある条項の文章の取得
```sparql
prefix law: <http://lod4all.net/law/resource/>
prefix lawp: <http://lod4all.net/law/property/>
prefix lawo: <http://lod4all.net/law/ontology/>
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix oa: <http://www.w3.org/ns/oa#>
prefix cnt: <http://www.w3.org/2011/content#> 
prefix doco: <http://purl.org/spar/doco/>
  
select ?para ?item ?p ?o ?sentence 
  where {
?s a doco:Sentence;
cnt:chars ?sentence;
lawp:clause ?clause .
?clause lawp:upper*  law:332AC0000000026_70-4;
lawp:index ?index;
lawp:paragraph ?para;
lawp:item ?item.
?s ?p ?o .
 } ORDER BY ?index
```

### 法令による定義語の違い

```sparql
prefix law: <http://lod4all.net/law/resource/>
prefix lawp: <http://lod4all.net/law/property/>
prefix lawo: <http://lod4all.net/law/ontology/>
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix oa: <http://www.w3.org/ns/oa#>
prefix cnt: <http://www.w3.org/2011/content#> 
prefix doco: <http://purl.org/spar/doco/>
  
select ?law ?body 
  where {
[] oa:hasBody [lawp:term "内国法人"; 
                        lawp:body ?body];
 oa:hasTarget/oa:hasSource ?at .
?at lawp:law/rdfs:label ?law
 } 
```

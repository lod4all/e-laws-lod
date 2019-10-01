# 日本の法令LOD

## はじめに
本データセットは、[電子政府の総合窓口](https://www.e-gov.go.jp/)から提供されている[日本の法令データ](https://elaws.e-gov.go.jp/download/lawdownload.html)をRDFに変換したものです。
法律間の参照関係を解析し、法令間のリンクを作成したほか、用語定義の抽出、文章構造の解析などを行い、これらの解析結果もRDFに反映しています。また、概要をつかみやすいように構造化したHTMLとも関連づけています。

本データセットのアプリケーションとしては、用語を探したり、委任規定などの法令参照の逆リンクをたどるなどを含めた法令読解支援アプリが考えられます。
[サンプルアプリケーションの説明資料](https://github.com/lod4all/e-laws-lod/blob/master/apps/documents/LOD%E3%83%81%E3%83%A3%E3%83%AC%E3%83%B3%E3%82%B82019_LOD4ALL.pdf)

今後は、自治体の条例などにも適用することで、地域の課題解決支援や、法令・条例に関するQAチャットボットなどの開発を考えています。

## 問い合わせ
本データセットについての問い合わせは、以下にお願いします。

株式会社 富士通研究所
lod4all-contact [at] ml.labs.fujitsu.com ( [at] を@に置き換えてください。)

## ライセンス
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.ja)

## データセットの説明
### 名前空間

```turtle
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>

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
* 法令(lawo:Law)
* 条項号(lawo:Clause)


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
  
select * 
  where {
?s a doco:Sentence;
lawp:clause/lawp:upper*  law:332AC0000000026_70-4;
cnt:chars ?sentence .
 } 
```
 
 - 条項でソートできるように、条、項、号を抜き出しておくことが必要か？
 - IDで処理しているが、ID以外で処理できるような構造にする

## SPARQL例
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

 
## 特定の法令を参照している法令の取得（未処理）
```
select * 
  where {
[]
  oa:motivatedBy oa:linking;
  oa:hasTarget/oa:hasSource/lawp:clause/lawp:upper* law:332AC0000000026; 
  oa:hasBody ?from.
 } 
```

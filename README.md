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

## データセットとデータ例
- defword.ttl: 用語定義
```
@prefix law: <http://lod4all.net/law/resource/> .
@prefix lawp: <http://lod4all.net/law/property/> .
@prefix lawo: <http://lod4all.net/law/ontology/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix oa: <http://www.w3.org/ns/oa#> . 

law:338M50000100046_5-3_1_d1_25
  rdf:type oa:Annotation, lawo:Definition;
  oa:motivatedBy oa:lidentifying;
  oa:hasTarget [a oa:SpecificResource; oa:hasSource law:338M50000100046_5-3_1_s1; 
                           oa:hasSelector [oa:start 0; oa:end 30]];
  oa:hasBody [lawp:term "相談員"; 
                        lawp:body "法第八条の二第二項の戦傷病者相談員（以下「相談員」という。）"].


law:338M50000100046_11_1_2_d1_26
  rdf:type oa:Annotation, lawo:Definition;
  oa:motivatedBy oa:lidentifying;
  oa:hasTarget [a oa:SpecificResource; oa:hasSource law:338M50000100046_11_1_2_s1; 
                           oa:hasSelector [oa:start 0; oa:end 31]];
  oa:hasBody [lawp:term "遺族"; 
                        lawp:body "請求者が法第十九条第三項に規定する遺族（以下「遺族」という。）"].
```
- lawnumname.ttl:
```
```
- lawstruct.ttl: 
```
```
- link.nt: DBpedia Japaneseとのリンク
```
```
- reflaw.ttl: 
```
```
- refterm.ttl:
```
```
- sentence.ttl:
```
```

## 利用例(サンプルSPARQL)

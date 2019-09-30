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
@prefix law: <http://lod4all.net/law/resource/> .
@prefix lawp: <http://lod4all.net/law/property/> .
@prefix lawo: <http://lod4all.net/law/ontology/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> . 

law:322AC0000000083
  rdf:type lawo:Law;
  rdfs:label "議院事務局法";
  lawp:lawNum "昭和二十二年法律第八十三号";
  lawp:lawTitle "議院事務局法";
  lawp:del_flg 0;
  lawp:year lawo:昭和22;
  lawp:lawID "322AC0000000083" .
  
law:325AC0000000147
  rdf:type lawo:Law;
  rdfs:label "国籍法";
  lawp:lawNum "昭和二十五年法律第百四十七号";
  lawp:lawTitle "国籍法";
  lawp:del_flg 0;
  lawp:year lawo:昭和25;
  lawp:lawID "325AC0000000147" .
```
- lawstruct.ttl: 
```
@prefix law: <http://lod4all.net/law/resource/> .
@prefix lawp: <http://lod4all.net/law/property/> .
@prefix lawo: <http://lod4all.net/law/ontology/> .
@prefix lawhtml: <http://lod4all.net/law/page/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> . 

law:135M10001000011_1
  rdf:type lawo:Clause;
  rdf:label "私立大学の研究設備に対する国の補助に関する法律第一条";
  lawp:clauseType lawo:Article;  
  lawp:law law:135M10001000011;
 
 lawp:html <http://lod4all.net/law/page/135M10001000011/article_1.html> .

law:135M10001000011_1_1
  rdf:type lawo:Clause;
  rdf:label "私立大学の研究設備に対する国の補助に関する法律第一条第一項";
  lawp:clauseType lawo:Paragraph;  
  lawp:law law:135M10001000011;
 
 lawp:upper law:135M10001000011_1; 
 lawp:html <http://lod4all.net/law/page/135M10001000011/article_1.html> .
```
- link.nt: DBpedia Japaneseとのリンク
```
<http://lod4all.net/law/resource/119IO0000000000> <http://www.w3.org/2000/01/rdf-schema#seeAlso> <http://ja.dbpedia.org/resource/メートル条約> .
<http://lod4all.net/law/resource/128AC0000000028> <http://www.w3.org/2000/01/rdf-schema#seeAlso> <http://ja.dbpedia.org/resource/通貨及証券模造取締法> .
<http://lod4all.net/law/resource/129AC0000000089> <http://www.w3.org/2000/01/rdf-schema#seeAlso> <http://ja.dbpedia.org/resource/民法> .
<http://lod4all.net/law/resource/130AC0000000029> <http://www.w3.org/2000/01/rdf-schema#seeAlso> <http://ja.dbpedia.org/resource/砂防法> .
<http://lod4all.net/law/resource/132AC0000000015> <http://www.w3.org/2000/01/rdf-schema#seeAlso> <http://ja.dbpedia.org/resource/供託法> .
<http://lod4all.net/law/resource/132AC0000000046> <http://www.w3.org/2000/01/rdf-schema#seeAlso> <http://ja.dbpedia.org/resource/船舶法> .
```
- reflaw.ttl: 
```
@prefix law: <http://lod4all.net/law/resource/> .
@prefix lawp: <http://lod4all.net/law/property/> .
@prefix lawo: <http://lod4all.net/law/ontology/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix oa: <http://www.w3.org/ns/oa#> . 

law:135M10001000011_2_○2_l1_38
  rdf:type oa:Annotation;
  oa:motivatedBy oa:linking;
  rdfs:label "135M10001000011_2_○2_l1_38";
  oa:hasTarget [a oa:SpecificResource; oa:hasSource law:135M10001000011_2_○2_s1; 
                           oa:hasSelector [oa:start 36; oa:end 38]];
  oa:hasBody law:135M10001000011_2_1.

law:135AC0000000011_1_1_l1_6
  rdf:type oa:Annotation;
  oa:motivatedBy oa:linking;
  rdfs:label "135AC0000000011_1_1_l1_6";
  oa:hasTarget [a oa:SpecificResource; oa:hasSource law:135AC0000000011_1_1_s1; 
                           oa:hasSelector [oa:start 3; oa:end 6]];
  oa:hasBody law:135AC0000000011_1.

law:国際観光文化都市の整備のための財政上の措置等に関する法律_平成一一年一二月二二日法律第一六〇号sp1_1_l1_8
  rdf:type oa:Annotation;
  oa:motivatedBy oa:linking;
  rdfs:label "国際観光文化都市の整備のための財政上の措置等に関する法律_平成一一年一二月二二日法律第一六〇号sp1_1_l1_8";
  oa:hasTarget [a oa:SpecificResource; oa:hasSource law:国際観光文化都市の整備のための財政上の措置等に関する法律_平成一一年一二月二二日法律第一六〇号sp1_1_s1; 
                           oa:hasSelector [oa:start 5; oa:end 8]];
  oa:hasBody law:国際観光文化都市の整備のための財政上の措置等に関する法律_2.
```
- refterm.ttl:
```
@prefix law: <http://lod4all.net/law/resource/> .
@prefix lawp: <http://lod4all.net/law/property/> .
@prefix lawo: <http://lod4all.net/law/ontology/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix oa: <http://www.w3.org/ns/oa#> . 

law:338M50000100046_5-3_1_t1_17
  rdf:type oa:Annotation, lawo:Refterm;
  oa:motivatedBy oa:lidentifying;
  oa:hasTarget [a oa:SpecificResource; oa:hasSource law:338M50000100046_5-3_1_s1; 
                           oa:hasSelector [oa:start 14; oa:end 17]];
  oa:hasBody law:338M50000100046_5-3_1_d1_25.

law:338M50000100046_11_1_2_t1_19
  rdf:type oa:Annotation, lawo:Refterm;
  oa:motivatedBy oa:lidentifying;
  oa:hasTarget [a oa:SpecificResource; oa:hasSource law:338M50000100046_11_1_2_s1; 
                           oa:hasSelector [oa:start 17; oa:end 19]];
  oa:hasBody law:338M50000100046_11_1_2_d1_26.
```
- sentence.ttl:
```
@prefix law: <http://lod4all.net/law/resource/> .
@prefix lawp: <http://lod4all.net/law/property/> .
@prefix lawo: <http://lod4all.net/law/ontology/> .
@prefix lawhtml: <http://lod4all.net/law/page/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix doco: <http://purl.org/spar/doco/> .
@prefix cnt: <http://www.w3.org/2011/content#> .  

law:135M10001000011_3_1_s1
  rdf:type doco:Sentence;
  cnt:chars "千八百九十三年十月一日以後諾威国政府ニ於テ交付シタル船舶積量測度証書ヲ受有スル諾威国ノ汽船又ハ帆船ハ帝国諸港ニ於テ其積量ヲ測度スルコトナク其証書ニ記載スル登簿噸数ヲ帝国船舶ノ登簿噸数ト同一ナリト看做ス";
  lawp:clause law:135M10001000011_3_1;  
  lawp:law law:135M10001000011 .
  
law:135M10001000011_2_1_s1
  rdf:type doco:Sentence;
  cnt:chars "千八百八十一年三月三十一日以後瑞典国政府ニ於テ交付シタル船舶積量測度証書ヲ受有スル瑞典国ノ汽船ハ帝国諸港ニ於テ其積量ヲ測度スルコトナク其証書ニ記載スル登簿噸数ニ帝国ノ船舶積量測度規則ニ依レハ控除ヲ許ササル部分ニシテ該証書ニ其控除ヲ明示シタル場所ノ噸数ヲ合セタルモノヲ帝国船舶ノ登簿噸数ト同一ナリト看做ス但瑞典国汽船ノ船長ヨリ申請アルトキハ特ニ帝国ノ船舶積量測度規則ニ定ムル割合ニ従ヒ機関室ニ対スル噸数ヲ控除シテ其登簿噸数ヲ算定ス";
  lawp:clause law:135M10001000011_2_1;  
  lawp:law law:135M10001000011 .
```

## 利用例(サンプルSPARQL)

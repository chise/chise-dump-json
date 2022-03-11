# CHISE/JSON-LD のデータ形式

## オブジェクト表現

### 文字参照 (`char-ref-info`)

文字参照には少なくとも

* `@id`		オブジェクトの ID (IRI)
* `granularity`	オブジェクトの包摂粒度 (IRI)

が含まれ、

* `title`		包摂粒度を示す括弧付きの文字列。UCS 抽象文字か UCS で表現可能なグリフ、もしくは、複数の UCS 抽象文字を包摂する抽象文字・部品の場合に付ける。
* `image`		字形画像の URL

のいずれかを含む。


### 文字情報 (`char-info`)

文字参照が持つ情報に加え、対象文字の持つ交換可能な全ての情報を含む。



## 文字情報 (`char-info`)の構成

### `@context`

JSON-LD のコンテキスト情報。root となる文字オブジェクト以外では省略される。


### `@id`
-  **型:**	IRI

文字オブジェクトの ID. 


### `title`
-  **型:**	文字列

包摂粒度を示す括弧付きの文字列。

\[**Note**\] 字体粒度の場合、括弧は省略される。


### `granularity`
-  **型:**	IRI

オブジェクトの包摂粒度。


### `image`
-  **型:**	IRI

字形画像の URL.


### `connotation`
- **型:**		文字参照の配列  
- **S 式表現:**	`<-denotational`
- **URL 表現:**	`from.denotational`

このオブジェクトを包摂するオブジェクト（親）を示す。


### `jishu`
- **型:**		文字参照の配列
- **S 式表現:**	`<-denotational@usage`
- **URL 表現:**`	from.denotational@usage`

字種。


### `component-category`
- **型:**	文字参照の配列
- **S 式表現:**	`<-denotational@component`
- **URL 表現:**	`from.denotational@component`

このオブジェクトを部品として包摂する抽象部品オブジェクトを示す。


### `radical-and-strokes`
- **型:**	キーをドメインとするコンテナー

部首・画数の情報。

値はオブジェクトで、各キーは情報のドメインを示し、値部に部首・画数に関る情報を示す。部首・画数に関する情報は次の述語で表現される：

|述語名|型|説明|
|----|----|----|
|`kangxi-radical`|文字列|康煕部首|
|`kangxi-radical-number`|自然数|康煕部首番号 (1〜214)|
|`kangxi-radical-source`|IRI の配列|康煕部首の典拠情報|
|`kangxi-strokes`|自然数|部首内画数|
|`kangxi-strokes-source`|IRI の配列|部首内画数の典拠情報|
|`total-strokes`|自然数|総画数|
|`total-strokes-source`|IRI の配列|総画数の典拠情報|

ドメインを示す URL は `chise:domain/<ドメイン名>` とする。なお、ドメイン
名が存在しない場合（これは共通部分を示す）、`chise:domain/common` とする。

例１：単一の部首画数の記述例

```json
"radical-and-strokes": {
  "chise:domain/common": {
    "kangxi-radical": "\u2f08", 
    "kangxi-radical-number": 9, 
    "kangxi-strokes": 8, 
    "total-strokes": 10
  }
}
```

例２：複数の部首画数を列挙した例

```json
"radical-and-strokes": {
  "chise:domain/cns": {
    "kangxi-radical": "\u2f17", 
    "kangxi-radical-number": 24, 
    "kangxi-strokes": 3, 
    "total-strokes": 5
  }, 
  "chise:domain/ucs": {
    "kangxi-radical": "\u2f08", 
    "kangxi-radical-number": 9, 
    "kangxi-strokes": 3, 
    "total-strokes": 5, 
    "kangxi-radical-source": [
      "bib:daikanwa", 
      "bib:ucs"
    ]
  }, 
  "chise:domain/common": {
    "kangxi-strokes": 3, 
    "total-strokes": 5
  }
}
```


### `phonetic-value`
- **型:**	キーをドメインとするコンテナー

値はオブジェクトで、各キーは情報のドメインを示し、値部に文字の音価に関る情報を示す。文字の音価に関する情報は次の述語で表現される：

|述語名|型|説明|
|----|----|----|
|`title`|文字列|音価の情報の説明|
|`url`|IRI|音価の情報の URL|
|`ja-kan-on`|日本語音表現|漢音|
|`ja-go-on`|日本語音表現|漢音|
|`ja-romaji`|文字列|日本語音（CHISE 字音仮名転写形式）|
|`ja-kana`|文字列|日本語音（現代仮名づかい）|
|`ja-kana-historical`|文字列|日本語音（字音仮名づかい）|

ここで、「日本語音表現」型は属性 `ja-romaji`, `ja-kana`, `ja-kana-historical` を持つオブジェクトである。属性 `ja-kana`, `ja-kana-historical` の値が等しい場合、属性 `ja-kana-historical` は省略可能である。

ドメインには現在の所

* `chise:domain/guangyun`
* `chise:domain/ja/on`
* `chise:domain/ja/on/conventional`
* `chise:domain/ja/on/tou`
* `chise:domain/ja/kun`
* `chise:domain/ja/kun/name`

が存在する。

キー `chise:domain/guangyun` は値として属性 `title` と `url` を持つオブジェクトの配列を取る。

キー `chise:domain/ja/on` は値として属性 `ja-kan-on` か `ja-go-on` の少なくともどちらかを持つオブジェクトの配列を取る。

その他の キー `chise:domain/ja/##` は値として「日本語音表現」型のオブジェクトの配列を取る。

例：

```json
"phonetic-value": {
  "chise:domain/guangyun": [
    {
      "title": "Web-yunzi: Guangyun-\u4e00", 
      "url": "http://suzukish.s252.xrea.com/search/inkyo/yunzi/\u4e00"
    }
  ], 
  "chise:domain/ja/kun": [
    {
      "ja-romaji": "hito-tu", 
      "ja-kana": "\u3072\u3068\u2010\u3064", 
      "ja-kana-historical": "\u3072\u3068\u2010\u3064"
    }, 
    {
      "ja-romaji": "hito", 
      "ja-kana": "\u3072\u3068", 
      "ja-kana-historical": "\u3072\u3068"
    }
  ], 
  "chise:domain/ja/kun/name": [
    {
      "ja-romaji": "hazime", 
      "ja-kana": "\u306f\u3058\u3081", 
      "ja-kana-historical": "\u306f\u3058\u3081"
    }, 
    {
      "ja-romaji": "kazu", 
      "ja-kana": "\u304b\u305a", 
      "ja-kana-historical": "\u304b\u305a"
    }
  ], 
  "chise:domain/ja/on": [
    {
      "ja-kan-on": {
        "ja-romaji": "itu", 
        "ja-kana": "\u3044\u3064", 
        "ja-kana-historical": "\u3044\u3064"
      }, 
      "ja-go-on": {
        "ja-romaji": "iti", 
        "ja-kana": "\u3044\u3061", 
        "ja-kana-historical": "\u3044\u3061"
      }
    }
  ]
}
```

### `general-category`
- **型:**	シンボルのリスト。

Unicode データベースの `general-category`.

漢字の場合は省略可。


### `bidi-category`
- **型:**	シンボルのリスト。

Unicode データベースの `bidi-category`.

漢字の場合は省略可。


### `mirrored`
- **型:**	真理値

Unicode データベースの `mirrored`（鏡像反転可能かどうか）

漢字の場合は省略可。


### `hanyu-dazidian`
- **型:**	整数のリスト

漢語大字典の巻、頁、文字番号?

例：
```json
"hanyu-dazidian": [
  1, 
  160, 
  4
]
```

### `daijiten-pages`
- **型:**	大字典頁オブジェクトの配列

大字典の頁情報。

大字典頁オブジェクトは次の属性を取る：

|述語名|型|説明|
|----|----|----|
|`daijiten-page`|自然数|ページ番号|
|`url`|IRI|頁画像の URL|

例：

```json
"daijiten-pages": [
  {
    "daijiten-page": 121, 
    "url": "http://image.chise.org/tify/?manifest=https://www.dl.ndl.go.jp/api/iiif/950498/manifest.json&tify={%22pages%22:[83]}"
  }, 
  {
    "daijiten-page": 122, 
    "url": "http://image.chise.org/tify/?manifest=https://www.dl.ndl.go.jp/api/iiif/950498/manifest.json&tify={%22pages%22:[84]}"
  }
]
```


### `citation`
- **型:**		参照オブジェクトの配列
- **S 式表現:**	`*references`

参考文献。

参照オブジェクトは次の属性を取る：

|述語名|型|説明|
|----|----|----|
|description|文字列|説明文|
|`url`|IRI|参考文献の URL|


### `structure-descriptions`
- **型:**	キーをドメインとするコンテナー

値はオブジェクトで、各キーは情報のドメインを示し、値部に漢字構造情報を示す。漢字構造情報は次の述語で表現される：

|述語名|型|説明|
|----|----|----|
|`ids`|文字列|IDS 文字列|
|`ideographic-structure`|文字表現の配列|漢字構造記述の解析木|

属性 ids の値に格納される IDS 文字列中の部品は可能な限り UCS 化されるが、実体参照も含まれ得る。

また、属性 `ideographic-structure` の値は漢字構造記述の解析木を表現したもので、この配列の最初の要素は IDC であり、2〜4 番目の要素は部品である。各部品は文字列か部品文字参照型オブジェクトで表現される。なおこの部品文字参照型オブジェクトは文字参照型オブジェクトか属性 `ideographic-structure` を持つオブジェクトである。


### `unify`
- **型:**	インデックス付きコンテナ

この文字オブジェクトに統合 (unify) される各 CCS の情報を示す。

値はオブジェクトで、各キーは統合される CCS の S 式表現を示し、値部に粒度情報付き符号化オブジェクトを取る。

符号化オブジェクトは次の述語で表現される：

|述語名|型|説明|
|----|----|----|
|`@id`|IRI|粒度情報付き符号化文字の ID|
|`CCS`|IRI|CCS の ID|
|`granularity`|IRI|粒度情報付き符号化文字の包摂粒度|
|`domain`|IRI|粒度情報付き符号化文字のドメイン|
|`code-point`|自然数|粒度情報付き符号化文字の CCS における符号位置|

なお、domain が指定されていない場合、属性 `domain` の値として `chise:domain/common` を用いる。

例：

```json
"unify": {
  "=ucs": {
    "@id": "chise:ccs/ucs/0x4FDD/abstract-character/common", 
    "CCS": "ccs:ucs", 
    "granularity": "chise:glyph-granularity/abstract-character", 
    "domain": "chise:domain/common", 
    "code-point": 20445
  }, 
  "=adobe-japan1-0": {
    "@id": "chise:ccs/adobe-japan1-0/03629/abstract-glyph/common", 
    "CCS": "ccs:adobe-japan1-0", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 3629
  }, 
  "=jis-x0208": {
    "@id": "chise:ccs/jis-x0208/0x4A5D/abstract-glyph/common", 
    "CCS": "ccs:jis-x0208", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 19037
  }, 
  "=gb2312": {
    "@id": "chise:ccs/gb2312/0x3123/abstract-glyph/common", 
    "CCS": "ccs:gb2312", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 12579
  }, 
  "=ks-x1001": {
    "@id": "chise:ccs/ks-x1001/0x5C41/abstract-glyph/common", 
    "CCS": "ccs:ks-x1001", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 23617
  }, 
  "=cns11643-1": {
    "@id": "chise:ccs/cns11643-1/0x4F71/abstract-glyph/common", 
    "CCS": "ccs:cns11643-1", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 20337
  }, 
  "=jis-x0213-1": {
    "@id": "chise:ccs/jis-x0213-1/0x4A5D/abstract-glyph/common", 
    "CCS": "ccs:jis-x0213-1", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 19037
  }, 
  "=big5": {
    "@id": "chise:ccs/big5/0xAB4F/abstract-character/common", 
    "CCS": "ccs:big5", 
    "granularity": "chise:glyph-granularity/abstract-character", 
    "domain": "chise:domain/common", 
    "code-point": 43855
  }, 
  "=gt": {
    "@id": "chise:ccs/gt/00905/abstract-glyph/common", 
    "CCS": "ccs:gt", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 905
  }, 
  "=gt-k": {
    "@id": "chise:ccs/gt-k/05181/abstract-glyph/common", 
    "CCS": "ccs:gt-k", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 5181
  }, 
  "=daikanwa": {
    "@id": "chise:ccs/daikanwa/00702/abstract-glyph/common", 
    "CCS": "ccs:daikanwa", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 702
  }, 
  "=daijiten": {
    "@id": "chise:ccs/daijiten/00342/abstract-glyph/common", 
    "CCS": "ccs:daijiten", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 342
  }, 
  "=shinjigen": {
    "@id": "chise:ccs/shinjigen/0273/abstract-glyph/common", 
    "CCS": "ccs:shinjigen", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "domain": "chise:domain/common", 
    "code-point": 273
  }
}
```


### `relations`
- **型:**	インデックス付きコンテナ

文字間の関係を示す。

値はオブジェクトで、各キーは関係素性の S 式表現を示し、値部にアノテーションオブジェクトをとる。

アノテーションオブジェクトは次の述語で表現される：

|述語名|型|説明|
|----|----|----|
|`@id`|IRI|アノテーションの ID|
|`reldesc`|オブジェクトの配列|文字間の関係記述のリスト|
|`sources`|IRI の配列|典拠情報|

属性 `reldesc` の値の要素となる関係記述オブジェクトは次の述語で表現される：

|述語名|型|説明|
|----|----|----|
|`@id`|IRI|文字間の関係の ID|
|`target`|文字参照|異体字関係のターゲット|
|`sources`|IRI の配列|文字間の関係の典拠情報|

例：

```json
"relations": {
  "<-vulgar": {
    "@id": "chise:ccs/ucs/0x815F/abstract-glyph/unicode/from.vulgar", 
    "reldesc": [
      {
        "@id": "chise:ccs/ucs/0x815F/abstract-glyph/unicode/from.vulgar/1", 
        "target": {
          "@id": "chise:ccs/ucs/0x81A3/abstract-glyph/unicode", 
          "granularity": "chise:glyph-granularity/abstract-glyph", 
          "image": "https://www.chise.org/chisewiki/glyph.cgi?char=GT-38594"
        }
      }
    ], 
    "sources": [
      "bib:shinjigen"
    ]
  }, 
  "<-wrong": {
    "@id": "chise:ccs/ucs/0x815F/abstract-glyph/unicode/from.wrong", 
    "reldesc": [
      {
        "@id": "chise:ccs/ucs/0x815F/abstract-glyph/unicode/from.wrong/1", 
        "target": {
          "@id": "chise:ccs/ucs/0x80F5/abstract-glyph/unicode", 
          "granularity": "chise:glyph-granularity/abstract-glyph", 
          "image": "https://image.hng-data.org/glyphs/daijiten/09423.png"
        }, 
        "sources": [
          "bib:zhengzitong", 
          "bib:daikanwa"
        ]
      }
    ], 
    "sources": [
      "bib:zhengzitong", 
      "bib:daikanwa"
    ]
  }
}
```


### `denotation`
- **型:**		`char-info` の配列
- **S 式表現:**	`->denotational`

字体粒度以上のオブジェクトとの包摂関係。

例：

```json
"denotation": [
  {
    "@id": "chise:ccs/ucs/0x4F01/abstract-glyph/unicode", 
    "granularity": "chise:glyph-granularity/abstract-glyph", 
    "image": "https://www.chise.org/chisewiki/glyph.cgi?char=GT-00535", 
    "radical-and-strokes": {
      "chise:domain/common": {
        "kangxi-radical": "\u2f08", 
        "kangxi-radical-number": 9, 
        "kangxi-strokes": 4, 
        "total-strokes": 6
      }
    }, 
...
    "unify": {
      "=ucs@unicode": {
        "@id": "chise:ccs/ucs/0x4F01/abstract-glyph/unicode", 
        "CCS": "ccs:ucs", 
        "granularity": "chise:glyph-granularity/abstract-glyph", 
        "domain": "chise:domain/unicode", 
        "code-point": 20225
      }, 
...
    }, 
    "subsume": [
      {
        "@id": "chise:ccs/ucs/0x4F01/abstract-glyph-form/unicode", 
        "granularity": "chise:glyph-granularity/abstract-glyph-form", 
        "image": "https://www.chise.org/chisewiki/glyph.cgi?char=GT-00535", 
        "unify": {
          "==ucs@unicode": {
            "@id": "chise:ccs/ucs/0x4F01/abstract-glyph-form/unicode", 
            "CCS": "ccs:ucs", 
            "granularity": "chise:glyph-granularity/abstract-glyph-form", 
            "domain": "chise:domain/unicode", 
            "code-point": 20225
          }, 
...
        }, 
        "subsume": [
          {
            "@id": "chise:ccs/daikanwa/00422/glyph-image/common", 
            "granularity": "chise:glyph-granularity/glyph-image", 
            "image": "https://glyphwiki.org/glyph/dkw-00422.svg"
          }, 
          {
            "@id": "chise:ccs/jis-x0208/0x346B/glyph-image/common", 
            "granularity": "chise:glyph-granularity/nil"
          }, 
...
        ]
      }
    ]
  }
```


### `subsume`
- **型:**		`char-info` の配列
- **S 式表現:**	`->subsumptive`

字体粒度未満のオブジェクトとの包摂関係。


### `unified-in-usage`
- **型:**		`char-ref-info` の配列
- **S 式表現:**	`->denotational@usage`

この属性の主語がこの値の字種であることを示す。


### `unified-as-component`
- **型:**		`char-ref-info` の配列
- **S 式表現:**	`->denotational@component`

部品としての包摂関係。

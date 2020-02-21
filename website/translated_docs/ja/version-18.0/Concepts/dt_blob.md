---
id: version-18.0-blob
title: BLOB
original_id: blob
---

- BLOB (Binary Large OBjects) フィールド・変数・式とは、連続した可変長バイトであり、各バイトを個々にアドレス指定可能な 1つのまとまったオブジェクトとして取り扱うことができます。 サポートされている BLOB のサイズは空 (長さがNULL) から、最大 2,147,483,647 バイト (2GB) までです。 
- BLOB は全体がメモリにロードされます。 BLOB 変数はメモリ内にだけ保持され、存在します。 BLOB フィールドは、レコードの他フィールドと同様に、ディスクからメモリにロードされます。
- 大量のデータを保持できる他のフィールドタイプ (ピクチャーなど) と同様に、レコードを更新してもBLOBフィールドはメモリに複製されません。 したがって、`Old` および `Modified` コマンドをBLOBフィールドに適用しても、返される結果は意味を持ちません。

## 引数渡し、ポインター、および戻り値

4D の BLOB は、4D コマンドまたは 4D プラグインの引数として渡すことができます。 また、BLOB はユーザーメソッドのパラメーターに渡したり、関数の戻り値にすることもできます。

ポインターを使用して、BLOB をメソッドに渡すことも出来ます。その場合は BLOB へのポインターを定義し、そのポインターをパラメーターとして渡します。

**例: **

```4d
  // BLOB タイプの変数を定義します
 C_BLOB(anyBlobVar)

// 4Dコマンドに引数として BLOB を渡します
 SET BLOB SIZE(anyBlobVar;1024*1024)

// プラグインに BLOB を引数として渡します
 $errCode:=Do Something With This BLOB(anyBlobVar)

// BLOB を引数として渡し、戻り値をBLOBで受け取ります
 C_BLOB(retrieveBlob)
 retrieveBlob:=Fill_Blob(anyBlobVar)

// BLOB のポインターをメソッドに渡します
 COMPUTE BLOB(->anyBlobVar)
```

**プラグイン開発にあたっての注意:** BLOB 引数は “&O” (数字の0ではなく、アルファベットの"O") として宣言します。

## 代入

BLOB は相互に代入することができます.

**例: **

```4d
  // BLOB 型の変数を二つ宣言します
 C_BLOB(vBlobA;vBlobB)
// 一つ目の BLOB に10K のサイズを割り当てます
 SET BLOB SIZE(vBlobA;10*1024)
// 一つ目の BLOB を二つ目の BLOB に代入します
 vBlobB:=vBlobA
```

ただし、BLOB に演算子を適用することはできません。

## BLOB のアドレス指定

中カッコ {...} を使用し、BLOB の各バイトを個別にアドレス指定することができます。 BLOB 内では、各バイトに 0 から N-1 の番号が割り当てられています。N は BLOB のサイズです。 例: 

```4d
  // BLOB を定義します
 C_BLOB(vBlob)
// BLOB のサイズを 256バイトに設定します
 SET BLOB SIZE(vBlob;256)
// 次のループは、BLOB の 256バイトをゼロに初期化します
 For(vByte;0;BLOB size(vBlob)-1)
    vBlob{vByte}:=0
 End for
```

BLOB の各バイトはすべて個別にアドレス指定できるため、BLOB フィールドまたは変数には実際のところ何でも格納できます。
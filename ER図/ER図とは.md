# ER図とは
ER図とは、データベースのテーブルとテーブル同士の関連を図に表したものでありデータベースのテーブル設計に用いられる。
***

# ER図の要素
## エンティティ
テーブルのこと。    
[![Image from Gyazo](https://i.gyazo.com/887224aca0576bae937b54dbf73ddd9f.png)](https://gyazo.com/887224aca0576bae937b54dbf73ddd9f)
***

## アトリビュート(属性)
テーブル内のカラム(属性値)   
この見本では    
- プライマリーキー    
- name      
- email    
- age
     
[![Image from Gyazo](https://i.gyazo.com/e35da063305b4d8177aab397724ccedd.png)](https://gyazo.com/e35da063305b4d8177aab397724ccedd)    
※ PK...プライマリーキー
***

## リレーション（関係）
エンティティ同士の関係のこと。    
Railsでいうアソシエーションのこと。    
[![Image from Gyazo](https://i.gyazo.com/f9d0ff916e10c0f3436cc654a6de10a1.png)](https://gyazo.com/f9d0ff916e10c0f3436cc654a6de10a1)
***

## カーディナリティ（多重度）
「1対1」「1対多」「多対多」など、リレーションの詳細を表現する記号。
[![Image from Gyazo](https://i.gyazo.com/c309c8ecd74b3f6a7b50cd5966c708b2.png)](https://gyazo.com/c309c8ecd74b3f6a7b50cd5966c708b2)
***

# カーディナリティの種類
[![Image from Gyazo](https://i.gyazo.com/5f2122c07573ee363b59c84a14df5f5b.png)](https://gyazo.com/5f2122c07573ee363b59c84a14df5f5b)    
基本的にはこの「0,1,多」を組み合わせて使う。
***

## 使い方(NG)
[![Image from Gyazo](https://i.gyazo.com/8c542d24b84212a4b7451bbef65684e5.png)](https://gyazo.com/8c542d24b84212a4b7451bbef65684e5)
このように、一種類ずつ使うのは不十分。
***

## 使い方(OK)
[![Image from Gyazo](https://i.gyazo.com/8658988f573ef313c82b3306a0cd2e28.png)](https://gyazo.com/8658988f573ef313c82b3306a0cd2e28)
- 一つのPostは必ず一人のUserと関連する。          
- 一人のUserはPostを複数持つが投稿しない場合もある関連を持つ。     
          
と言うように下限と上限をつける。
***

# 見本
[![Image from Gyazo](https://i.gyazo.com/ab1e0c00cd0786a12c91b67c183f51c4.png)](https://gyazo.com/ab1e0c00cd0786a12c91b67c183f51c4)
- 一人のUserは0~多の投稿を持つ。          
- 一つの投稿は必ず一人のUserと結びつく。     
                
=> User:Post = 1:多(０含む)     
     
- 一人のUserは0~多のコメントを持つ。     
- 一つのコメントは必ず一人のUserと結びつく。     
     
=> User:Comment = 1:多(0含む)     
     
- 一つのPostは0~多のコメントを持つ。          
- 一つのコメントは必ず一つのPostと結びつく。          
          
=> Post:Comment = 1:多(0含む)   
***

### ❓ 1:1? 　1:多?
例えば、user_id:1が自分自身だとする。               
Postテーブルの中の全データーから、user_id:1と言う情報だけで一件のPostが特定できるだろうか？          
答えは、NO。          
「user_id:1, post_id:1」、「user_id:1, post_id:2」など多数出てくる。                    
          
逆の視点に変えて今度はPostからみる。     
post_id:1と言う情報だけで一件のPostが特定できるだろうか？        
答えは、YES。     
「user_id:1, post_id:1」なら、Userはuser_id:1の人だと特定できる。     

なので User:Post = 1:多(０含む)
***



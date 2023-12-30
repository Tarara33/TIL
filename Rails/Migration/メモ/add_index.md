# add_index
指定したテーブルにインデックスを追加するメソッド。  
インデックスとは、たとえば本でいう牽引のようなもの。  
    
たとえば、このような Userテーブルを用意する    
      
[![Image from Gyazo](https://i.gyazo.com/50a75d8c0aa724ac3314c0d2acab979e.png)](https://gyazo.com/50a75d8c0aa724ac3314c0d2acab979e)  
***

## インデックスなしの場合
emailにユニーク制約だけつけたとする。
~~~
t.string :email, unique: true
~~~
そうすると、たとえば emailが「c22」という新しいユーザーが登録されるとき、  
DBは emailが同じやつがいないか、id 1から一つづつ確認していく。    
これが10件とかなら大丈夫だが、10000件だとすごく負担になる。  
~~~
[MYSQL]
SELECT * FROM users WHERE email = c22(特定のメールアドレス);
~~~
[![Image from Gyazo](https://i.gyazo.com/51919bba9301d3e5d489b94d772f98cd.png)](https://gyazo.com/51919bba9301d3e5d489b94d772f98cd)
***
  
## インデックスありの場合
emailにインデックスとユニーク制約をつけたとする。
~~~
add_index :users, :email, unique: true
~~~
そうすると、たとえば emailが「c22」という新しいユーザーが登録されるとき、
DBは emailが cから始まるところから探し始める。    
なので、早い + 負担が減る。
~~~
[MYSQL]
SELECT * FROM users WHERE email = c22(特定のメールアドレス);
~~~
[![Image from Gyazo](https://i.gyazo.com/cda27c64d29991e388ed817348b91d82.png)](https://gyazo.com/cda27c64d29991e388ed817348b91d82)
***

# 複合キーインデックス
インデックスのキーにするものが複数ある場合のこと。

たとえばこのように書いておくと、
~~~
add_index :users, [:name, :email], unique: true
~~~
まず、nameでインデックスかけたもののなかで、さらに emailでインデックスかけて探す。   
        
たとえば　nameが「D3」の emailが「c66」を探すとなると、こんな感じで探す。
~~~
[MYSQL]
SELECT * FROM users WHERE name = 'D３' AND email = 'c66';
~~~
[![Image from Gyazo](https://i.gyazo.com/6b21955d2b29c6ab65c72505d5c1ba15.png)](https://gyazo.com/6b21955d2b29c6ab65c72505d5c1ba15)
***

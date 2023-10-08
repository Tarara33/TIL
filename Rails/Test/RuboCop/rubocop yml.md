# rubocop.yml
RuboCopの全体的な設定を書くファイル。  
ここで各種ルールを有効、無効にしたり、設定を変更したりする。

デフォルト設定は[ココ](https://github.com/rubocop/rubocop/blob/master/config/default.yml)にあるが、量が多いので自分でカスタマイズすることが多い。
***

# 設定によく出るコード
### .rubocop_todo.ymlファイルを継承
.rubocop_todo.ymlファイルを継承する。  
つまり、そこに退避されてるエラーは無視するということ。
~~~
inherit_from: .rubocop_todo.yml
~~~
***

### 拡張の継承
RuboCopが必要とする拡張を指定するためのもの。  
なので Gemfileに記載した rubocop系gemたち = requireさせるというわけではなく必要に応じて書く。
~~~
require:
  - rubocop-capybara
  - rubocop-rails
~~~
***

## RuboCopの設定においてのカテゴリ
### AllCop  
RuboCop全体の設定
***

### Exculude  
特定のファイルまたはディレクトリを除外する設定  
この例では、vendor/、db/、bin/、spec/、node_modules/ ディレクトリ内のファイルは除外されている。
***

### Layout    
コードのフォーマットやレイアウトに関連するルールが含まれている。  
例えば、インデント、空白の扱い、改行の位置など。
***

### Metrics  
コードの複雑度や長さに関連するルールが含まれている。  
メソッドやクラスの長さ、条件分岐の複雑度、ABC（Assignment, Branch, Condition）メトリクスなどが含まれる。
***

### Naming  
このカテゴリには、変数、メソッド、クラスなどの命名規則に関連するルールが含まれている。  
適切な命名規則を遵守することで、コードの可読性を向上させることができる。
***

### Style  
このカテゴリには、コーディングスタイルに関連するルールが含まれている。  
例えば、カーリー括弧のスタイル、インデントのスタイル、メソッドやクラスの定義のスタイルなど。
***

### Bundler  
このカテゴリには、Rubyの Gemパッケージ管理に関連するルールが含まれている。  
Gemfileの書き方や Gemのバージョン指定に関するルールが含まれる。
***

### Lint
このカテゴリには、一般的なコード品質や問題に関連するルールが含まれている。  
例えば、未使用の変数やメソッドの検出、コード内の冗長な部分、ポテンシャルなエラーや問題の検出など。
***

## カテゴリ内のオプション
### Enabled (true or false)
特定の RuboCopルールを有効または無効にするために使用される。  
そもそもルールを発動するかどうかって感じ。
***

### CountComments (true or false)
特定の RuboCopルール内でコード内のコメントを警告やエラーの対象から除外するかどうか。  
これが trueだと、コメントアウトも引っかかる。
***

### Max
カウント数の上限。
***

# 設定見本
~~~
[rubocop.yml]

inherit_from: .rubocop_todo.yml

require
  - rubocop-rails
  
AllCops:
  Exclude:
    - 'vendor/**/*'
    - 'db/**/*'
    - 'bin/**/*'
    - 'spec/**/*'
    - 'node_modules/**/*'
  🌞DisplayCopNames: true

Layout/MultilineBlockLayout:  💡複数行のブロックのレイアウトに関連するルール
  Exclude:
    - 'spec/**/*_spec.rb'

Layout/LineLength:  💡コードの各行の文字数の制限に関連するルール
  Enabled: false

Metrics/AbcSize:  💡コード内のメソッドや関数の複雑性を評価し、その複雑性が一定の閾値を超える場合に警告を発生させる。
  Max: 25　　　　　　　　　　　　　　　　　　　　　　A= メソッド内で変数に値を割り当てる回数をカウントする。
                   　　B= メソッド内での条件分岐（if 文、case 文など）の数をカウントする。
                  　　　　C= メソッド内での条件式の数をカウントする。

Metrics/BlockLength:  💡コード内のブロック（メソッド、クラス、モジュール、条件文など）の長さを評価し、長いブロックに警告を発生させる。
  Max: 30
  Exclude:
    - 'Gemfile'
    - 'config/**/*'
    - 'spec/**/*_spec.rb'

Metrics/ClassLength:  💡クラスの行数が一定の閾値を超える場合に警告を生成することを目的としている。
  CountComments: false
  Max: 300

Metrics/CyclomaticComplexity:  💡コード内のメソッドの複雑性を評価し、メソッドのサイクロマティック複雑性（Cyclomatic Complexity）が一定の閾値を超える場合に警告を生成する。
  Max: 30

Metrics/MethodLength: 💡メソッドの行数が一定の閾値を超える場合に警告を生成することを目的としている。
  CountComments: false
  Max: 30

Naming/AccessorMethodName:  💡Rubyのクラスやモジュール内でアクセサーメソッド（ゲッターメソッドやセッターメソッド）の名前付けに関連する慣習に従うことを確認する。
  Exclude:
    - 'app/controllers/**/*'

Style/AsciiComments:  💡プロジェクト内のコメントがASCII文字で記述されているかどうかを確認し、違反がある場合に警告を生成する。
  Enabled: false

Style/BlockDelimiters:  💡Rubyコード内でブロックを定義する際の括弧のスタイルを統一させることを目的としている。
  Exclude:
    - 'spec/**/*_spec.rb'

Style/ClassAndModuleChildren:  💡クラスやモジュール内の子要素の配置や整理に関する慣習を確認し、コードの一貫性を維持することを目的としている。
  Enabled: false

Style/Documentation:  💡コード内に適切なコメントやドキュメンテーションが含まれていることを確認し、コードの理解やメンテナンスを助けることを目的としている。
  Enabled: false

Style/FrozenStringLiteralComment:  💡文字列リテラルが凍結されていることを明示的に示すコメントが存在するかどうかを確認し、コードの安全性と予測可能性を向上させることを目的としている。
  Enabled: false

Style/IfUnlessModifier:  💡条件文をより簡潔に表現するためのスタイルガイドに従うことを確認し、コードの一貫性を維持することを目的としている。(ifや unlessの条件文が修飾子形式で書かれているか)
  Enabled: false

Style/WhileUntilModifier:  💡条件文をより簡潔に表現するためのスタイルガイドに従うことを確認し、コードの一貫性を維持することを目的としている。(whileや untilの条件文が修飾子形式で書かれているか)
  Enabled: false

Style/HashEachMethods:  💡ハッシュをイテレートする際の一貫性を確保し、コードの可読性を向上させることを目的としている。
  Enabled: false

Style/HashTransformKeys:  💡ハッシュ内のキーを変換する方法に一貫性を持たせ、コードの可読性を向上させることを目的としている。({"name" => "John"} の代わりに {:name => "John"}など)
  Enabled: false

Style/HashTransformValues:  💡ハッシュ内の値を変換する方法に一貫性を持たせ、コードの可読性を向上させることを目的としている。({name: :John} の代わりに {name: "John"}など)
  Enabled: false

Bundler/OrderedGems:  💡Gemfile内の Gemのリストが特定の順序に従っているかどうかを検証し、一貫性を持たせることを目的としている。
  Enabled: false
~~~
***

### 🌞DisplayCopNames  
コードの検査結果において、各コップ（ルール）の名前を表示する設定。  
これにより、どのコップが警告またはエラーを生成したかを明示的に表示する。  

【trueの場合】  

[![Image from Gyazo](https://i.gyazo.com/434ddff1e3268496d5ace9abb8839e53.png)](https://gyazo.com/434ddff1e3268496d5ace9abb8839e53)

【falseの場合】  

[![Image from Gyazo](https://i.gyazo.com/fe88ae2412825036253bf60b4e2ee992.png)](https://gyazo.com/fe88ae2412825036253bf60b4e2ee992)

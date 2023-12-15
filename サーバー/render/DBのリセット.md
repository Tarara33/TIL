# DBをリセットする
データーを全て消してリセットする方法
***

# bin/renderbuild.shの編集
~~~
[bin/renderbuild.sh]

#!/usr/bin/env bash
# exit on error
set -o errexit

bundle install
bundle exec rake assets:precompile
bundle exec rake assets:clean

⭐️DISABLE_DATABASE_ENVIRONMENT_CHECK=1 bundle exec rails db:migrate:reset
~~~
この１行を追加して、サーバーを立ち上げると、リセットされる。

### ⚠️ リセットしたらコード元に戻す！
戻さないと、サーバー立ち上がるたびにリセットされる。
~~~
[bin/renderbuild.sh]

#!/usr/bin/env bash
# exit on error
set -o errexit

bundle install
bundle exec rake assets:precompile
bundle exec rake assets:clean
⭐️bundle exec rails db:migrate:reset
~~~
***

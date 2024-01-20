# APIでのデプロイ
フロントエンドを react、バックエンドを railsになど分けた場合はデプロイはそれぞれ行う。  

例えば、herokuを使った時は、  
フロントサイドは `https://your-react-app.herokuapp.com` のような URLが振られ、    
バックサイドは `https://your-rails-app.herokuapp.com`のような URLが振られる。  
***

## ❓ バックエンドのサイトに飛んだらどうなる？？
フロントサイドの URLは飛んだらアプリが使える。  
しかし、バックサイドの URLに飛んだらどうなる？？  
=> 飛べるけど、人間が見るには意味がわからないページが出る(JSONとか)！
***

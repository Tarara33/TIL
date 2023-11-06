[参考](https://wa3.i-3-i.info/word172.html)  
  
# IPアドレス  
「Internet Protocol address（インターネット・プロトコル・アドレス）」の略で「IPアドレス」  
ネットワークにおけるコンピューターの住所のようなもの。    
ネットワークに接続されているコンピューターは必ず IPアドレスを持っていて、通信をする際には必ず IPアドレスを指定する。
	
⭐️ ちなみに、localhostの IPアドレスは「127.0.0.1」
***

## グローバル IPアドレス
全世界で通用する IPアドレス。      
インターネットのこと。
***

## プライベート IPアドレス
仲間内でのみ通用する IPアドレス。    
LAN（例えばあなたの自宅内のネットワーク）のこと。  
***

### ネットワーク部 と ホスト部
[参考](https://en-junior.com/vpc/#index_id4)  
プライベート IPアドレスは、パブリック IPアドレスとは異なり、「ネットワーク部」と「ホスト部」に分けられる。  
ネットワーク部はプライベート IPアドレスの中でも所属するグループを表していて、	ホスト部はそのグループの中で1つを特定する部分。  

例えば「1年2組の田中くん」の例であれば、「1年2組」と言う部分がネットワーク部で、  
「田中くん」と言う部分が、個人を特定するためのホスト部となる。  

IPアドレスの場合は、アドレスの数字の途中でネットワーク部とホスト部を分ける。  
たとえば、2つ目のドットまでをネットワーク部、残りをホスト部とするといった感じや、最初の24桁をネットワーク部とすることも可能。

***

# IPアドレスの確認
[このサイト](https://www.cman.jp/network/support/go_access.cgi)でできる。
***

# プライベート IPアドレスでの通信
例えば、同一のLANを使っていたら、スマホからも「http://localhost:3000」にアクセスしてアプリ開ける。

## ① プライベート IPアドレスの確認
~~~
$  networksetup -listallhardwareports

=>
Hardware Port: Ethernet Adapter (en3)
Device: en3
Ethernet Address: 1e:a0:93:1f:cd:27

Hardware Port: Ethernet Adapter (en4)
Device: en4
Ethernet Address: 1e:a0:93:1f:cd:28

Hardware Port: Thunderbolt Bridge
Device: bridge0
Ethernet Address: 36:60:d6:7b:57:80

Hardware Port: Wi-Fi
Device: en0
Ethernet Address: 80:65:7c:d9:20:4e

Hardware Port: Thunderbolt 1
Device: en1
Ethernet Address: 36:60:d6:7b:57:80

Hardware Port: Thunderbolt 2
Device: en2
Ethernet Address: 36:60:d6:7b:57:84

VLAN Configurations
===================
~~~
- en0...Wi-Fi接続    
- en1 & en2...Thunderboltポートに接続されたネットワークデバイス    
- en3 & en4...有線Ethernet接続    
- bridge0...Thunderbolt Bridge接続  
    
今回の場合W i-Fiを使用しているので、en0というデバイスにプライベート IPアドレスが割り当てられているはず。    
`ifconfig [デバイス名]`とコマンドを実行することで、そのデバイスに割り振られたプライベート IPアドレスが確認できる。    
~~~
$ ifconfig en0

=>
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=6463<RXCSUM,TXCSUM,TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
	ether 80:65:7c:d9:20:4e 
	inet6 fe80::1063:d4f1:c145:e8d6%en0 prefixlen 64 secured scopeid 0xc 
	inet6 2400:2410:da60:9a00:1cc6:d1c4:7b3c:6eee prefixlen 64 autoconf secured 
	inet6 2400:2410:da60:9a00:f17c:6bc5:bfe5:9e2f prefixlen 64 autoconf temporary 
	inet 192.168.3.9 netmask 0xffffff00 broadcast 192.168.3.255
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
~~~
`inet`の部分がプライベート IPアドレスなので、今回は「192.168.3.9」
***

### ❓ なぜハードウェアがたくさん出てくる？？
Macコンピューターは複数のネットワークインターフェースを持つことがあるため。    
このコマンドを実行すると、すべての利用可能なネットワークハードウェアポートがリストされる。
***

## ② スマホで接続
サーバーを立ち上げるときに、`-b 0.0.0.0`をつける。
~~~
$ rails s -b 0.0.0.0
~~~
「localhost」の部分にプライベート IPアドレス入れるので、    
スマホの URL入力で「http://192.168.3.255:3000」と打つと繋がる。
***

# DNS
IPアドレスがわからなくてもドメイン名をブラウザに打ち込めばサイトにアクセスすることができる。	  
実はコンピューターは私たちの見えないところでドメイン名を　IPアドレスに変換していて、その仕組みのことを DNSと言う。	  
ドメイン名から IPアドレスを索引してくれるサーバーのことを DNSサーバーと言う。	  
また、ドメイン名から IPアドレスを索引することを名前解決と言う。		 	  
		    
Macには名前解決するためのコマンドラインツールである nslookupが標準インストールされている。  
`nslookup github.com`と実行してみると、GitHubにアクセスする際に必要な IPアドレスが索引できる。  
~~~
$ nslookup github.com
Server:		240b:12:2ae1:d300:8222:a7ff:fe24:3dc
Address:	240b:12:2ae1:d300:8222:a7ff:fe24:3dc#53

Non-authoritative answer:
Name:	github.com
Address: 52.192.72.89
~~~
⚠️ 但し、IPアドレスで直接サイトにアクセスすることはサーバー側の設定で許可されていないケースは多い。	  
また、IPアドレスからドメイン名にリダイレクトさせるケースもあり、その際にブラウザが警告を発したりする（GitHubがまさにそう）

 [![Image from Gyazo](https://i.gyazo.com/89bf06b068dab3c7f8ecbaa384128b7b.png)](https://gyazo.com/89bf06b068dab3c7f8ecbaa384128b7b)
***

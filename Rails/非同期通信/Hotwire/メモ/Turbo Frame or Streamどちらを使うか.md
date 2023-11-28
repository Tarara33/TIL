# Turbo Frame か　Stream どちらを使うか
基本的には Turbo Framesを使い、Turbo Framesで実現できない場合に Turbo Streamsを使えばいい。

なので、  
編集機能を一覧画面でページ遷移せず Turbo化したい = 一ヶ所の変更 = Turbo Frame   
編集機能を一覧画面でページ遷移せず Turbo化したい + Flashも出したい = 複数箇所の変更 = Turbo Stream
***

# page
### have_content
~~~
 expect(page).to have_content('更新しました')
~~~
***

### not_to
~~~
expect(page).not_to have_content('更新しました')
~~~
***

### have_css
~~~
expect(page).to have_css('.eye_catch')
~~~
***

### have_selector
~~~
expect(page).to have_selector('section.eye_catch.text-left')
expect(page).to have_selector("input[value=#{author.name}]")
~~~
***

### have_http_status
コレする時は ドライバーを RackTestに切り替える必要がある。 
capybaraの設定で他のドライバー使ってても、テストファイル内にコレ書いてあれば、特定のテストは RackTestに切り替わる。
~~~
before do
  driven_by(:rack_test)
end
~~~
~~~
expect(page).to have_http_status(200)
~~~
***

# current_path
### eq
~~~
expect(current_path).to eq(admin_article_preview_path(article.uuid))
~~~
***

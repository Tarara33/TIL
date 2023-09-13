# response 
### have_http_status
~~~
expect(response).to have_http_status(:success)
~~~
***

### redirect_to
~~~
expect(response).to redirect_to user
~~~

# response.body
### include 
~~~
expect(response.body).to include full_title('Sign up')
expect(response.body).to include '<div class="pagination">'
~~~
  
⚠️ includeはタグ内の属性全てと比較するので、
~~~
[rspec]

expect(response.body).to include "<input type=\"hidden\" name=\"email\" id=\"email\" value=\"#{@user.email}\" />"

[生成HTML]

<input type="hidden" name="email" id="email" value="sarina0618@gmail.com" autocomplete="off">
~~~
このように`autocomplete`がテストに入ってなかったりすると失敗する。
***

# flash
### be_any
~~~
expect(flash).to be_any
~~~
***

### be_empty
~~~
expect(flash).not_to be_empty
~~~
***

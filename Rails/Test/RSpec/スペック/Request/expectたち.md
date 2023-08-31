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
~~~
***

# flash
### be_any
~~~
expect(flash).to be_any
~~~
***



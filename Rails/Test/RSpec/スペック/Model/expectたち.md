# モデル名
### be_valid
~~~
expect(user).to be_valid
~~~
***

### be_invalid
~~~
expect(user).to be_invalid
~~~
***

# モデル名.authenticated?
### be_falsy
~~~
expect(user.authenticated?('')).to be_falsy
~~~
***

### be_truthy
~~~
expect(user.authenticated?('')).to be_truthy
~~~
***

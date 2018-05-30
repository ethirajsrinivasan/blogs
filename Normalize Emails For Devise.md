## Normalize Emails For Devise

Devise - a flexible authentication solution for Rails based on Warden says [plataformatec](https://github.com/plataformatec/devise). Yes it is and is my favorite authentication gem.Due to its flexible nature it does have some issues pouring down. I got a chance to view this [issue](https://github.com/plataformatec/devise/issues/4309).The issue as stated by the author

We're trying to apply some normalizing rules on email addresses.

  E.g. user@googlemail.com -> user@gmail.com.

We try to overwrite the params inside the session controller 
like this

```ruby
def create
   params[:user][:email] = normalize(params[:user][:email])
   super
end
```

However, somehow the authentication fails, and we can't figure
out why.We tried to do the same before new,but it doesn't work either.

What are we doing wrong?

The code seemed to be perfect but why is this problem.The solution to the problem lies here

```ruby
#/lib/devise/rails/warden_compat.rb
def request
  @request ||= ActionDispatch::Request.new(env)
end

#/lib/warden/mixins/common.rb
def params
  request.params
end
```

The params are defined from env variable.Overriding the local params does not change the values in env variables.So we must override the value in request to solve this problem.

The solution

```ruby
def create
  params[:user][:email] = normalize(params[:user][:email])
  request.params[:user].merge!(params[:user])
  super
end
```


Thanks to [gingerlime](https://github.com/gingerlime).Happy Coding!!!

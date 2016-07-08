---
published: true
title: How To Test Your Rails Web API with POSTMAN In 5 Minutes
layout: post
---
### Setup Postman

 1. Install Postman on Chrome app from [here](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en)
 2. Activate cookie interceptor with following the following artcle. [Using interceptor to read and write cookies](https://www.getpostman.com/docs/interceptor_cookies)
 
 ## Prepare you request

 ### Get request
 You just fill in the URL where you want to make a request so that you can make a GET request to it by clicking 'Send' button.
 
 ### Post request
 You need to remember the mechanics of preventing CSRF attacks in Rails. Probably you have seen the below code.

*atnd_stash/app/controllers/application_controller.rb*

```
・・・
protect_from_forgery with: :exception
・・・
```

This code allows to raise the exception when a POST request is made by unknow host. On the context, if you make a request like GET way, you can see the below error.

```
Can't verify CSRF token authenticity
Completed 422 Unprocessable Entity
```

To avoid this, we need to set the proper csrf token with key `X-CSRF-Token` to the HTTP header. And the toke is embeded in every pages rendered by Rails app.
You could get it with the below JQuery snippet in Chrome console.

```
$('meta[name="csrf-token"]')
[<meta content=​"jbd3s74JmH+MUVU31UQFUZWpghy2ATAgTsRQFqyTkKM=" name=​"csrf-token">​]
```

Now that you got proper csrf token, you should be able to make a POST request. Let's enjoy the confortable web API development!

### References

- [Using interceptor to read and write cookies](https://www.getpostman.com/docs/interceptor_cookies)
- [Postman on Chrome web store](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en)
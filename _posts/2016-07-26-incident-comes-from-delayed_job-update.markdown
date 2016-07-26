---
published: false
title: Incident came from DelayedJob Update
layout: post
---

### The Incident I've Got
In a couple days ago, I've got a incident which comes from the DelayedJob update from `4.0.1` to `4.1.2`. After the update, the DJ was down and we couldn't start it again. As a result, our End user was not able to send or receive messages which were delayed to process. 

### Our code disabling DJ to start again
I've found the applicatoin error when I ran the boot script of DJ.

```
bin/delayed_job run
XXXXXXXXXX # Application dependent error 
```

The error is raised in the scope of a AR model. The scope is supposed to raise the exeption when the class instance is not initialized. This hit me to assume that the updated DJ has changed the timing to load the code.

### Which versions do produce the issue?
I tried to figure out the actual cause. So I dug in the DJ change logs. After my some tests on each version. I found that it's been causing from `4.0.4`. The change log is as follows.

```
- Support for Rails 4.2
- Allow user to override where DJ writes log output
- First attempt at automatic code reloading
- Clearer error message when ActiveRecord object no longer exists
- Various improvements to the README
```

### References
- [delayed_job/CHANGELOG.md](https://github.com/collectiveidea/delayed_job/blob/master/CHANGELOG.md)
- [Don't override AR yaml generation](https://github.com/collectiveidea/delayed_job/commit/a47e0d7b55a30fb5853f20b139caddd39535727f)
- [psych/lib/psych/visitors/to_ruby.rb](https://github.com/tenderlove/psych/blob/master/lib/psych/visitors/to_ruby.rb#L208)
- [delayed_job/lib/delayed/psych_ext.rb](https://github.com/collectiveidea/delayed_job/blob/master/lib/delayed/psych_ext.rb#L40)
- [Missing 'unscoped' in psych_ext ](https://github.com/collectiveidea/delayed_job/pull/737)


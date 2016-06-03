---
published: false
title: Learn How To Build Your Server From Chef Cookbook Recipes
layout: post
---
# Prerequisite
- OS X Yosemite 10.10.5

# Install ChefDK

You can download it [here](https://downloads.chef.io/chef-dk/mac/).

Add the aliases below to your .bashrc for the convenience

```
cat <<EOF >> ~/.bashrc
# chef
alias cf='chef'
alias cfc='chef-client'
EOF
source ~/.bashrc
```

```
CA0905:chef-repo uu089149$ cf -v
Chef Development Kit Version: 0.14.25
chef-client version: 12.10.24
berks version: 4.3.3
kitchen version: 1.8.0
```


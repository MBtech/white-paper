---
layout: post
title: "Cheatsheet OpenFaaS"
categories:
  - cheatsheet
tags:
  - cheatsheet
---
To avoid combination of stderr/stdout that is enabled by default add this to the .yaml file within a particular function block
```
  environment:
        combine_output: false
```

Client-side chaining:
```
echo -n "" | faas-cli invoke nodeinfo | faas-cli invoke markdown
```

**References:**

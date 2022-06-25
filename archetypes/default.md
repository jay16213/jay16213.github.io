---
title: "{{ replace .Name "-" " " | title }}"
summary: ""
tags: []
categories: []
date: {{ .Date }}
lastmod: {{ .Date }}
draft: true

# toha params
menu:
  sidebar:
    name: {{ replace .Name "-" " " | title }}
    identifier: {{ .Name }}
    parent:
    weight: 10
---

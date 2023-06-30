---
title: Petunjuk Host Online
permalink: index.html
layout: home
---

# Direktori Konten

File lab yang diperlukan dapat [DIUNDUH DI SINI](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip)

## Lab

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Modul | Laboratorium |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

## Demonstrasi

{% assign demos = site.pages | where_exp:"page", "page.url berisi '/Instructions/Demos'" %}
| Modul | Demonstrasi |
| --- | --- | 
{% for activity in demos  %}| {{ activity.demo.module }} | [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

---
title: Instruksi yang Tersedia Online
permalink: index.html
layout: home
---

# Daftar Isi Direktori

File lab yang diperlukan dapat [DIUNDUH DI SINI](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip)

## Lab

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Modul | Laboratorium |
| --- | --- | 
{% untuk aktivitas di lab %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

## Demonstrasi

{% assign demos = site.pages | where_exp:"page", "page.url berisi '/Instructions/Demos'" %}
| Modul | Demonstrasi |
| --- | --- | 
{% untuk aktivitas di Demo %}| {{ activity.demo.module }} | [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

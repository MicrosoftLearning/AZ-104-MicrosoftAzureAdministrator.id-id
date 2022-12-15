---
title: Petunjuk Host Online
permalink: index.html
layout: home
---

# <a name="content-directory"></a>Direktori Konten

File lab yang diperlukan dapat [DIUNDUH DI SINI](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip)

Hyperlink ke masing-masing latihan lab tercantum di bawah ini.

## <a name="labs"></a>Lab

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Modul | Laboratorium |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}



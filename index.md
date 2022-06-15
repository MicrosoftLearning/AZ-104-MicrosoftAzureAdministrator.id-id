---
title: Petunjuk Host Online
permalink: index.html
layout: home
ms.openlocfilehash: 1dde36744b9541205d719973757171e13ec37223
ms.sourcegitcommit: 8a0ced6338608682366fb357c69321ba1aee4ab8
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 11/08/2021
ms.locfileid: "145198179"
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



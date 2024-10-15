---
title: Online gehostete Anweisungen
permalink: index.html
layout: home
---

# Inhaltsverzeichnis

Erforderliche Übungsdateien können [HIER](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions/archive/master.zip) HERUNTERGELADEN werden

Scrollen Sie nach unten für Demonstrationen. 

## Übung

{% assign Exercise = site.pages | where_exp:"page", "page.url contains '/Instructions/Exercises'" %}
| Modul | Übung |
| --- | --- | 
{% for activity in Exercise  %}| {{ activity.Exercise.module }} | [{{ activity.Exercise.title }}{% if activity.Exercise.type %} - {{ activity.Exercise.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}

<!-- ## Demonstrations (under construction)

{% assign demos = site.pages | where_exp:"page", "page.url contains '/Instructions/Demos'" %}
| Demonstration |
| --- | 
{% for activity in demos  %}| [{{ activity.demo.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
-->



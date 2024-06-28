---
layout: page
title: Vanilla Classifier
description: Designed a linear classifier from scratch. 
# img: assets/img/project/linear-classifier.png
importance: 4
category: AI
published: false
giscus_comments: false
---

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/linear-classifier.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/linear-classifier.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
{% jupyter_notebook jupyter_path %}
{% else %}

<p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}
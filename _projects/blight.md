---
layout: page
title: Blight Detection
description: Assisting the farmers by potato blight grade detection.
img: assets/img/blight.jpg
importance: 3
category: AI
giscus_comments: false
---

> Code: [GitHub Repo](https://github.com/hrishikeshh/Blight-Classification)
>
> For API architecture, refer the above codebase.

---
[Early blight](https://ipm.cahnr.uconn.edu/early-blight-and-late-blight-of-potato/) and [late blight](https://extension.umn.edu/disease-management/late-blight), two serious diseases of potato, are widely distributed. We are detecting the stage of the blight using deep learning to help farmers take appropriate preventive actions. 

---

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/potato-disease-classification-model-using-image-data-generator.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/potato-disease-classification-model-using-image-data-generator.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
{% jupyter_notebook jupyter_path %}
{% else %}

<p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}
---
layout: splash
permalink: /
# hidden: true
header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/philly.jpg
  #actions:
  #  - label: "<i class='fas fa-download'></i> Install now"
  #    url: "/docs/quick-start-guide/"
excerpt: >
  Featuring tutorials and projects from my Statistics classes at Brigham 
Young University.
feature_row:
  - image_path: /assets/images/tutorial.jpg
    alt: "posts"
    title: "Blog Posts"
    excerpt: "Blog posts for my Data Science Process class."
    url: "/posts/"
    btn_class: "btn--info"
    btn_label: "Go to Blog Posts"
  - image_path: /assets/images/competition.jpg
    alt: "competitions"
    title: "Kaggle Competitions"
    excerpt: "See code and scores from various Kaggle Competitions."
    url: "/assignments/"
    btn_class: "btn--info"
    btn_label: "Go to Kaggle Competitions"
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}

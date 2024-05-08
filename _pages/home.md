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
excerpt: "Featuring tutorials and projects from my Statistics classes at Brigham Young University."
feature_row:
  - image_path: /assets/images/tutorial.jpg
    alt: "posts"
    title: "Blog Posts"
    excerpt: "Blog posts for my Data Science Process class."
    url: "/posts/"
    btn_class: "btn--info"
    btn_label: "Go to Blog Posts"
  - image_path: /assets/images/competition.jpg
    alt: "projects"
    title: "Projects"
    excerpt: "See written reports code from various assignments and projects."
    url: "/projects/"
    btn_class: "btn--info"
    btn_label: "Go to Projects"  
  - image_path: /assets/images/jonas-jacobsson-0FRJ2SCuY4k-unsplash.jpg
    alt: "bio"
    title: "About Me"
    excerpt: "Learn more about my qualifications."
    url: "/about/"
    btn_class: "btn--info"
    btn_label: "Go to Bio"
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}

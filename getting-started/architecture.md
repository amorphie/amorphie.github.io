---
layout: spec
mermaid: true
---

# Architecture

```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```


<div class="mermaid">
flowchart TD;
    A[Deploy to production] --> B{Is it Friday?};
    B -- Yes --> C[Do not deploy!];
    B -- No --> D[Run deploy.sh];
    C --> E[Enjoy your weekend!];
    D --> E;
</div>  


{% if page.mermaid }
  {% if page.mermaid }
  {% include mermaid.html %}
{% endif %}
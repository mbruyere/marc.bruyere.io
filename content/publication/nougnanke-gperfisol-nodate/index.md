---
title: 'gPerfIsol: GNN-based Rate-Limits Allocation for Performance Isolation in Multi-tenant
  Cloud'

# Authors
# A YAML list of author names
# If you created a profile for a user (e.g. the default `admin` user at `content/authors/admin/`), 
# write the username (folder name) here, and it will be replaced with their full name and linked to their profile.
authors:
- Benoit Nougnanke
- Justin Loye
- Jean-Francois Baffier
- Simone Ferlin
- Marc Bruyere
- Yann Labit

# Author notes (such as 'Equal Contribution')
# A YAML list of notes for each author in the above `authors` list
author_notes: []

date: -01-01

# Date to publish webpage (NOT necessarily Bibtex publication's date).
publishDate: '2024-01-10T08:12:41.047140Z'

# Publication type.
# A single CSL publication type but formatted as a YAML list (for Hugo requirements).
publication_types:
- article-journal

# Publication name and optional abbreviated publication name.
publication: ''
publication_short: ''

doi: ''

abstract: Performance Isolation in Multi-Tenant Cloud Data Centers (MTCDCs) consists
  of a set of mechanisms to make sure tenants’ use of resources does not impact other
  tenants. In this context, traffic shapers and rate limiters are fundamental to addressing
  the challenges of performance isolation in MTCDCs, which include predictable performance
  as minimum bandwidth guarantees, tenants-level fairness, and optimal resource utilization.
  However, the classical linear programming process to find the optimal rates to apply
  does not scale in terms of computing time, especially with the huge number of nodes,
  dominated mainly by virtual machines in an MTCDC environment. Motivated by this
  observation, this paper introduces gPerfIsol, a novel Graph Neural Network (GNN)-based
  approach designed to find near-optimal rates allocation in near-real-time to ensure
  performance isolation in MTCDC. gPerfIsol’s key innovation leverages Heterogeneous
  GNNs to capture MTCDCspecific topological information and demand traffic matrix.
  Evaluations based on datasets generated through simulation demonstrate the effectiveness
  of gPerfIsol’s binary classification model with a precision score of 0.964 and a
  recall score of 0.973. Ultimately, gPerfIsol offers a promising solution for nearoptimal
  rate limit allocation for traffic shapers in multi-tenant environments, enhancing
  performance isolation.

# Summary. An optional shortened abstract.
summary: ''

tags: []

# Display this page in a list of Featured pages?
featured: false

# Links
url_pdf: ''
url_code: ''
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

# Publication image
# Add an image named `featured.jpg/png` to your page's folder then add a caption below.
image:
  caption: ''
  focal_point: ''
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects: ['internal-project']` links to `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: []
---

Add the **full text** or **supplementary notes** for the publication here using Markdown formatting.

---
layout: page
title: Code generation
permalink: /code-generation/
order: 1
share: false
---

Planck provides smart code generators. Its not just a filling templates with some data, Planck can grab info from your code and use it for code generation. For example after you add something to your router, Planck can create controller for this route, without any additional data from user. When you ```import``` model in your controller - Planck can create this model if it does not exists.

Code generators can be turn off in production environment with ```codeGeneration.autoGeneration``` option in your config file.

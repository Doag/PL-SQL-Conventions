---
layout: default
published: true
---

# Logging

## General

Logging is the process that handles all error related information.

Currently the best logging package is [Logger (OraOpenSource)](https://github.com/OraOpenSource/Logger). Best practices for using Logger can be found [here](https://github.com/Doag/DOAG-PL-SQL-Coding-Conventions/blob/gh-pages/articles/logging.md).

Logging is a balancing act, with clarity vs. completeness and simplicity vs. disk-space as mayor competitors.
- Clarity for feedback
- Completeness for analysis
- Simplicity to make sure that the decision to log or not does eat more performance than the action being logged
- Disk space wasted by irrelevant log records

## Best Practices


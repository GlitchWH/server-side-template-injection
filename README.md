# Server-Side Template Injection

Solutions  to the PortSwigger Web Security Academy server-side template injection (SSTI) labs, with custom Python exploit scripts and step-by-step walkthroughs.

## About

Server-side template injection (SSTI) happens when user input is placed into a template and the template engine evaluates it as code, instead of displaying it as plain text. An attacker can then run template expressions on the server, which can lead to reading secrets, reading files, or running system commands.

The risk depends on the engine. Some engines, such as Freemarker and Handlebars, expose enough of the Java or JavaScript runtime that a template can reach dangerous methods. Others, such as Django, mainly expose framework settings and debug information.

Each lab in this repository has its own folder containing:

- **Python exploit script:** automates the HTTP requests needed to deliver the payload.
- **Step-by-step walkthrough:** explains the template engine, the injection point, the payload, and how to prevent it.

## Labs

| # | Lab | Engine | Technique | Status |
|---|-----|--------|-----------|--------|
| 1 | Basic server-side template injection | ERB (Ruby) | `<%= ... %>` evaluation, then `system()` | ✅ Solved |
| 2 | Basic server-side template injection (code context) | Tornado (Python) | Closing `}}` and injecting `{% import os %}` | ✅ Solved |
| 3 | Server-side template injection using documentation | Freemarker | `new()` to create `Execute`, then run a command | ✅ Solved |
| 4 | Server-side template injection in an unknown language with a documented exploit | Handlebars | Fuzzing to identify the engine, then a published exploit | ✅ Solved |
| 5 | Server-side template injection with information leak | Django | `{% debug %}` to inspect context, then `settings.SECRET_KEY` | ✅ Solved |
| 6 | Server-side template injection in a sandboxed environment | Freemarker | Chaining allowed Java methods to read a file | ✅ Solved |
| 7 | Server-side template injection with a custom exploit | Custom (PHP-style user object) | Error-disclosed `setAvatar()` and `gdprDelete()` | ✅ Solved |



## Key Takeaways and Prevention

- **Never let users write template source.** Pass user data into a fixed template as variables.
- **Identify the engine before exploiting.** Fuzzing with template delimiters reveals the engine from its error messages.
- **Restrict what templates can reach.** Remove access to settings, reflection, file-handling methods, and system modules.
- **Treat sandboxes as a defense-in-depth layer, not a boundary.** Blocklists of method names are easy to bypass by chaining allowed calls.
- **Keep secrets out of template context.** Debug tags and settings objects should not be reachable from user-controlled templates.

## Disclaimer

This repository is created strictly for educational purposes and authorized security research. All exercises were performed within isolated PortSwigger Web Security Academy environments.
## Author
GlitchWH (Waqas Hussain)

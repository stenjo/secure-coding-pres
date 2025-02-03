---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: images/appsec-background.webp
# some information about your slides (markdown enabled)
title: Coding secure
info: |
  ## Building Antifragile Applications through Secure Coding using Typescript
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

<div class="absolute top-5">
  <span class="font-700">
    Sten Johnsen: 
    Bouvet One - March 2025
  </span>
</div>

<div class="absolute bottom-10">
  <h1>Secure coding</h1>
  <p>Building Antifragile Applications through Secure Coding using Typescript</p>
</div>
<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
layout: image
---

![Little Bobby Tables](/images/exploits_of_a_mom_2x.png){ w-full h-auto }


---
layout: image-right
image: images/sten.jpeg

---

# About me


---
layout: two-cols-header

---
# Antifragile apllicatons
::left::

<arrow  x1="350" y1="310" x2="195" y2="334" color="#953" width="2" arrowSize="1" />

::right::

Fragile:
- performance reduces under stress

Robust:
- performance constant under increasing stress

Antifragile:
- performance increases under increasing stress

---


# Perimeter security


---
layout: two-cols-header
---

::left::
![Secure by Design](/images/default-h1PakPUs-medium.jpg){ h-full w-auto }
::right::
# Secure by Design
by Dan Johnsson, Daniel Sawano, Daniel Deogun

Pocket 2019 Engelsk

Summary

Secure by Design teaches developers how to use design to drive security in software development. This book is full of patterns, best practices, and mindsets that you can directly apply to your real world development. You''ll also learn to spot weaknesses in legacy code and how to address them.
::center::

---
layout: image-right
image: public/images/Secure-Coding-Practices.jpg
transition: slide-left
---

# Traditional Application Security

- Security in separate activities
    - Threat modelling
    - Penetration tests
    - Security review
    - SAST
    - DAST

- DevOps:
    "Build Quality in" -> "Build Security in"


---
transition: slide-up
---

# Sanitizing input

## First line defence
```html
<input type="email" required pattern="^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$" />
```

## Restrict input format

```ts
const sanitizeInput = (value: string) => {
  return value.replace(/[^a-zA-Z0-9 ]/g, ""); // Remove special characters
};
```

## Strong form types

```ts
interface FormData {
  name: string;
  email: string;
  age: number;
}

const [formData, setFormData] = useState<FormData>({ name: "", email: "", age: 0 });
```

---
layout: fact

---
# References

- xkcd
- Secure by Design, 
- NDC Security, Jan. 2025

---
layout: center
---
# Questions?

---
layout: center
---

# Thank You
---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
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

# Coding Secure

Building Antifragile Applications through Secure Coding using Typescript

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
layout: image
---

![Little Bobby Tables](/images/exploits_of_a_mom_2x.png){ w-full h-auto }

---

# Antifragile apllicatons
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

References

Questions

Thank You
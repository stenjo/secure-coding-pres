---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /images/Secure-Coding-Practices.jpg
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
layout: two-cols-header

---
# Antifragile applications
::left::

!['Fragile vs Antifragile'](/images/Fragile_Antifragile.png) { w-full h-auto }

::right::


### Antifragile:
- performance increases under increasing stress


### Robust:
- performance constant under increasing stress

### Fragile:
- performance reduces under stress

Antifragile system: 
  A system that increases performance as a function of stress 

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
transition: slide-up

---

# Handling input data from URL

https:\//tubes.com/report?projectid=<span v-mark.underline.orange>c9b1e9b2-3f5d-4b1e-8b1e-9b2c9b1e9b2c</span>&pipeid=c9b1e9b2-3f5d-4b1e-8b1e-9b2c9b1e9b2c


<div v-click>
```ts
export const getPipeReport = async (pipeId: string, projectId: string): Promise<any> => {
    
    const response = await axios.get(`/api/pipes/${projectId}/report?pipeId=${pipeId}`);
    const data = response.data;
    return data;
};
```
</div>

<!--
  Alter projectId
- sql injection
- access to wrong project?
- parameter list

-->
---

# Security in depth 

Improve security and quality

````md magic-move {lines: true}
```ts {*}
const renderPipeReport = async () => {
    const projectId = getFromUrl('projectId');
    const pipeId = getFromUrl('pipeId');

    const data = await getPipeData(projectId, pipeId);
    // render the report
}
```

```ts {*}
const renderPipeReport = async () => {
    const projectId: string = getFromUrl('projectId');
    const pipeId: string = getFromUrl('pipeId');

    const data = await getPipeData(projectId, pipeId);
    // render the report
}
```

```ts {*}

type Guid = string;

const renderPipeReport = async () => {
    const projectId: Guid = getFromUrl('projectId');
    const pipeId: Guid = getFromUrl('pipeId');

    const data = await getPipeData(projectId, pipeId);
    // render the report
}
```

```ts {*}
type Guid = string & { readonly __brand: unique symbol };;

const renderPipeReport = async () => {
    const projectId: Guid = getFromUrl('projectId');
    const pipeId: Guid = getFromUrl('pipeId');

    const data = await getPipeData(projectId, pipeId);
    // render the report
}
```

```ts {*}
type Guid = string & { readonly __brand: unique symbol };;

const renderPipeReport = async () => {
    const projectId: Guid = getFromUrl('projectId') as Guid;
    const pipeId: Guid = getFromUrl('pipeId') as Guid;

    const data = await getPipeData(projectId, pipeId);
    // render the report
}
```

```ts {*}
const renderPipeReport = async () => {
    const projectId: Guid = new Guid(getFromUrl('projectId'));
    const pipeId: Guid = new Guid(getFromUrl('pipeId'));

    const data = await getPipeData(projectId, pipeId);
    // render the report
}
```
````

<!--

Using the parameters and passing it on to the api for fetching data. No type checking opens up for run-time errors and possibly invalid input 

[click] Specifying primary type will to some extent ensure we are handling a string and not an array, object or any other type of data.

- ✅ Explicit typing prevents unintended values.
- ✅ Catches potential runtime errors at compile-time.
- ✅ Ensures projectId always behaves like a string.

[click] Defining Guid as a string means exactly that. They are treated the same. 
No extra typechecking and no added safet althoug readability is improved as we can understand a Guid is expected.

[click]
Adding typechecking helps us make sure string and Guid is not interchanged. They are not treated the same.

This way we ensure that if getFromUrl() returns a string, we will have a compile-time error. 

[click]
Solution then is often just doing an explisit cast, and getFromUrl() can return a strng again.

Any validation of the type has to be done manually

[click]
Solution is defining a class

-->

---

# Security in depth 

```ts {*}
const renderPipeReport = async () => {
    const projectId: Guid = new Guid(getFromUrl('projectId'));
    const pipeId: Guid = new Guid(getFromUrl('pipeId'));

    const data = await getPipeData(projectId, pipeId);
    // render the report
}
```

Class implementation

````md magic-move {lines: true}
```ts {*}
export class Guid {
  protected value: string

  constructor (value: string) {
    this.value = value
  }
}
```
```ts {*}
export class Guid {
  protected value: string

  constructor (value: string) {
    this.value = value
    this.validate();
  }

  private validate(): void {
    const uuidRegex = /^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/i;
    if (!uuidRegex.test(this.value)) {
        throw new Error(`Invalid GUID format: ${this.value}`);
    }
  }
}
```
```ts {*}
export class Guid {
  protected value: string

  protected constructor (value: string) {
    this.value = value
    this.validate();
  }

  public static create(value: string): Guid {
      return new Guid(value);
  }

  private validate(): void {
    const uuidRegex = /^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/i;
    if (!uuidRegex.test(this.value)) {
        throw new Error(`Invalid GUID format: ${this.value}`);
    }
  }
}
```
````    
<!-- 
Defining a class makes:
- Even stronger typechecking
- variable immutable
- open up for additional validations

[click]
Adding a validation ensures that truly is Guid and adheres to the format requirements of a guid.

[click]
For even better encapsulation, define an explisit create method and keep the constructor protected

-->
---

# Security in depth 

```ts {*}
const renderPipeReport = async () => {
    const projectId: Guid = Guid.create(getFromUrl('projectId'));
    const pipeId: Guid = Guid.create(getFromUrl('pipeId'));

    const data = await getPipeData(projectId, pipeId);
    // render the report
}
```

Class implementation

````md magic-move {lines: true}
```ts {*}
export class Guid {
  protected value: string

  protected constructor (value: string) {
    this.value = value
    this.validate();
  }

  public static create(value: string): Guid {
      return new Guid(value);
  }

 ...
}
```
````    
<!-- 
For even better encapsulation, define an explisit create method and keep the constructor protected

-->
---

# Even stronger type checking

```ts {*}
const renderPipeReport = async () => {
    const projectId: ProjectGuid = ProjectGuid.create(getFromUrl('projectId'))
    const pipeId: PipeGuid = PipeGuid.create(getFromUrl('pipeId'));

    const data = await getPipeData(projectId, pipeId);
    // render the report
}
```
```ts {*}
export abstract class Guid {
    protected constructor(value: string) {
        super(value);
        this.validate(); // ✅ Ensure validation is applied
    }

    protected validate(): void {
    ...
    }

    public static create<T>(this: new (value: string) => T, value: string): T {
        return new this(value);
    }
}
```


<!--

-->

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

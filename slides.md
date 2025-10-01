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
    HelloStavanger - October 2025
  </span>
</div>

![Little Bobby Tables](/images/exploits_of_a_mom_2x.png){ w-full h-auto }

<br/>

<div class="absolute bottom-10">
  <h1>Secure coding</h1>
  <p>Building Antifragile Applications through Secure Coding using Typescript</p>
</div>

<!--

These slides are prepared for HelloStavanger use, but feel free to access, use and be inspired at any time

-->


---
layout: default
transition: slide-up

---

# Sten Johnsen

A tech geek spending his work and spare time figuring out stuff involving electronics and software


- **Bouvet** - Since 2008, currently team-lead and (full-stack) developer
![Sten](/images/Sten_Johnsen_6879.JPG?url) {width=200px margin=30px align=right}

- **Experience** - Graduated 1991 - B.Eng Microelectronic computer systems. Programming since my first real job - never looked back.
  
- **Roles** - Programmer, project manager, program manager, department head, entrepreneur, agile coach and relationship counsellor

- **Trainer** - DevOps certification courses, Agile, Scrum

- **Busy with** - Quality of software and creating high performing teams


---
layout: two-cols-header

---

# Topics of today

::left::

## Antifragility

<br/>

## Perimeter security <br/>(input sanitation)
  
<br/>
  
## Security in design

::right::

## Domain primitives

<br/>
  
-  ### What

<br/>
  
-  ### Why

<br/>
  
-  ### How (code examples)

<br/>
  
## some experiences and recommendations



---
layout: two-cols-header

---

::left::
![Antifragile](/images/default-oVU8aL0x-medium.jpg){ h-full w-auto }
::right::

# Antifragile
## Things that Gain from Disorder
By Nassim Nicholas Taleb
![Taleb](/images/nassim-nicholas-taleb-1221.jpg) {width=200px margin=30px align=right}

Pocket 2013 English

The antifragile is beyond the resilient or robust. The resil­ient resists shocks and stays the same; the antifragile gets better and better.
Just as human bones get stronger when subjected to stress and tension, many things in life benefit from stress, disorder, volatility, and turmoil. What Taleb has identified and calls antifragile are things that not only gain from chaos but need it in order to survive and flourish.

::center::

---
layout: two-cols-header
layoutClass: gap-16

---

# Antifragile applications

::left::

!['Fragile vs Antifragile'](/images/Fragile_Antifragile.png) { w-full h-auto }

::right::


## Antifragile:
- performance increases under increasing stres

## Robust:
- performance constant under increasing stress

## Fragile:
- performance reduces under stress

<!--
Antifragility is when the system becomes stronger when more stress is applied. Our own body is an antifragile system. When we train, we actually are damaging the muscle cells a little, an our body rebuilds them but just a little stronger than they were.

For an application or an IT system, stress is user load, unusual ways of using the application, hacking attempts, changing package versions and all the other stuff that might contribute to the system breaking down

The 'healing' process is the routines, the practices, the speed and the general mindset in changing the system.

-->


---
layout: two-cols-header
---

::left::
![Secure by Design](/images/default-h1PakPUs-medium.jpg){ h-full w-auto }
::right::
# Secure by Design
by Dan Johnsson, Daniel Sawano, Daniel Deogun

Pocket 2019 English

Summary

Secure by Design teaches developers how to use design to drive security in software development. This book is full of patterns, best practices, and mindsets that you can directly apply to your real world development. You''ll also learn to spot weaknesses in legacy code and how to address them.
::center::

<!--

Inspired by this book we decided to go for Domain Primitives as part of our secure architecture when building our next application

-->

---

# What are Domain primitives?

Let's dive in and set the context



---
layout: two-cols-header
transition: slide-left

---

# Traditional Application Security

Many ways of increasing and maintaining the security of the application

::left::

## Security in separate activities
 
- Threat modelling

- Penetration tests

- Security review

- SAST

- DAST

## DevOps:
    "Build Quality in" -> "Build Security in"
::right::

![DevOps](/images/Devops-toolchain.svg.png)

::center::

<!--
Even in DevOps we teach that security often are particular activities like 
- Threat modelling
- Penetration tests
- Security reviews
- Static application security tests
- Dynamic application security test

All these activities are good and needed but if we do not keep an architecture helping us maintain and improve security, we end up with a fragile - or at best - a robust application.

We aim higher than that!

Our understanding of "build Quality in" is, among other things, a good architecture.

-->
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

<!--
Consider some frequently used examples of input sanitation.

first: input pattern validation in form. -> Can be bypassed by a simple form post

Second: sanitizing functions. -> better, but has to be implemented for all inputs

third: restricting the types of input -> tied to a specific form - other ways of inputing the same data?

But the type restrictions can be made stronger!

-->

---
transition: fade

---
Practical example:

# Handling input data from URL

https:\//tubes.com/report?projectid=<span v-mark.underline.orange>c9b1e9b2-3f5d-4b1e-8b1e-9b2c9b1e9b2c</span>&pipeid=c9b1e9b2-3f5d-4b1e-8b1e-9b2c9b1e9b2c


<div v-click>

```ts

// api

export const getPipeData = async (pipeId: string, projectId: string): Promise<any> => {
    
    const response = await axios.get(`/api/pipes/${projectId}/report?pipeId=${pipeId}`);
    return response.data;
};

```
</div>

<div v-click>

`guid`s are often used as lookup keys and some times used unsanitized in an SQL query!

What could go wrong?

</div>

<!--

Consider a frontend url for rendering a report of a pipe of some sort.

[click]
Recognize the query string parameters `projectId` and `pipeId`. Typically represented as strings.

[click]
Translates to something like this api/backend call

[click]
Without security i mind - this could open up for:

- sql injection
- access to wrong project?
- X site scripting
- IDOR (Insecure Direct Onbject Reference) -> authentication bypassing
- duplicate keys
- business logic abuse

by altering the projectId

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

When rendering the report we are using the parameters and passing them on to the api for fetching data. 

Without type checking this opens up for run-time errors and possibly invalid input 

- Did you notice that getPipeData() call has swapped parameters?

[click] Specifying primary type will to some extent ensure we are handling a string and not an array, object or any other type of data.

- ✅ Explicit typing prevents unintended values.
- ✅ Catches potential runtime errors at compile-time.
- ✅ Ensures projectId always behaves like a string.

[click] Defining Guid as a string means exactly that. They are treated the same. 
No extra typechecking and no added safety although readability is improved as we can understand a Guid is expected.

[click]
Adding typechecking helps us make sure string and Guid is not interchanged. They are not treated the same.

This way we ensure that if getFromUrl() returns a string, we will have a compile-time error. 

[click]
Solution then is often just doing an explisit cast, and getFromUrl() can return a string again.

Any validation of the type has to be done manually

[click]
Solution is defining a class

-->


---
transition: fade

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

Making Guid an abstract class enables us to create domain primitives that are not interchangeable
and may help us keep the parameter orders intact.

Swapping places of `pipeId` and `projectId` now, would create a linting error!

-->

---
layout: two-cols-header
transition: fade

---

# Why Domain Primitives?

::left::

<div v-click>

### Type Safety

Makes intent explicit. In a function call with two or more parameters, you can't pass `projectId` and `pipeId` in the wrong order.

</div>

<br/>
<div v-click>

### Validation at boundaries

Single-source-of-truth on what is valid data.

</div>


::right::

<div v-click>

### Self-documenting code

It's easy to see what type of data the variable contains.

</div>
<br/>

<div v-click>

### Domain logic

Domain rules and logic can live inside the primitive, I.e. 
```ts 
myId.equals(someOtherId); 
``` 
checks both the actual value and the type.

</div>

::bottom::

<!--

[click] obvious

[click] obvious

[click] obvious
 
[click] We like to use domain primitives as part of our data models, where conversion to and from json is encapsulated.
-->

---
layout: two-cols-header

---

# Why Domain Primitives? (2)

::left::

<div v-click>

### Consistency (across systems)

Prevents duplication and inconsistent behaviour 
<br/>
across services. 
<br/>
Same code used both in frontend 
and backend.

</div>

<br/>

<div v-click>

### Testability

A primitive is isolated and testing behaviour (validation, conversion, comparison) 
<br/>
independantly is easy.

</div>

<br/>
::right::

<div v-click>

## Refactoring safety

If you decide to make all guids lowercase, you only need to do this in one place - not everywhere

</div>

<br/>


<div v-click>

### Integration with richer patterns

Combines well with DDD concepts like Entites, Value Objects and Aggregates. Keeps things focused and reduces bugs

</div>

<br/>
::bottom::

<!--

[click] Using the same code makes sure we have the same data

[click] The data object will not exist unless it validates. 

[click] obvious
 
[click] Encapsulation keeps things tidy and easy to test
-->

---

# Pitfalls

<br/>

<div v-click>

### 1. Compile-time safety, zero runtime safety
 
| **Pitfall:** | Protection only at compile time. Invalid data can still enter.|
|---------------|-----------------------|
| **Mitigate:** | Always validate at boundaries (DTO → domain). <br/>Keep `create()` (throws) and `tryCreate()` (safe) factories and use them consistently.|

</div>

<br/>

<div v-click>

### 2. “Primitive proliferation” and ceremony

|**Pitfall:** | Making a primitive for everything explodes the type count and boilerplate, slowing teams down.|
|---------------|-----------------------|
|**Mitigate:** | Create primitives only where rules/meaning matter (IDs, money, percentages, time, email, URLs). Use plain types for purely mechanical values.|

</div>

<!--
Are there pitfalls? Yes indeed!

I'll let you know about our top 4.

[click]
Domain primitives are brilliant as a detector of possible bugs. Make sure to handle run-time data errors in a proper way. Don't let your application crash because an object property was null when your validation expects it not to be.

[click]
Keep an eye on the benefits of creating a domain primitive. Some times as number is just a number and might not need any particular validation. Use data types to your benefit on the level that suits the need.

-->

---


# Pitfalls

<br/>

<div v-click>

### 3. Error-handling strategy mismatch

| **Pitfall:** | Throwing in create leaks exceptions into hot paths; returning undefined hides errors.|
|---------------|-----------------------|
| **Mitigate:** | Offer both:<br/>`create()` (throws) for tests/internal invariants<br/>`tryCreate()` (returns T \| undefined \| Result) for boundaries<br/>Pick one per layer and be consistent.|

</div>

<br/>

<div v-click>

### 4. Test overhead

| **Pitfall:** | Every primitive adds fixtures/mocks and increases test churn.|
|---------------|-----------------------|
| **Mitigate:** |Provide test helpers (validGuid(), email()), and property-based tests for core primitives to cover many cases once.|

</div>


---
layout: two-cols-header
transition: fade

---

# What we learned


::left::

<div v-click>

- ## Always catch your exceptions

</div>

<br/>

<div v-click>

- ## Make your domain primitive easy to use

</div>

::right::

<div v-click>

- ## Be strict when reviewing

</div>

<br/>

<div v-click>

- ## Use any bugs to make your application stronger

</div>
::bottom::


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
layout: fact

---

# Thank You


## ![Presentation](/images/QR.png) {width=300px margin=30px align=right}

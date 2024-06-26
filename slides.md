---
# try also 'default' to start simple
theme: ./theme
themeConfig:
  primary: '#d01c24'
colorSchema: light
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://images.unsplash.com/photo-1576675466684-456bcdeccfbf?q=80&w=3540&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
# apply any unocss classes to the current slide
class: "text-center"
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# some information about the slides, markdown enabled
info: |
  ## Fuji-web Talk
  Fuji-Web is an intelligent AI partner that understands the user’s intent, navigates websites autonomously, and executes tasks on the user’s behalf while explaining each action step.
transition: slide-left
title: Introducing Fuji-Web
mdc: true
hideInToc: true
---

<h1 class="title-white">Introducing Fuji-Web</h1>

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Start the tour <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-1">
  <a href="https://blog.normalcomputing.ai/posts/2024-05-22-introducing-fuji-web/fuji-web.html" target="_blank" alt="Blog Post" title="View blog post"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-blog />
  </a>
  <a href="https://github.com/normal-computing/fuji-web" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
Good luck with your presentation! You can do this! 🚀
-->

---
layout: default
hideInToc: true
class: gradient-header
---

# Table of contents!

<Toc maxDepth="1"></Toc>

<!--
We will talk about these topics today.
-->


---
layout: default
class: gradient-header
---

# Demo

<SlidevVideo v-click controls style="width: 80%">
  <source src="https://storage.googleapis.com/normal-blog-artifacts/fuji-web/FujiWebFinal.mp4" type="video/mp4" />
  <p>
    Your browser does not support videos. You may download it
    <a href="https://storage.googleapis.com/normal-blog-artifacts/fuji-web/FujiWebFinal.mp4">here</a>.
  </p>
</SlidevVideo>

---
layout: default
class: gradient-header
---

# Fun facts about Fuji-Web

- 🥇 **First of its kind** - Fuji-Web is the first ever web agent based on multi-modal model (first PoC demo on Nov 7 2023)
- 🏆 **State-of-the-art** - Fuji-Web achieved highest scores in a benchmark measuring web agent's ability to perform real-world tasks
- ⭐ **Star collector** - Over 100 GitHub stars in the first 24 hours of open-source release
- 👨‍🦯 **Accessible** - Fuji-Web has built-in features to make it a useful tool for visually impaired people
- 🔑 **Captcha Cracker** - Fuji-Web can often solve text-based Captcha challenge with a few tries


---
layout: section
class: gradient-header
---

# Motivation

<!--
Now, why did we build Fuji-Web?
-->

---
layout: two-cols
---

<h2 class="normal-title-solid">Product | <img src="/normal-logo.png" width="100px" style="display: inline-block; margin-top: -2px" /></h2>

We’ve been enabling complex workflow augmentation for some of the most high-stakes industrial and advanced manufacturing applications around the globe.

<br>
<br>

- We believe AI agents will become highly capable assistants and transform how we interact with computers 
- We want to create an end-to-end system that can work across often clunky internal enterprise tools and diverse web environments (Jira, internal wikis, operator notes, SOPs, and specifications)

::right::

<img src="/product-1.jpg" class="ml-2" />
<img src="/product-2.jpg" class="mt-1 ml-2" />

---
layout: default
---

<h2 class="normal-title-solid">Research | <img src="/normal-logo.png" width="100px" style="display: inline-block; margin-top: -2px" /></h2>

We have demonstrated the challenges of attaining the level of sophisticated reasoning and calibrated uncertainty-awareness required for decision-making in these high-stakes environments.

<img src="https://storage.googleapis.com/posteriors/docs_landing.png" width="50%" style="mix-blend-mode: multiply;" />

<br>
<br>

- We want our AI to be able to act after it _reasons_
- We want to understand the challenges in building AI systems working in complex environments


---
layout: section
hideInToc: true
---

# Chanllenges, Solutions & Learnings

<!--
Next, let's talk about the challenges we faced, the solutions we came up with, and the learnings we had.
-->

---

## The architecture of Fuji-Web

<img src="/tech-design.png" />

<!--
Before we dive into the challenges, let's take a look at the architecture of Fuji-Web.

All of its code is written in TypeScript. React-based UI connected to the browser's extension API. It directly connects to the LLM API as its only "backend".

When user start a task from the UI, the agent will gather information about the current web page through an observer component. It then talks to the LLM, decide what action to take, and then operate the web page through the actuator component. It repeats this loop until the task is done.
-->

---

# Challenge 1: Building Chrome Extension 

<div v-click>
We'll skip this part for now 😢
</div>

<v-click>

But why not use browser automation software (Puppeteer, Playwright, Selenium)?

- Chrome extension is easier for users to install
- Automation softwares are treated as bots by websites
- Users can use their own Chrome profiles: exsiting sessions, cookies, etc.
- Users can use it anywhere anytime


</v-click>

<!--
The first challenge we had was to build it as a Chrome Extension. In the diagram you just saw, the components are clearly separated. In a real chrome extension, the communication between different parts can be very tricky. Your code needs to run in at least 3 worlds (the web page, side panel UI, service worker), each of them has its own capabilities and limitations. 

What is worse, the documentation is often very outdated. 

I don't think you are here to listen to me complain today. So let's skip the details.

But you may ask, why does it have to be an extension? Most other web agent implementation is using browser automation software.
-->

---
hideInToc: true
---

# Challenge 2: How to... make it work?

- Prompt engineering?
- Agent Framework?
- Fine tuning?
- <del>Train our own model?</del>

<!--
The second challenge: how to make the agent work? There are a lot of buzz words about how you would use LLMs. Obviously I don't have to GPU to train my own model in this case, so we can cross that out.

The real questions, how do we think about this problem
-->

---

## Two main approaches

<div v-click.hide>
  <img src="/optimize-llm-openai-talk.jpg" width="80%" />
</div>
<div v-after>
  <img src="/optimize-llm-openai-talk-2.jpg" width="80%" />
</div>

Source: [A Survey of Techniques for Maximizing LLM Performance](https://www.youtube.com/watch?v=ahnGLM-RC1Y&t=254s) -- OpenAI

<style>
  .slidev-vclick-hidden {
    display: none;
  }
</style>

<!--
I want to share this diagram I took from a talk OpenAI published on YouTube. It describes two main approaches you can think about when you optimize LLM performance: Context optimization and LLM optimization. Fine-tuning is LLM optimization, RAG is more context optimization, Prompt engineering is kinda of both. Apparently, these are NOT the only methods you can use, not even for Q&A applications.

Next, I'd like to share what I built in Fuji-Web on both directions.

Let's start with context optimization.
-->

---

# Challenge 2: Help LLM understand the web page (Context optimization)

- GPT-4V can understand a screenshot of a web page & read its content
- But it cannot tell us which part of the web page to interact with
  - It can give us the coordinates, but they are often incorrect (hallucination)
  - It can give us the text on the web page, but it is not enough to identify the exact element

<!--
Now, here's the real second challenge.

After some experiments on ChatGPT, we can find out that...

...

For example, you might tell you to click "like" button, but there might be multiple "like" button visible on the page. Also, many buttons use icons instead of text, like a bell icon means notifications.
-->

---
layout: default
---

## Focus on the actions

<v-clicks>

- Basic actions/tools: click, type, scroll, etc.
- How to segment the web page to find interactive elements? 
- How to annotate the segments?

</v-clicks>

<!--
Since GPT-4V has no problem reading the content from the screenshot, we only need to focus on the actual actions we need the agent to generate. There are not many of them: clicking, typing and scrolling are the most common ones. 

Clicking usually only works on buttons and links, and typing on inputs. How do we find them?

After we find them, in what form can we tell the LLM about it?
-->

---
layout: default
---

### Segmentation based on HTML semantics

Use HTML semantics and WAI-ARIA roles to identify these interactive components accurately

| Website      | Elements found with interactive HTML tags | Elements found with interactive HTML tags + WAI-ARIA roles |
|--------------|--------------------------------------------|------------------------------------------------------------|
| amazon.com   | 534                                        | 547                                                        |
| twitter.com  | 56                                         | 121                                                        |
| github.com   | 1364                                       | 1446                                                       |

<!--
There are some interesting recent work on vision based segmentation on UI screenshots. But because we are working on web, there's a unique advantage: we have direct access to HTML, and HTML has semantics and ARIA roles. We can find interactive elements with them.
-->

---

### Getting information for icon-only buttons

- Use the `aria-label` attribute
- Use `<label>` elements, `name` and `placeholder` attributes of input elements

<!--
Similarly, we can gather additional information for those elements, especially when a button is icon-only.
-->

---
layout: default
---

### Overlay style annotation (Set-of-Mark)

- Overlay style annotation is more intuitive
- However, it can cover the content on the web page

<img src="https://som-gpt4v.github.io/website/img/teaser.png" width="70%" />

<!--
Now that we've found the interactive elements, how do we communicate with the LLM about them?

You might have heard of this prompting technique for vision language models called set of mark prompting. The idea is you can just trace the border of an object and write a label on it. Then the LLM can talk back to you with the label. Pretty smart right?

However, on web page it can cover the content. For small buttons, the text cannot be seen and the LLM cannot tell what it does!
-->

---
layout: default
---

### Tooltip style annotation

- Less intrusive, but can cause confusion
- Can still block the content

<img src="/tooltip.jpg" width="80%" class="mt-2" />

<!--
I also experimented with this kind of tooltip style annotation. Good news: it does not cover the original button. Bad news: it sometimes cover the content nearby.
-->

---
layout: default
---

### UFO-inspired style annotation

- [UFO(**U**I-**Fo**cused agent for Windows)](https://arxiv.org/pdf/2402.07939) is a paper published by Microsoft Research in Feb 2024

<img src="/ufo-input.jpg" width="80%" class="mt-2" />

<!--
Eventually, this UFO paper from MicroSoft gave me an idea. If the annotation covers the content, we can send a clean screenshot along with it!

It's very simple now that we talk about it, but it was truly a breakthrough for Fuji when I found this paper.
-->

---

### Example of image annotation

<img src="/annotation-example.png" width="95%" />

---

### Example of control information in text context

```
label = 4
name = Home
tagName = A
role = link
===
label = 5
name = Search and explore
tagName = A
role = link
===
label = 6
name = Notifications (1 unread notification)
tagName = A
role = link
```

---

### Keeps irrelevant elements off the list

Context optimization is:

- What the model needs to know
- What the model **does not** need to know

<img src="https://storage.googleapis.com/normal-blog-artifacts/fuji-web/x-side-by-side.jpg" width="80%" />

_(Left: without “top-layer element only” filter. Right: with “top-layer element only” filter)_


---

# Challenge 3: Build an agent (LLM optimization)

- Agent Framework ([LLM agent introductions](https://www.promptingguide.ai/research/llm-agents))
- Guide the model to generate the desired output (not [outlines](https://github.com/outlines-dev/outlines))

---

## Fuji-Web's Agent logic

- We use a customized ReAct agent in Fuji-Web
- ReAct is a simple framework based on a loop of observation, reasoning, and action
- It naturally fits how we interact with the web: observe the web page, reason about the context, and take actions

<v-click>
<br>

- Why no planning?
  - Without further training or context, planning can be hard
  - The Intenet is a rather unpredictable place
  - There is no easy way to restore a previous state
  - We might want to use a different strategy (outside of Fuji) for more complex workflows

</v-click>

---

## Fuji-Web's JSON mode

- One-shot response example
- Prefilling responses for JSON format ([Anthropic's introduction](https://docs.anthropic.com/en/docs/prefill-claudes-response))
  - It works well with **any** LLM model... and can outperform the model provider's own JSON mode??!
  - Can be used to further guide the model to generate the desired output with particular structure


---

### Prefilling Example: JSON mode

````md magic-move
```typescript{all|6-11,19-21}
// ...
messages.push({
  role: "user",
  content,
});
if (params.jsonMode) {
  messages.push({
    role: "assistant",
    content: "{",
  });
}
const completion = await openai.chat.completions.create({
  model: model,
  messages,
  max_tokens: 1000,
  temperature: 0,
});
let rawResponse = completion.choices[0].message?.content?.trim() ?? "";
if (params.jsonMode && !rawResponse.startsWith("{")) {
  rawResponse = "{" + rawResponse;
}
// ...
```
```typescript{1,7-12,20-23}
const prefillText = "{\n  \"thought\": \""; // expected ReAct response in JSON
// ...
messages.push({
  role: "user",
  content,
});
if (params.usePrefill) {
  messages.push({
    role: "assistant",
    content: prefillText,
  });
}
const completion = await openai.chat.completions.create({
  model: model,
  messages,
  max_tokens: 1000,
  temperature: 0,
});
let rawResponse = completion.choices[0].message?.content?.trim() ?? "";
if (params.usePrefill && !rawResponse.startsWith(prefillText)) {
  rawResponse = prefillText + rawResponse;
}
// ...
```
````

---

# Challenge 4: Maintain code quality

- Agent development can be hard and slow since we don't have a (good) existing framework
- We need to maintain code quality and ensure the system is robust

---

## A type-safe, robust system to provide agent tools

### Schema definition

```typescript
// define tools
// matches a JSON like this: {name: "setValue", args: {label: "17", value: "Joe Smith"}}
export const setValueSchema = z.object({
  name: z.literal("setValue"),
  description: z
    .literal(
      "Focus on and set the value of an input element with the label on the annotation.",
    )
    .optional(),
  args: z.object({
    label: z.string(),
    value: z.string(),
  }),
});
// ...

// define the union
export const toolSchemaUnion = z.discriminatedUnion("name", [
  clickSchema,
  setValueSchema,
  // ...
]);
export type ToolOperation = z.infer<typeof toolSchemaUnion>;
```

---

### Parse the JSON response to ensure correct format

You can parse the JSON response from the model:

```typescript
try {
  operation = toolSchemaUnion.parse(response.action);
} catch (err) {
  const validationError = fromError(err);
  // user friendly error message
  throw new Error(validationError.toString());
}
```

Then you can:

```typescript
switch (operation.name) {
  case "scroll":
    await scroll(domActions, action.args.value);
  case "click": {
    const success = await click(domActions, action.args.label);
    if (!success) {
      console.error(
        "Unable to find element with label: ",
        action.args.label,
      );
    }
    break;
  }
  // ...
  default:
    console.error("Invalid action name", action);
}
```

---

### Tool descriptions extract from schema to use in context

```
Name: click
Description: Click on an element with the label on the annotation.
Arguments:
  - label (string)

Name: setValue
Description: Focus on and set the value of an input element with the label on the annotation.
Arguments:
  - label (string)
  - value (string)
```

---

# Benchmarks

We compared Fuji-Web’s ability to successfully complete real-world tasks to [WebVoyager](https://arxiv.org/abs/2401.13919) using their proposed benchmarks. As of today, we have finished running and evaluating the tasks on 7 websites, and observe compelling quality and performance. Results are shown in the following table:


<table class="table table-bordered">
  <tr>
   <th>
   </th>
   <th>Allrecipes
   </th>
   <th>ArXiv
   </th>
   <th>Apple
   </th>
   <th>Google Search
   </th>
   <th>BBC News
   </th>
   <th>GitHub
   </th>
   <th>Cambridge Dictionary
   </th>
  </tr>
  <tr>
   <th>GPT-4 (All Tools)
   </th>
   <td>11.1% 
   </td>
   <td>17.1% 
   </td>
   <td>44.2%
   </td>
   <td>60.5%
   </td>
   <td>9.5%
   </td>
   <td>48.8%
   </td>
   <td>25.6%
   </td>
  </tr>
  <tr>
   <th>WebVoyager
   </th>
   <td>53.3% 
   </td>
   <td>51.2%
   </td>
   <td><strong>65.1%</strong>
   </td>
   <td>76.7%
   </td>
   <td>61.9%
   </td>
   <td>63.4%
   </td>
   <td>65.1%
   </td>
  </tr>
  <tr>
   <th>Fuji-Web
   </th>
   <td><strong>64.4%</strong>
   </td>
   <td><strong>65.1%</strong>
   </td>
   <td>60.4%
   </td>
   <td><strong>81.4%</strong>
   </td>
   <td><strong>76.2%</strong>
   </td>
   <td><strong>73.2%</strong>
   </td>
   <td><strong>86.0%</strong>
   </td>
  </tr>
</table>

<figcaption class="figure-caption">Table: The main results for Fuji-web. GPT-4 (all tools) and WebVoyager success rates are reported in the WebVoyager paper (last revised Feb 29 2024, using GPT-4V model). Fuji-web was benchmarked using the GPT-4o model.</figcaption>


---

# Roadmap

- Expose API for easy integration with browser automation frameworks (e.g. Puppeteer, Playwright, Selenium)
- Add support for more complex & cross-tab workflows
- Add support for more browsing behaviors (select from dropdown, extract content from entire page etc.)
- Add support for saving workflows
- Add support for sharing workflows & instructions with others
- Create wikipedia-like knowledge base where users can work together to create knowledge that can improve the Fuji-Web's performance

---
layout: center
class: text-center
---

# Questions?

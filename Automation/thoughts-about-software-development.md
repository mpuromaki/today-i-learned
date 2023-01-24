# Thoughts about Industrial Automation Software Development

## Introduction

My idea for this article is to gather my experience in Automation Engineering under one article. I'll
focus on different areas of Automation Engineering, describe issues I've seen and explain my thinking
around fixing those issues.

First we should look into Automation Engineering and notice how it contains many other professions.
It is normal for Automation Engineer to work daily with mechanical/pneumatic/hydraulic designs,
dabble in electrical designs and schematics and of course, write software. In many cases Automation
Engineer is in the frontline of customer communications and relations.

Usually though, atleast in larger companies, there are different people or departments for mechanical
and electrical design work, while management layers handle communications with customers. This means
that a lot of the Automation Engineers workload, atleast in my experience, is around Software
Development.

Even for software-focused work knowledge is required in multiple areas like (but not limited to)
basic physics and industrial processes, software development, automation specifics like industrial
protocols and basics of how Programmable Logic Controllers work. Nowadays industrial systems are
complex IT-systems, so knowledge of networking and administration of Windows machines are also pretty
much required. In addition to these basics are domain specific knowledge, which is defined more
closely by the industry and what kind of projects Automation Engineer is working on. One of these
domain specific things is, sadly, the software engineering tools themselves. Even when IEC 61131-3
tries to specify how automation software should look and feel, almost every tool is very different
to effectively use. Some automation systems even use their own languages instead of the ones defined
in standards. Furthermore these tools are not usually that polished and contain many bugs, wasting
engineers' time.

All this usually means that not enough time is dedicated for software development skills, not in
schools and not by the employers. We as an industry have come a long way from old relay based control
systems. Most of the hard problems were solved mechanically back then, but when we look at modern
industrial automation it is easy to notice how Automation Engineering companies have really become
software development companies. Solutions to problems in controls and processes are routinely
driven to be solved by software. Yet somehow it feels that these same companies have not really
realized that they are, in fact, software development companies.

It is very rare to see Version Control Systems or Bug Trackers used in these companies, even when
they are available. Software is not designed (as utilizing architectures and principles defined
by our colleagues in computer world), instead more rules are added to AND and OR blocks almost
pseudorandomly until the software "works". Software is not documented, and management nor the customer
require documentation for software. Documentation required is around the processes and UI, and those
documents might really not be written on the basis of how the software actually works.

Is this really Engineering?

### Table of Contents

1. [Project Management](#project-management)
2. [Software Engineering](#software-engineering)
3. [Documentation](#documentation)

## Project Management

Main issue in Project Management for Industrial Automation Software development is very similar to
project management issues in computer software development. Basically it boils down to three points:

1. Project Manager has no visibility into how the software development is going.
2. Even the Automation Engineers do not know how long it takes to develop software.
3. "Oh yes, there will be changes!"

These create normal but problematic situations on every software development project. Some parts of
this issue is accentuated in industrial automation context, where the whole projects are very waterfall
-like, mostly due to having to synchronize multiple professions and companies alongside their
subcontractors. Checkpoints for software development progress are defined either as "everything works in FAT"
or "software is done 50% / 100%". And even these imprecise checkpoints are mainly for billing purposes.

PM might not have any tools to properly view the status of the software development and even the Automation
Engineers can not give out any meaningfull estimates. Based on how and what technical practices are used
in the software development, the development time required for "functionality" might slow down immensely
even in the timespan of one project. If there is software left to do for only one subprocess, it might mean
cascading bugs and changes in every other part of the code in problematic code base.

So what can the PM do in this kind of situation? "Try to make it work" is not really that effective,
introducing new people in to the software development team will only slow things further for the next
12 months. Pushing from the PM without actual context of what parts of the software are really necessary,
and what are not, will lead to Automation Engineers "shipping shit" to "increase speed" (to quote Robert C.
Martin). This is not really something anyone in the project wants, and it will only make the later parts
of the project suck even more.

### How to fix these issues?

Robert C. Martin ("Uncle Bob") talks that the PMs have couple of adjustment knobs:

* Quality
* Time to market
* Cost
* Doneness

Lowering quality in industrial automation software projects should not really be an option, so it leaves
us with three choises. When project is clearly becoming late the PM can talk with customer and get them
agree to being late, employ more people to the project in last ditch effort to make the date or somehow
get the customer to agree to shipping partial project.

Problem with all these fixes is that the customer will not like these options on the eleventh hour of the
project. So to really have an option to leverage these tools, the PM would need information on the sad
state of the software development as early as possible. One great option is the early Agile practices.

Agile, in this context, basically means that the software development project is divided into small tasks.
PM, alongside customer when required, decide what tasks are most valuable to be implemented next. Cyclically
these tasks are completed by the automation engineers, tasks are adjusted as more information comes in, and
new next tasks are selected. This workflow does not impede the software development, but it allows the PM
do their job way more effectively. All this of course requires that the automation engineers follow some
technical practices, which we'll talk about later in this article.

Looking at the pile of tasks, and how slow they're consumed, the PM knows early if there will be issues
with projects timeline. The PM and customer can then early affect the software development process by
choosing what tasks are valuable and also decide together what level of Doneness is good enough for the
date. Any new functionality required by the customer, or any changes to the scope of the project, will
be added as tasks to the pile. This means that PM has clearer picture on how the project has grown and
even might have better case for extra billing. The customer is forced to think more about their changes
and additions, which helps to reduce pointless work.

The PM also can see early if the development process has stagnated for some reason and arrange
required help/resources before the automation engineers totally burn out.

## Software Engineering

I already talked a bit about issues on software development side in the introduction.

As a some kind of philosophical guiding principle, each party of an industrial automation software project
should acknowledge that software was created because it is easy to change. Software was selected as a
solution because it is easy to change. Customer or the process designer might not really know everything
at the start of the project. Thus software should be architected in a way which expects changes and which
is easy to change. APIs must be defined, so there is "contract" to which side can adhere to.

Different functionalities need to be decoupled so that it is easy to add selection for different kinds
of control strategies in the future. This decoupling also allows much easier testing, and even some tools
might support automated testing of function blocks.

Some things to consider:

* "Expecting Professionalism" by Uncle Bob.
* "Clean Code" series by Uncle Bob.
* Agile principles
* SOLID principles.
  * SOLID is oriented more towards object-oriented software, which largely industrial automation isn't.
  * But the principles can be used to refer functions and/or function blocks instead of classes.
  * Single responsibility principle is essential. Software is made to be chanced, there will be changes.
    Create the software in such a way that it is easy to change.
  * Interface segregation is important in the internal and external APIs of industrial automation software.
    There are many different communication and control interfaces to HMIs, other automation systems.
* Basic programming patterns like State Machine.
  * State Machines are essential for Industrial Automation. These can be used to solve something like
    80% of all issues in this kind of software.

Robert C. Martin ("Uncle Bob") might be a bit controversial on some issues, but the main points in his
talks are important. Professionalism, ethics and quality is very important in industrial automation,
as this kind of software has always consequences in the real world. One software bug could really kill
someone. This should be taught in schools and by the employers. It needs to be really hammered down to
new hires minds.

If your software development group is small and mostly senior level employess, they should be able to
figure out these technical practices and tooling by themselves.

On the other hand if there is larger amount of new unexperienced employees some additional work is needed.
In this case technical group-level leadership role is really important. This kind of software development
group needs some authority to guide the group in right direction and make sure that technical practices
are executed properly. For the first year or-so of new hires pair programming between senior automation
engineer and the new hire should be considered. This would be very effective training tool to get the
new employee up to speed, to learn required tools and practices. In any case senior level engineers
should have some kind of time budget to be used in pair programming / internal consulting / teaching.
**For the company it is very important that this technical knowledge does not get siloed.**

### Tools

Tools for version control, bug tracking and automated testing do not seem to be used in industrial
automation software development, even if nowadays these tools are usually available in the development
environment. These tools are not taught in automation engineering schools.

Version Control tools also allow the Automation Engineering group to properly execute other significant
technical practices like code reviews. These tools allow the group to stay true to their principles
and self govern to fix issues. Self governing is the best solution for keeping proper quality in
software, as it does not impede the software development work (that much). But it requires senior
employees who are invested to keep these practices. Who have the authority and budget to keep these
practices.

## Documentation

In many places documentation means only the documentation that is supplied to the customer at the end of
the industrial automation project. This thinking however fails short in the real world. Good documentation
is divided in multiple documents, each catering to different recipient.

Recipients for documentation in Industrial Automation Software projects (with some examples):

* Project Customer:
  * For Customers Company: device lists, manuals for maintenance, schematics, specifications etc.
  * For Customers Operators: Automation system manual, HMI user guide, Quick reference manual, Upset
    recovery manual.
* Systems Integrator:
  * Communication API documentation. Control diagrams. Device/Process descriptions. External system
    requirements.
* Sales?
* Internal Support:
  * Customer documenation
  * Internal documentation
* Internal Software Engineers:
  * Architectural design, control flow design etc.
  * Interface design documents, IO-lists.
  * Industrial process design documents.
  * Hardware documents, PLC & IO, used protocols, third party devices.
  * Version Control for context in code changes.
  * Issues in Bug Tracker for context in code changes
  
The internal software development documents refer to the software. The software is designed to change,
so the documents also must be easy to change. For the same reason these documents must be in version
control. If nice-looking deliverables are required from these, those can be generated with tools like
pandoc.

"Refactoring" this internal documentation should never be separate task. It is part of the technical
practices, part of the software engineering work, and thus always done. If this practice is not
followed, in some time these documents will be worse than non-existing. They will actively hinder
new hires.

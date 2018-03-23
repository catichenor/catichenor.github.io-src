Title: Mission Statement
Date: 2018-03-22 18:26
Modified: 2018-03-22 18:26
Category: Site
Tags: programming, howto, learning, C, C++, JavaScript, Java, Python
Slug: mission-statement
Authors: Christopher Tichenor
Summary: The mission statement of this website.

The goal of this site, at least for now, is for public accountability of my efforts toward learning algorithms. As I’ve found out, it’s really quite difficult to get a job involving even entry-level coding without knowledge of algorithms pulled randomly out of a book (and apparently unrelated to actual life/job experience). I’m definitely not an entry-level coder, but I’m largely self-taught and thus have little background in algorithms.

So how shall I study these? There’s a few variables:

* What language to use.
* What sites provide the best path to learning.
* What learning method to use.

## Language 

I’m thinking four possibilities: C/C++, Java, JavaScript, Python 

### C, then C++ for OO

* Pros
	* Closer to the metal, will learn manual memory management 
	* Many tutorials map closely to the specific data structure implementations of the C family 
	* Many imaging libraries, especially on embedded hardware, use C++
	* More impressive in interviews(?)
* Cons
	* I haven’t used either C or C++ much in a decade.
	* Have to learn the idiosyncrasies of two (related) languages.
	* Code is more “fiddly”, will take longer thair complete lessons.
	* Have to manually manage memory.

### Java

* Pros
	* Used by most automation engineers from what I can tell.
	* Doesn’t have as many "sticky points" as C/C++
	* Only have to use one language.
	* My most recent programming classes used Java, so this should be “review”.
* Cons
	* I haven’t used Java much in the past decade, either.
	* Pretty far from technologies I’m interested in (except maybe Scala).
	* In something of a no man’s land between low-level and high-level languages.
	* Icky, boilerplate-y.

### JavaScript

* Pros
	* Will he helpful for front-end web (and maybe backend) programming.
	* I’ve used JavaScript much more recently than Java and C/C++
	* Good excuse to learn new JavaScript enhancements 
* Cons
	* Not really suited for algorithms 
	* More of a functional language with C-ish syntax
	* Almost a DSL for browsers (I know, Node.js makes it a server language, but that ship seems to be sailing...)
	* High-level

### Python

* Pros
	- The primary language I'm currently using.
	- I've lots of tooling to support my work.
	- Pythonista gives me a way to do exercises on-the-go.
	- A very flexible language, can help with most of what I want to do.
	- Popular, lots of tutorials available.
* Cons
	- High-level
	- Doesn't really map well to certain algorithms (can't work with pointers, etc.)
	- A relatively slow language.

I *really* want to do C/C++ but time is of the essence so I think the best option here is Python.

## Sites

[This page](https://medium.freecodecamp.org/5-key-learnings-from-the-post-bootcamp-job-search-9a07468d2331) I read last weekend was pretty insightful. Here's the sites directly lifted from that, seems like a good place to start:

>  * [InterviewCake][1]: My favorite resource for data structures and algorithms. It breaks down solutions into step-by-step chunks — a great alternative to Cracking the Code Interview (CTCI). My only gripe is that they don’t have more problems!
>  * [HiredInTech’s System Design Section][2]: A great guide for system design interview questions.
>  * [Coderust][3]: If you’re avoiding CTCI like the plague, Coderust 2.0 may be perfect for you. For $49, you get solutions in almost any programming language, with interactive diagrams.
>  * [Reddit’s How to Prepare for Tech Interviews][4]: I constantly used this as a benchmark for how prepared I was.
>  * [Front End Interview Questions][5]: An _exhaustive_ list of front-end questions.
>  * [Leetcode][6]: The go-to resource for algorithm and data structure questions. You can filter by company, so for example, you could get all the questions that Uber or Google typically ask.

My notes on each:

* **InterviewCake:** $99 for 1 year/50 hours of practice questions. Not too bad of a deal.
* **HiredInTech System Design:** Looks like it's free? Not a bad deal at all. I think I got a pretty good idea of systems architecture in my last job but it might be good to formalize that.
* **Coderust/educative.io:** $49 for lifetime access and 83 lessons. Also a pretty good deal.
	- By the way, CTCI refers to "Cracking the Coding Interview", which I am indeed avoiding like the plague (too popular).
* **Reddit article:** It's Reddit, so of course it's free*, but yeah, seems like a good benchmark.
* **GitHub Front End Interview Questions:** I'm not terribly interested in front-end development, but this could be useful.
* **Leetcode:** I downloaded an app that collects info from this site, not sure if it requires a subscription, which is $35/month or $159/year.

## Learning Method

Judging from my typical work method, I'll set a goal of 3 algorithms a day, and adjust based on my pace. I'll also need to alter the questions to protect the innocent.

   [1]: https://www.interviewcake.com/
   [2]: https://www.hiredintech.com/classrooms/system-design/lesson/60
   [3]: https://www.educative.io/collection/5642554087309312/5679846214598656
   [4]: https://www.reddit.com/r/cscareerquestions/comments/1jov24/heres_how_to_prepare_for_tech_interviews/
   [5]: https://github.com/h5bp/Front-end-Developer-Interview-Questions
   [6]: https://leetcode.com/
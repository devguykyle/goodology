---
title: How to Get Better as a Web Software Engineer
published: true
description: Some thoughts on how to get from junior developer to better and gain real production skills.
categories:
- advice
tags: 
- upskill 
- advice
- web development
- web developer
# cover_image: https://direct_url_to_image.jpg
# Use a ratio of 100:42 for best results.
# published_at: 2023-02-28 18:32 +0000
---
# From Junior to Better
As I look back on my career as a software engineer from the perspective of a senior software engineer I realize that a junior web software engineer isn't someone who knows nothing, it is someone who knows enough to make a web app dangerous. Getting out of that stage as a developer can be a lengthy and at times confusing ask. I have found that several factors contribute the long churn of time spent as a junior developer. For one, a junior developer is often trying to figure out which way is up. By that I mean there are so many languages, frameworks, and roles within the software development world, it can be hard to know which stream of expertise to follow. This leads to starting and stopping tutorials and books that sail over your head. After several months or even a couple of years,  you wind up knowing a very small amount -- almost nothing -- about every popular tool in existence. However, and most importantly, when its time to sit down and build a healthy application, you haven't a clue what a healthy application looks like. These issues can be remedied.

# Problem Solving
The key fix for this dilemma, everything described below, can be summed up in the phrase 'enjoy a skill'. This is hard for so many reasons. We can be worried about finding a job if we dont know every technology on every job description. Impostor syndrome sets in when we think other people know everything and we know nothing. There is also the valid idea that it is important to know more than one trick. However, there is a lot to be said for someone who knows how to produce high quality results with their own tools whatever they may be. Investors, hiring managers, and project teams are looking for persons who can produce something not just people who have heard of a lot of 'tools of production.' So how do you get to a place where you can produce something well crafted on your own as a web software engineer.

## Depth in a Language
Having 30 tabs of tutorials open is not the way to achieve production level skills in 30 programming languages. It is simply not useful to know how to use the print or log function in 30 different programming languages. That wont tell you what makes any language more useful than the next. Instead of 30 tabs of tutorials on different languages, find one in-depth tutorial on one or two languages (depending on you real diet for these things) and stick to them. Learn all that you can and start building things that fit that language's paradigm. Explain to yourself what it is doing in the various parts of your project. Eventually you'll run into comparison of the language you're learning in depth versus other languages and it will lead to a generally better grasp on programming languages overall.

## Depth in a Framework
Whether you consider yourself a front-end, back-end, or full-stack engineer, there are simply too many frameworks to learn them all at once. Don't let job descriptions and conference talks convince you that you're unhireable if you don't know framework ```{{ some_framework}}```. Similar to the standard set with regard to languages, you're better off picking some particular tool and running with it. Learn how to safely handle form inputs and where certain files should go. Read documentation and watch talks that explain not just how to build an application in the framework but learn why its built that way. Build something simple and share it with the community behind that framework and then iterate to make it better.

## Depth in Databases
A lot of web developers can make the mistake of thinking that databases are simple and when you're only running your app on localhost that can seem true enough. But app in production need to be able to scale and a lot of collection and table design decisions go into that among other concepts so don't simply build the same app in SQL and NoSQL and move on. Pick a journey with one of these structures and get good at utilizing it for the next year.

## Depth in Other Tools
For the sake of not becoming too repetitive, lets lump all of the stack into this same idea. Find an IDE like VSCode and learn the macros and shortcuts in it. Pick an operating system and learn how to use its command prompt with ease. Finally hone your skills with some Git client. Without these, your best knowledge of any toolset will be bottlenecked if you are not sufficient with your local pipeline.

## Depth in Delivery
The final key is to deliver. When learning these tools of the trade, find a project to use them in. Either become a serious contributor to an open source project or build a tool yourself. Whichever option you choose, make sure you do it with care. Don't just release another portfolio or blog site and then abandon it because in the lifecycle of the app you will develop expertise. Just as a case study consider: Yet Another Task App
	1. build a full stack task app and deploy it on Heroku (2 weeks)
	2. think of a cool additional feature and build it out and deploy (2 weeks)
	3. find out a package or gem had a security patch yup upgrade it and deploy (1 day)
	4. add a separate notes service to your app (1 weeks)
	5. learn about some FE framework and build out your apps api and a new FE to use it then deploy the FE app (1 month)
	6. want to add an enterprise wide notes system so you make changes to your db and add user roles (2 weeks)
	7. add Typesense for searching notes (1 week)
	8. Add a worker to off load the email process
        9. add an end to end testing framework
        10. incorporate useful bug logging
	11. add gifs, add email alerts when notes are added or updated, add stipe payments for the business plan of your app.
	
# Final Note
That is a couple of months well spent. Through all of these steps and by focusing in on a particular set of technologies, you are learning to work in production and taking on increasingly complex task. It is better to release yet another task list app that you actually keep up with than to abandon part of some dangerously insecure SaaS idea and chuck it on your resume. Whatever you build, utilize your developing knowledge of your toolset to build it extremely well and then maintain it. One of the worst things that can happen in an interview with a junior or midlevel dev is to ask 'how would you improve ```{{ some_app_from_their_short_resume }}``` ' if you were building it today and to find out they aren't sure what they did or why. Depth in your toolset will help you answer this more intelligently.

The best way for junior engineers to gain a breadth of skills and to become better contributor is to aim at depth of skill.
---
layout: post
title:  "The Rock That Would Become a Gem.  "
date:   2017-07-10 12:56:00 -0400
---


This project, for some time, has been causing me angst.  It was a rock around my neck.  I had grown accustom to the work flow of the lessons and labs, and equally accustom to the ‘Ask a Question’ option; Let’s just say that I started to know the Learn coaches’ work schedules.   

This project was different.  It was just me.  I took (or taking, still not quite done) this serious.  I wanted to feel the weight of responsibility.  I started by watching and reading all the provided resources.  I read, and reread material, lessons and labs.  I watched Avi’s “Walk Through” video, taking notes the whole time.  I scribbled ideas in my notebook, and visited websites I was interested in scrapping.  I jotted down diagrams of the flow of my gem.  I made list of things to do.  I re-watched parts of Avi’s video, making even more detailed notes.  Then it occurred to me: I need to write some code.  

I started.  I pulled up an empty terminal and text editor.  Yikes!  It was all about baby-steps.  I typed into the command line, bundle gem famous_physicists_cli_app.  It didn’t take long before I deeply regretted such a long name, but with this file structure that looked so “project” like, I couldn’t look back now.  

My next step was to get my environment correctly formatted, and get the files talking to one another.  So, my task was to print out to the terminal “E=mc^2.”  I simply wanted to format my file structure so I could have the bin file speaking to the cli file, and I could print a simple, hard-coded statement.  This took two hours.  Oh, but the elation I experienced when I saw, “E=mc^2.” Like I said, baby-steps.  But this was an important step for me. Because I felt the power of momentum.  

I continued by following Avi’s suggestions, and stubbed out, in my cli class, my entire gem.  It printed out to the terminal beautifully, but, as Avi said, it was brain-dead.  This, however, provided me real structure.  The things I had to start working on became clear; I could begin to see potential classes and methods.

The basic idea for my gem was to create a resource where a user could see a list of the top physicists, and peruse through basic info and maybe a bio or profile.  So that was the big picture I had in mind and my cli was printing out this basic structure.   

Now that I had my cli up and running, and putting out to the terminal the basic structure of my gem, I began to build my scraper class.  I picked a website that stable, conveniently formatted and accessible.  With my website picked, I wrote my first method for my scraper class which simply opened the website.  I then worked extensively to scrape and print out the list of physicists.  The website I was working with featured history’s twenty most famous physicists.  This is clearly a subjective list, but largely speaking, I agreed, so I continued.  I enjoyed the scrapping. I found it to be like a scavenger hunt, but I had to keep in mind the explicit instructions of the project.  I had to stay focus on the functionality of the gem, and not get lost on the scrapping.  

After some steady work, I got a handsome list to print out.  I used the each_with_index enumerator, producing a numbered list of physicists.  I must have printed that list out a dozen times.  But there was no time to break my arm patting myself on the back, I had a lot more methods to write.  Additionally, I had break my code up into smaller methods.  Initially, I had one method opening the website, collecting the names, iterating over the newly populated array, and printing this out.  But I was happy that I had something up and running.  

I worked to divide those functionalities up into individual methods, ones that I could call on later.  Additionally, I created a method that scraped the necessary information.  Namely, I scraped the name, date of birth, date of death, what the physicist was most famous for and a brief profile.  Now, I could print all this information out, however, there lacked any real methods with defined responsibilities.  So, I worked to encapsulate smaller chunks of functionality into methods.  But the thing that I was ignoring was instantiating an object called a physicist which had all the attributes that I had worked so hard to scrape from the website.  

I also I had another problem: the index website provided a link for each physicist, and this is where all the meaningful date was scrapped from.  However, sixteen of the twenty link took you to a specific html format.  Consequently, this was easy to scrape from.  But there were four links that took you to an html format that was different than the other sixteen.  Now, I was faced with two obstacles.  First, I still had to work through the process of instantiating a new object called physicist, one in which has all the attributes, and second was figuring out a way for a method or methods to know which html formatting it should consider when scrapping the website.  So, again, I relied on baby-steps.  I needed to build a loop that would tell the program which scrapping code to use for which sub link.  While I was scrapping all the necessary attributes, I built a method that scraped these sub-links.  So, since I knew the sub-links that were problematic, I built a loop that enumerated over the array of sub-links, and assigned the appropriate scrapping code to the appropriate sub-link website.  But this too was long and ugly code.  

But I had something up and running, and I could print out, now, all the information I want the user to have access to.  Mind you, I have not made one object called a physicist yet.  My job now was to take the functionality of the loop that allows me to access the appropriate data, and tweak so I can use it to instantiate an object with all those attributes.  However, the problem I found was that in the same method I needed to access two different sources for data.  I may be a bit of brute force, and definitely worthy of a longer look and refactoring, but I went with a loop that each time called for that particular link to be opened.  It is messy, verbose and expensive, but it worked like a charm.  I could now instantiate and object called ‘physicist,’ and have access to all those attributes I so badly needed.  

I made something.  Something that nobody else knew anything about.  Whether I was sharing it with my wife, friends or mentor, I was the one who knew everything there was to know about it.  I’m not sure I have ever experienced this sort of creative power.  I went to bed thinking about the code, functionally and the users experience.  It also has caused me to begin to think much, much more about other potential projects, perhaps, regarding education, music (I’m a guitar player), exercise and maybe, meditation.  This project, that started as a rock around my neck, after some polishing, had finally become a gem.   







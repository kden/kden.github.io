
# Statement regarding the use of AI
I've made liberal use of AI throughout the project.  However, I didn't type in a prompt and let it spit out my project.  It was an 
iterative process which was greatly sped up by using ChatGPT 4o and Google Gemini Pro.  

One reason that I wanted to do a project that interacted with hardware sensors was
that I wanted the code to interact with a hard barrier that AI could not simulate.  I figured it would be easy to ask an LLM to spit out an entire program that looked
like a plausible solution, but if it doesn't interact with anything, you don't know if it works or not.

Also, electronic components and libraries change quickly, and I thought I would probably have to do some troubleshooting and
development that the LLM could not help with.

## LLM Limitations
Gemini and ChatGPT have accelerated my development immensely, but there were some problems they struggled with.
For example, both struggled with writing React unit tests of any complexity.  They also got confused by converting timezones between 
JavaScript timestamps and Firebase's own timestamp format.  Often, they would produce Terraform definitions that were on the right track, but six months to a year behind, 
and I would have to track down the most recent version of their resource definitions to fix an error. 

Some general problems I ran into:
- Forgetting a source code file I've already shared and generating an entirely new one, deleting all the updates I've been making
- Flipping back and forth between the same two solution fixes that don't work
- Hitting the memory limit when a project reaches a certain size, so that it can't keep track of all the files that a change affects
- Adding "improvements" to your code that you didn't ask for that interfere with the sequential plan for updates you're making
- Knowledge in general is a little bit behind, maybe a year or so, so you will have to figure out what has changed in the latest library versions

## Results
One of the effects of using AI to speed up the development process was that I got to try out different approaches more easily.  
I got to spend more time architecting the project and breaking it down into reasonable blocks of work that could be tested.  
Less time was spent looking up how to do the initial configuration of a project and add boilerplate code.
I could also see that it could be addictive, and it would be easy to let enthusiasm for the project run away with you and implement a bunch of 
untested spaghetti code.  Software engineers that can see the big picture and direct and verify the LLMs work are going to be 
valuable members of their teams.
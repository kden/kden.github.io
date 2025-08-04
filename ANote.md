# A Note to Employers Regarding the Use of AI

As I write this, in July of 2025, both employers and potential employees are struggling greatly with the use of AI.  Job postings are inundated with automatically-generated applications.  Cover letters are no longer a useful indicator of someone's writing abilities. Potential employees are screened out by opaque criteria that make no distinction between auto-generated applications and applications that have been labored over for hours.  There are no tools that have been proven to reliably detect AI.

I would discourage employers from forbidding developers from using AI or documentation in coding exams.  For one thing, that's not an accurate reflection of what they will have access to on the job.  Someone with a great memory for syntax is not necessarily the person who will have other strengths, like critical thinking or creativity.  For another thing, you will end up rewarding people for being dishonest.  Your applicants will be in the sad and difficult position of choosing between cheating or being routinely outperformed by those who do.

Based on my recent experience in the job market, I'd suggest the following:

1. Include 1-3 short essay questions in the job application.  As I look through all of the identical job submission forms for greenhouse, lever, myworkdayjobs, etc., I can't help but think about how easy it would be to write scripts to fill these cookie-cutter application forms in bulk.  Which is a huge problem right now for potential hirers and hirees.  Adding a few questions like "What drew you to this field?" or "What is an obstacle you've had to overcome in your career that made you a more understanding person?"  Most forms are looking for a resume and a cover letter, which bulk applicants probably already have ready.  This might make at least some of them skip your form.
   
   A related option is a technical screen-out question that asks them to go to a different part of site, solve a simple problem, and come back and paste the answer.  This obviously requires more setup on your end, but you might earn it back in the number of resumes you don't have to weed out.

2. Use take-home exams, and allow the use of AI with disclosure.  One of the best-done interview processes I went through took this approach. 

   Try to aim for an assignment that won't take more than a couple of hours.  Asking someone to do 2 days of unpaid work is a big investment for a job you may not get.  

   Provide some, but not all of the test cases for the assignment.  

   Finally, in the interview, ask them questions about the code and make sure they can explain how all of it works.  Ask them what features they would add.  Ask them what challenges they ran into.

   I also prefer take-home exams because they are the easiest to adjust for people with disabilities.  You can easily use your own tools for a take-home. You can also work around your schedule.  Additionally, if someone is willing to put in some extra time to do a good job, I don't think they should be penalized for that. 

3. Allow AI and documentation searches during online coding exams.  Use a coding tool that will let them run some predefined test cases and hold back some test cases.
   Again, after the exam, you can ask them to explain what everything does.

4. Consider offering the position as a contract-to-hire position.  Realistically, to know if someone is going to be able to do the job, the best test is to let them do the job.  The interview and job hiring process has been marginally effective since long before the widespread availability of AI.

I'd like to end with what's either an apocryphical internet story or a Dad joke:  "Once, I had a boss who would take the stack of resumes for any job application, shuffle them, and toss half right in the trash.  Naturally I was appalled.  I asked him
why he did it.  He said, 'I don't want to hire any unlucky people.'"  

We don't always acknowledge how hard and time-consuming hiring is, but it is one of the most important roles you can have.  That investment can have a huge positive or negative impact on your organization.


# BUT WHAT ABOUT SECURITY??

You are right to be concerned about security.  There have already been a number of instances of personal information and source code that were fed into LLMs as a part of questions, became training data, and were later leaked to other users.  You will have to measure your security risks and plan appropriately.  First, what is the risk of loss if your source code is leaked?  You should be aware of this already, because LLMs are not the only way that data gets leaked.  Depending on your risk, you might do one of several things (from less to more secure):

1. Study the terms of your agreement with the LLM provider and make sure that you are using a product where your data does not become part of their training data.  Of course, this assumes that you trust them to not "accidentally" save your data anyway.

2. Use a cleaning script to remove sensitive information from source code before uploading it or sharing it with an LLM.

    I have the beginnings of such a script in the [sunlight_sensor_gcp](https://github.com/kden/sunlight_sensor_gcp/blob/main/clean_code_export.sh) project. It copies only the files that get checked into git to a separate folder to be uploaded as context to some LLM tool.  You shouldn't be checking any sensitive information into source control in any case.  In addition, I would probably add code to search and replace company and product names with something generic.

3. Use an offline LLM.  There are some LLMs you can download and run locally.

Keep your assets secure.  But weigh the cost of security versus the cost of potential loss.  I think it is only going to get harder to prevent developers from using AI as a part of their work.  Soon, if not already, it will be like asking someone to write code without Google or Stack Overflow, while competing with companies who have access to these tools and can develop much more rapidly. 


_Written without the aid of AI_

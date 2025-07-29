This was published on my blog for Codepaw Consulting, when I ran my own consulting LLC, in 2018.  In 2025, I wish that awkward English were still a reliable red flag for spam communications.  That's something that LLMs are masking well.

# Is this a scam?  A true-life story

A few weeks ago, a friend of mine told me how she brought her personal laptop to work one day and had trouble connecting to the printer.  She did what any of us would do: went online and Googled for a solution.  Even better than a list of instructions, she found an ad from a company who could log onto her computer remotely, and experts could fix the problem as she watched.

She contacted the company for help, and they fixed the printer issue easily. Then they ran an anti-virus scan against her computer and found a number of viruses.  For the service, they charged her $170.  However, it came with 2 years of personalized expert technical help and virus protection.  They told her that they could only be paid by check.

As I listened to her story, I thought, well, is it or isn’t it?  She sought them out; she didn’t click on a link in a phishing email or a pop-up on her computer telling her it was infected.  They fixed her printer problem, after all.  Since it wasn't a printer she'd used before, I'm guessing they downloaded and installed the drivers for her and that was it.  On the other hand, what was this business about requiring a check?  And it wouldn’t be difficult to fake a virus scan that found viruses on her computer, and then pretend to fix it. 

I asked her if I could research it.  Here’s what I found out.

# Who are they?

So, I also did what anyone would do when rating the credibility of a company they’ve never heard of.  I went online and looked for reviews.  Unfortunately, the name of the company, “Web Online Help, Inc.” was so generic it was difficult to search.  So I tried the phone  number.  Interestingly, it was associated with a similar business with a similarly generic name, “Green Web Assist.”  This seemed like a red flag, but a small one.  Some businesses that operate online have several names to help with SEO and marketing their services to groups of customers with different needs.  Web Online Help could have purchased Green Web Assist and absorbed their phone numbers.  

Green Web Assist had some negative reviews on “[Ripoff Report](https://www.ripoffreport.com/)”  but it wasn’t so much that people were accusing them of fraud as people who weren’t happy and wanted refunds.  So, another small red flag.  Not all small businesses can afford to have the refund policies of an Amazon.  That doesn’t mean they’re malicious.

Then, I looked to see if these were legitimate businesses.  Their web sites indicated that they were in the New York City area, so I looked at the [New York State database for company registration](https://www.dos.ny.gov/corps/bus_entity_search.html).  In the defense column (the green flag column?) they were each registered as legitimate businesses.  I used Google street view to look at their business addresses.  Green Web Assist appeared to be a hotel, and Web Online Help appeared to be run out of someone’s house.  This still doesn’t seem all that suspicious to me.  Lots of small businesses are run out of the home. 

# Are they professional?

I also just looked over their web sites to see if they looked professional or thrown together.  I didn’t do anything fancy like analyze the source code of the pages.  I was looking to see if links were broken or there were misspellings.  Bad grammar is not evidence of a crime, but if the language is awkward it might be a weak signal that the business is located overseas where they are safe from the victims of their fraud.  One of the sites had the standard boilerplate icons linked to social media sites like Facebook, Twitter, and LinkedIn, but none of the links worked. 

Also, some of the language was awkward.  Here’s part of the follow-up email from Web Online Help that my friend shared with me.  What do you think?

> Thank you for choosing Web Online Help Inc as your technical support
> partner. Your Customer ID Number is XXXXXXX. Please use this for all
> your communication with Green Web Assist. This Customer ID covers your
> subscription of $170 USD which secure your device(s) for the certified
> time.
> 
> Please follow the following instructions carefully to avoid any scams:
> We will never call you. Therefore, don’t entertain any inbound calls. As
> a recent scam trend, you might receive a call by bad people claiming to
> be your subscription partner. In case they do, please do not entertain.
> If they still persist, you may ask them to confirm your Customer ID
> before continuing.
> Never share your Customer ID with anybody. Keep it secure and safe.
> Incase you receive any emails claiming to be us, check the authenticity
> of Email ID from where you received the email very carefully.

If it had only been the error with “subscription which… secure your devices” I would have written it off to a typo.  But who says “Please do not entertain”? or “you might receive a call by bad people”?  It sounds like something that was poorly translated.  

Even including instructions to avoid scams seems a little like a weak ploy to gain trust.  

# Is this a known scam?

Well, I wasn’t convinced either way by what I’d found so far.  So next, I tried to determine whether this was a known scam.  In particular I thought that the part about paying by check was unusual.  I was also curious whether scammers were known to be using search engine ads targeting people who needed technical support.

This is an important point.  Information security, and indeed, all of information technology moves fast.  Not only does the layman not know whether a hypothetical solution has been implemented, people in the field sometimes don’t know how advanced technologies have  gotten.  Instead of just looking at situations you know to be possible, you need to imagine what could be possible soon and research that before you rule it out.   

As it happens, I couldn’t find anything about check scams being any more in vogue than they have been in the past.  [However, scammers may be motivated to ask for checks because they don’t have the customer protection offered by credit cards]( https://www.fool.com/investing/2017/01/29/how-writing-personal-checks-can-expose-you-to-frau.aspx).  Once the money is out of your checking account, you can’t get it back. 

[The tech support ad scam, on the other hand, seems to be having a surge](https://www.digitaltrends.com/web/google-crackdown-on-tech-support-scams/).     Well, that’s a red flag.  

# Looking for more evidence
My friend had two programs that the tech support company left on her computer.   One started up and printed out messages saying that it had installed a bunch of files.  The other one, by far my favorite, started with a splash screen for a program called BAT2EXE and then printed out a list of arbitrary IP addresses and ports.

So what were these programs actually doing?

I started with the second program, called “Network Security Firewall.”  So, if you’re familiar with Windows programs, or like me, you’re old enough to have downloaded zipped shareware programs from BBSes in the 90’s onto your Windows 3.1 machine, you may already know that an “EXE” is an executable file, and a “BAT” file is a batch file.  Usually, an EXE is compiled.  If you were to open it in a text editor, you’d see a bunch of garbage.  Usually, a BAT file is a text file.  If you were to open it in a text editor, you could read the lines of code.  


BAT2EXE, then, is a program that takes a BAT file and converts it to an EXE file.  The advantage then is that if someone downloads your EXE it’s harder for them to read the source code.  But most BAT2EXE programs don’t create a very sophisticated EXE.  [When the EXE actually runs, it just extracts the BAT file to a temporary directory and runs it as a BAT file](https://forums.techguy.org/threads/solved-exe-back-to-bat.759618/).  So all I had to do was run the program and grab the BAT file while it was running.  (It would delete the BAT file every time execution completed.)  Here are the contents.

```
@echo off
set ztmp=C:\Users\IEUser\AppData\Local\Temp\ytmp
set MYFILES=C:\Users\IEUser\AppData\Local\Temp\afolder
set bfcec=tmp73078.exe
set cmdline=
SHIFT /0
@Echo Off
Echo Active Connections

Echo Proto  Local Address          Foreign Address       State
Echo  TCP    192.168.1.102:49345    None               Secured
ping www.google.com -n 2 >null
Echo  TCP    192.168.1.237:49223    None               Secured
ping www.google.com -n 2 >null
Echo  TCP    192.168.1.242:23946    None               Secured
ping www.google.com -n 2 >null
Echo  TCP    192.168.1.102:54781    None               Secured
ping www.google.com -n 2 >null
Echo  TCP    192.168.1.45 :44098    None               Secured
ping www.google.com -n 2 >null
Echo  TCP    192.168.1.109:12387    None               Secured
ping www.google.com -n 2 >null
Echo  TCP    192.168.1.175:34609    None               Secured
ping www.google.com -n 2 >null
Echo  TCP    192.168.1.200:49957    None               Secured
ping www.google.com -n 2 >null
Echo  TCP    192.168.1.145:49965    None               Secured
ping www.google.com -n 2 >null
Echo  TCP    192.168.1.133:49972    None               Secured

ECHO(&echo(&echo(&echo     	Network Secure!
ECHO(&echo(&echo(&echo        	Security is Running...
echo a=MSGBOX ("YOUR COMPUTER IS PROTECTED!",,"Network Status") > %temp%\tmpmsg.vbs
call %temp%\tmpmsg.vbs
del %temp%\tmpmsg.vbs /f /q

taskkill /f /IM cmd.exe
pause >null
exit
```

So, overall, this script prints out a bunch of bogus information and tells you, “YOUR COMPUTER IS PROTECTED!”

Look at the lines starting with `Echo TCP`.    Echo is like print in other languages.  It means, "echo the following to the screen."  So all of those lines are printing out arbitrary IP addresses and ports.  It’s not intelligently looking at your network and coming up with a list of addresses; it’s not using some magic well-known list of addresses guaranteed to be important on your network; it’s just a flat list of made-up addresses being printed for show.  These addresses may not even exist on your network, and when it’s printing “Secured,” it’s not actually securing anything.

The lines beginning with “ping www.google.com” ping the google server.  Ping is a simple command that goes out to the server of your choice and just asks it to send you a response confirming that it’s alive.  The purpose here seems to be to create a brief delay so that the other commands seem to be doing something.  The `>null` gets rid of the response, so even if you were to get some useful error message from the ping command, it's throwing it in the garbage.

# So… is it a scam?
Well, the batch file program proves that this company is being dishonest by claiming that they are running something that actually secures your computer.  So yeah, I think it's a scam. At least the security services they claimed to offer are a scam.  Fortunately, my friend was running Windows 10, which ships with decent anti-virus software and it had been up and running  continuously, and indicated no events or other problems.  At first, I feared the worst for her computer.  But then, I thought a bit more about what we knew about this company.

They’re a business registered in the United States.  Their motivations seem to be financial, not personal or political.  They seem to have a vested interest in keeping this scam going so that they can keep generating cash.  They can’t do that if they draw customer attention to the fact that they’re committing fraud.  So, my friend is keeping an eye on her checking account and her antivirus software, and has deleted every file that they put on her computer.  I’m guessing... and hoping... she won’t hear from them again.  I also think she has little chance of getting her money back.  But hey, they did set up her printer connection.

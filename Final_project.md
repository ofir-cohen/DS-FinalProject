
# Data science final project - Ofir Cohen and Doron Sionov

##                     Generating famous pages' Facebook posts

## Step 1 - Information collection

We will use the facebook sdk to perform facebook grpah api calls.

The following facebook api code is commented out to save time in future runs. the post will be loaded from a file we have created.


    #!pip install facebook-sdk

Obtaining graph instance: 


    #import facebook
    #import requests
    
    #graph = facebook.GraphAPI(access_token="ommited for privacy concerns")

### Collecting posts

Now that we have a graph instance, we will use it to get our 5 pages posts.
We define a generic function to iterate over facebook graph api pages to retrieve all posts, and call it with our 5 pages. 
Note - if we want different pages, we can easily replace pages by giving a different url to the getAllPosts function call


    def getAllPosts(pageURL):
        FirstPosts = graph.get_object(id=pageURL, fields="posts",limit = "100")['posts']
        allPosts = []
        while(True):
            try:
                for post in FirstPosts['data']:
                    if(len(allPosts)<150):
                        if('message' in post):
                            allPosts.append(post['message'])
                        FirstPosts=requests.get(FirstPosts['paging']['next']).json()
                    else:
                        return(allPosts)
            except KeyError:
                break
        return(allPosts)

We chose these 5 pages:
Donald Trump,
Barack Obama,
Humans of New York,
Harvard University
and 
Fox news.


    #TrumpPosts = getAllPosts("https://www.facebook.com/DonaldTrump/")
    #ObamaPosts = getAllPosts("https://www.facebook.com/barackobama/")
    #HONYPosts = getAllPosts("https://www.facebook.com/humansofnewyork/")
    #HarvardPosts = getAllPosts("https://www.facebook.com/Harvard/")
    #FoxPosts = getAllPosts("https://www.facebook.com/FoxNews/")

Here we write the posts to two files, one that is human friendly and easy to read:


    #collectedPostsFile = open('collectedPosts-Readable.txt', 'w',encoding = 'utf-8')
    #for i,page in enumerate(allposts):
    #    collectedPostsFile.write("\n The " + pagesNames[i] + " page posts are:"+  "\n" + "\n")
    #    for post in page:
    #        collectedPostsFile.write("%s\n" % post)
    #collectedPostsFile.close()

And one that is used as input for the rest of the code:


    #collectedPostsFile = open('collectedPosts-AsInput.txt', 'w',encoding = 'utf-8')
    #collectedPostsFile.write(str(allposts))
    #collectedPostsFile.close()

Reading posts from the input file:


    import ast
    collectedPostsFile = open('collectedPosts-AsInput.txt', 'r',encoding = 'utf-8')
    allposts = collectedPostsFile.read()
    allposts = ast.literal_eval(allposts)
    collectedPostsFile.close()
    TrumpPosts = allposts[0]
    ObamaPosts = allposts[1]
    HONYPosts = allposts[2]
    HarvardPosts = allposts[3]
    FoxPosts = allposts[4]

### Describing the data

Every facebook page has mulitple types of publishes - pictures, videos, posts, stories etc. 
So, it is often not possible to collect large amount of textual posts from a page.
Therefore we would start by looking at the amount of posts we collected, and look at 5 from each page.


    allposts = [TrumpPosts,ObamaPosts,HONYPosts,HarvardPosts,FoxPosts]
    pagesNames = ["Donald Trump","Barack Obama","Humans of New York","Harvard University","Fox news"]
    index = 0
    for page in allposts:
        print("In the page of ",pagesNames[index], " we collected ",len(page), " posts\n")
        print("The latest 5 posts in this page are: \n",)
        for post in page[0:5]:
            print(post,"\n")
        print("\n")
        index = index + 1

    In the page of  Donald Trump  we collected  121  posts
    
    The latest 5 posts in this page are: 
    
    Today, it was my great honor to host a School Safety Roundtable at the White House with State and local leaders, law enforcement officers, and education officials.
    
    There is nothing more important than protecting our children. They deserve to be safe, and we will deliver! 
    
    History shows that a school shooting lasts, on average, 3 minutes. It takes police & first responders approximately 5 to 8 minutes to get to site of crime. Highly trained, gun adept, teachers/coaches would solve the problem instantly, before police arrive. GREAT DETERRENT! 
    
    I will be strongly pushing Comprehensive Background Checks with an emphasis on Mental Health. Raise age to 21 and end sale of Bump Stocks! 
    
    Question: If all of the Russian meddling took place during the Obama Administration, right up to January 20th, why aren’t they the subject of the investigation? 
    
    President Donald J. Trump's schedule for Thursday, February 22nd:
    · Meeting with State and local officials on school safety 
    
    
    
    In the page of  Barack Obama  we collected  111  posts
    
    The latest 5 posts in this page are: 
    
    Today, Kehinde Wiley and Amy Sherald became the first black artists to create official presidential portraits for the Smithsonian. To call this experience humbling would be an understatement. 
    
    Thanks to Kehinde and Amy, generations of Americans — and young people from all around the world — will visit the National Portrait Gallery and see this country through a new lens. They’ll walk out of that museum with a better sense of the America we all love. Clear-eyed. Big-hearted. Inclusive and optimistic.  And I hope they’ll walk out more empowered to go and change their worlds. 
    
    This month, we tell the stories of a special set of heroes: The African American community organizers and active citizens who have, throughout our history, brought people together in pursuit of a more just, inclusive, and generous America. The pioneers who challenged us to dream bigger.
    
    C.T. Vivian is one of those heroes. A Baptist minister, C.T. Vivian was one of Dr. King’s closest advisors. “Martin taught us,” he says, “that it’s in the action that we find out who we really are.” And time and again, Reverend Vivian was among the first to be in the action. In 1947, he joined a sit-in to integrate an Illinois restaurant. He was one of the first Freedom Riders. In Selma, he stood on the courthouse steps to register blacks to vote -- and was beaten, bloodied and jailed for it. In standing up, he helped inspire a young woman to sit down and hold her ground. Her name was Rosa Parks. She said of him, “Even after things had supposedly been taken care of and we had our rights, he was still out there, inspiring the next generation, including me.”
     
    And me. Without the sacrifices and hard-won battles of folks like C.T. Vivian, I may have never had the honor to serve as your president.
     
    And that’s the beauty of this nation: Each new generation stands on the successes of the last -- and reaches up to bend the arc of history in the direction of more freedom, more opportunity, more justice.
    
    As we continue to reflect on the sacrifices and contributions made by generations of African Americans this month, let us resolve to continue our march toward a day when every person knows the unalienable rights to life, liberty, and the pursuit of happiness. 
    
    Over the course of the past year, Michelle and I have been working with an extraordinary team to dream up a campus for active citizenship on Chicago’s South Side: The Obama Presidential Center. 
    
    Today, we’re taking a significant step forward and I wanted to share the very latest: Obama.org/The-Center 
    
    During my presidency, I started a tradition of sharing my reading lists and playlists. It was a nice way to reflect on the works that resonated with me and lift up authors and artists from around the world. With some extra time on my hands this year to catch up, I wanted to share the books and music that I enjoyed most. From songs that got me moving to stories that inspired me, here's my 2017 list — I hope you enjoy it and have a happy and healthy New Year. 
    
    The best books I read in 2017:
    The Power by Naomi Alderman
    Grant by Ron Chernow
    Evicted: Poverty and Profit in the American City by Matthew Desmond 
    Janesville: An American Story by Amy Goldstein
    Exit West by Mohsin Hamid 
    Five-Carat Soul by James McBride 
    Anything Is Possible by Elizabeth Strout
    Dying: A Memoir by Cory Taylor
    A Gentleman in Moscow by Amor Towles
    Sing, Unburied, Sing by Jesmyn Ward
    *Bonus for hoops fans: Coach Wooden and Me by Kareem Abdul-Jabbar and Basketball (and Other Things) by Shea Serrano
     
    My favorite songs of 2017:
    Mi Gente by J Balvin & Willy William 
    Havana by Camila Cabello (feat. Young Thug)
    Blessed by Daniel Caesar 
    The Joke by Brandi Carlile
    First World Problems by Chance The Rapper (feat. Daniel Caesar)
    Rise Up by Andra Day
    Wild Thoughts by DJ Khaled (feat. Rihanna and Bryson Tiller)
    Family Feud by Jay-Z (feat. Beyoncé)
    Humble by Kendrick Lamar
    La Dame et Ses Valises by Les Amazones d’Afrique (feat. Nneka)
    Unforgettable by French Montana (feat. Swae Lee)
    The System Only Dreams in Total Darkness by The National
    Chanel by Frank Ocean 
    Feel It Still by Portugal. The Man
    Butterfly Effect by Travis Scott
    Matter of Time by Sharon Jones & the Dap-Kings
    Little Bit by Mavis Staples
    Millionaire by Chris Stapleton
    Sign of the Times by Harry Styles 
    Broken Clocks by SZA
    Ordinary Love (Extraordinary Mix) by U2
    *Bonus: Born in the U.S.A. by Bruce Springsteen (not out yet, but the blues version in his Broadway show is the best!) 
    
    On behalf of the Obama family, Merry Christmas! We wish you joy and peace this holiday season. 
    
    
    
    In the page of  Humans of New York  we collected  150  posts
    
    The latest 5 posts in this page are: 
    
    “I feel like God has given me a talent for art, and using that talent is a form of gratitude.  For years I worked in an automotive workshop.  I’d try to paint when I came home, but I never had any energy left.  So I finally quit.  Now I have more freedom to develop my talents, but it hasn’t been easy.  Art is more difficult in this country.  It’s not really seen as a profession.  It would be easier if I was alone, but I have a family to support so there’s a lot of pressure to earn.  I’m trying to be tough in poverty.  I read a lot of poetry to give me strength.  I would never beg, so right now I’m drawing portraits and caricatures to support myself.  I’d rather be painting what I think and feel, but I have to fulfill what the consumer wants.”
    
    (Jakarta, Indonesia) 
    
    “Mom raised me on her own since I was young.  My dad didn’t care about our family.  He was there physically, but he wasn’t there.  He kept all his income for himself.  He gambled away our possessions.  We couldn’t buy anything for ourselves because he would sell it.  He sold all our electronics.  He even sold our furniture.  But my mother still did everything for him: cooked his food, did his laundry… everything.  Then one day she came home from work and the house was empty.  There was nothing left but our clothes.  I told her it was time to go.  For the last two years we’ve been living in a rented house together.  Mom seems so much lighter now.  We can own our own things.  We can actually make progress and move forward in life.  I've started working at a pharmacy and I'm paying my way through college.  I’m still in touch with my father.  I even give him money for rent and food.  I have no interest in taking revenge.  I’m showing him how he should have treated us.”      
    
    (Jakarta, Indonesia) 
    
    “I’m a fund manager.  One of my employees stole millions from my clients.  He hid it from me for almost two years.  He was working with a broker to create fake transactions.  He disappeared when I discovered the scheme.  I could call the police but it would be all over the press, and I’d never get another client.  I could declare bankruptcy but I’m not a coward.  So I’m trying to pay my clients back the initial amount they invested.  I sold my other business.  I sold two houses and a car.  That’s why I’m taking trains and online taxis.  Some of my clients feel sorry for me.  Most of them are angry.  A few hate me.  All of them keep calling.  I tell them to give me time, but they keep calling.  I’m smoking four packs of cigarettes a day because of the stress.  I’ve got to find fresh money.  If I can get new clients, then I can slowly repay the old ones with the profits that I make.  But all of them want their money now.”  
    
    (Jakarta, Indonesia) 
    
    “Several years ago my father used our house for collateral on a business loan.  When things fell apart, we lost everything.  My dad got depressed and fell sick.  My mother was backed into a corner.  She had four young children but no income.  At the time she was a very feminine housewife.  But she asked our neighbors to teach her how to farm.  Then she convinced our landlord to let her plant an empty field.  He told her, ‘If you can make it yield anything, we’ll split the profits.’  It didn’t succeed right away.  There were a lot of pests.  Some of the crops dried out.  But my mother harvested whatever was left, and it was enough to support our family.  She cut off her hair during this time.  She got very muscular.  She worked all day in the fields but still found time to make sure we went to school.  If my brothers and I ever started crying, she’d tell us: ‘If I’m strong enough to do this, you’re strong enough to go to school.’”    
    
    (Jakarta, Indonesia) 
    
    “I tried to pick my wife up from her religious group one night, but her friends told me that she’d left with someone else.  She told me the guy was just a friend.  But over the next few months, I started to hear more and more things.  Somebody saw her coming out of a hotel with another man.  Then a few weeks later I discovered them on a beach together.  Then finally I came home early one day and found them in my house.  I was heartbroken.  Because I really did love her.  I left town immediately after the divorce because I didn’t want to be reminded of the heartbreak.  I’ve lived in Jakarta for three years now.  I have my own shop.  I’ve saved a lot of money.  I’m doing better than I ever have before.  My plan was to become successful and win my wife back, but my daughter tells me that she’s already remarried.  So I’m hoping to meet someone else.  One of my customers was left by her husband.  She also has older children.  We’ve been talking on the phone a lot lately.”      
    
    (Jakarta, Indonesia) 
    
    
    
    In the page of  Harvard University  we collected  150  posts
    
    The latest 5 posts in this page are: 
    
    Firefly, a new microscope from Harvard's Cohen Lab, can show how neurons pulse, communicate, and shine. The microscope's large field of view and fast imaging capability allows it to image electrical signals quickly traveling from neuron to neuron.⠀
    ⠀
    The images could illuminate how disorders like epilepsy and Alzheimer’s disease affect neuron communication and thus enable researchers to discover how to prevent and treat a host of neurological diseases.⠀
    ⠀
    Here, each bright dot represents one neuron in the brain of a genetically modified mouse.⠀
    ⠀
    Credit: Vaibhav Joshi 
    
    At Harvard Business School's "Crossover into Business" program, athletes from the WNBA, NBA, and NFL worked with their M.B.A. student-mentors discussing the "LeBron James" case and endorsement opportunities. 
    
    73°F in 360°! 
    
    Inside the Harvard Semitic Museum, a new exhibit blends important artifacts with immersive technology. Take a look! 
    
    Beautiful weather has arrived! David Chang '17 "rescues" Waverly He '18 from being holed up inside the Northwest Building writing her thesis on an unseasonably warm February day.
    
    Oh, and that's a 21-foot killer whale skeleton in the background.
    
    Photo: Rose Lincoln/Harvard Staff Photographer 
    
    
    
    In the page of  Fox news  we collected  150  posts
    
    The latest 5 posts in this page are: 
    
    Eric Trump and Charlie Kirk speak at CPAC 2018. 
    
    During a fight late last year, a family friend told a dispatcher that Nikolas Cruz “bought a gun from Dick’s last week and is now going to pick it up,” according to the 911 call log. She added Cruz had “bought tons of ammo,” “used a gun against (people) before” and "has put the gun to others [sic] heads in the past.” 
    
    President Donald J. Trump on Thursday floated the possibility of paying teachers a “bonus” to carry concealed firearms, stressing the importance of school security even as he calls for strengthening gun laws in the wake of the Parkland mass shooting. 
    
    BREAKING NEWS: A federal grand jury filed a 32-count indictment Thursday against former Trump campaign chairman Paul Manafort and aide Rick Gates.
    
    Both men are charged with multiple counts of preparing false tax returns, bank fraud, conspiracy to commit bank fraud and failure to disclose income from foreign sources. 
    
    Coral Springs Police hold a press conference on last week's school shooting in Parkland, Florida.
    
    (Credit: WFOR) 
    
    
    
    

Lets look at minumun,maximun and average length of the posts.


    minLength = len(TrumpPosts[0])
    maxLength = 0
    avg = 0
    count = 0
    sumLength = 0
    for page in allposts:
        for post in page:
            count = count + 1
            sumLength = sumLength + len(post)
            if(len(post) < minLength):
                minLength = len(post)
            if(len(post)>maxLength):
                maxLength = len(post)
    avg = sumLength/count
    print("The longest post is ", maxLength," characters long\n")
    print("The shortest post is ", minLength," characters long\n")
    print("The avergae length of the posts is ", avg," characters long\n")

    The longest post is  5599  characters long
    
    The shortest post is  7  characters long
    
    The avergae length of the posts is  249.08944281524927  characters long
    
    

## Step 2 - Creating a classifier for the different pages

### Data cleaning

We start by cleaning the posts' text.
First, we remove non english letters since we are collecting posts from offical facebook pages, and therefore punctuation such as ":)" is not likely to be found.


    import re
    onlyEnglish=[]
    for page in allposts:
        englishPagePost=[]
        for post in page:
            englishPost = re.sub("[^a-zA-Z]", " ",post)
            englishPagePost.append(englishPost)
        onlyEnglish.append(englishPagePost)

Lets look at the first post of every page after this stage:


    for page in onlyEnglish:
        print(page[0])
        print("\n")

    Today  it was my great honor to host a School Safety Roundtable at the White House with State and local leaders  law enforcement officers  and education officials   There is nothing more important than protecting our children  They deserve to be safe  and we will deliver 
    
    
    Today  Kehinde Wiley and Amy Sherald became the first black artists to create official presidential portraits for the Smithsonian  To call this experience humbling would be an understatement    Thanks to Kehinde and Amy  generations of Americans   and young people from all around the world   will visit the National Portrait Gallery and see this country through a new lens  They ll walk out of that museum with a better sense of the America we all love  Clear eyed  Big hearted  Inclusive and optimistic   And I hope they ll walk out more empowered to go and change their worlds 
    
    
     I feel like God has given me a talent for art  and using that talent is a form of gratitude   For years I worked in an automotive workshop   I d try to paint when I came home  but I never had any energy left   So I finally quit   Now I have more freedom to develop my talents  but it hasn t been easy   Art is more difficult in this country   It s not really seen as a profession   It would be easier if I was alone  but I have a family to support so there s a lot of pressure to earn   I m trying to be tough in poverty   I read a lot of poetry to give me strength   I would never beg  so right now I m drawing portraits and caricatures to support myself   I d rather be painting what I think and feel  but I have to fulfill what the consumer wants     Jakarta  Indonesia 
    
    
    Firefly  a new microscope from Harvard s Cohen Lab  can show how neurons pulse  communicate  and shine  The microscope s large field of view and fast imaging capability allows it to image electrical signals quickly traveling from neuron to neuron     The images could illuminate how disorders like epilepsy and Alzheimer s disease affect neuron communication and thus enable researchers to discover how to prevent and treat a host of neurological diseases     Here  each bright dot represents one neuron in the brain of a genetically modified mouse     Credit  Vaibhav Joshi
    
    
    Eric Trump and Charlie Kirk speak at CPAC      
    
    
    

We continue to clean the data by converting the posts to lower case, and split each post to words.


    lowerCaseWords=[]
    for page in onlyEnglish:
        pagePostsWords=[]
        for post in page:
            words = post.lower().split()                 
            pagePostsWords.append(words)
        lowerCaseWords.append(pagePostsWords)

Lets look at the first post of every page after this stage:


    for page in lowerCaseWords:
        print(page[0])
        print("\n")

    ['today', 'it', 'was', 'my', 'great', 'honor', 'to', 'host', 'a', 'school', 'safety', 'roundtable', 'at', 'the', 'white', 'house', 'with', 'state', 'and', 'local', 'leaders', 'law', 'enforcement', 'officers', 'and', 'education', 'officials', 'there', 'is', 'nothing', 'more', 'important', 'than', 'protecting', 'our', 'children', 'they', 'deserve', 'to', 'be', 'safe', 'and', 'we', 'will', 'deliver']
    
    
    ['today', 'kehinde', 'wiley', 'and', 'amy', 'sherald', 'became', 'the', 'first', 'black', 'artists', 'to', 'create', 'official', 'presidential', 'portraits', 'for', 'the', 'smithsonian', 'to', 'call', 'this', 'experience', 'humbling', 'would', 'be', 'an', 'understatement', 'thanks', 'to', 'kehinde', 'and', 'amy', 'generations', 'of', 'americans', 'and', 'young', 'people', 'from', 'all', 'around', 'the', 'world', 'will', 'visit', 'the', 'national', 'portrait', 'gallery', 'and', 'see', 'this', 'country', 'through', 'a', 'new', 'lens', 'they', 'll', 'walk', 'out', 'of', 'that', 'museum', 'with', 'a', 'better', 'sense', 'of', 'the', 'america', 'we', 'all', 'love', 'clear', 'eyed', 'big', 'hearted', 'inclusive', 'and', 'optimistic', 'and', 'i', 'hope', 'they', 'll', 'walk', 'out', 'more', 'empowered', 'to', 'go', 'and', 'change', 'their', 'worlds']
    
    
    ['i', 'feel', 'like', 'god', 'has', 'given', 'me', 'a', 'talent', 'for', 'art', 'and', 'using', 'that', 'talent', 'is', 'a', 'form', 'of', 'gratitude', 'for', 'years', 'i', 'worked', 'in', 'an', 'automotive', 'workshop', 'i', 'd', 'try', 'to', 'paint', 'when', 'i', 'came', 'home', 'but', 'i', 'never', 'had', 'any', 'energy', 'left', 'so', 'i', 'finally', 'quit', 'now', 'i', 'have', 'more', 'freedom', 'to', 'develop', 'my', 'talents', 'but', 'it', 'hasn', 't', 'been', 'easy', 'art', 'is', 'more', 'difficult', 'in', 'this', 'country', 'it', 's', 'not', 'really', 'seen', 'as', 'a', 'profession', 'it', 'would', 'be', 'easier', 'if', 'i', 'was', 'alone', 'but', 'i', 'have', 'a', 'family', 'to', 'support', 'so', 'there', 's', 'a', 'lot', 'of', 'pressure', 'to', 'earn', 'i', 'm', 'trying', 'to', 'be', 'tough', 'in', 'poverty', 'i', 'read', 'a', 'lot', 'of', 'poetry', 'to', 'give', 'me', 'strength', 'i', 'would', 'never', 'beg', 'so', 'right', 'now', 'i', 'm', 'drawing', 'portraits', 'and', 'caricatures', 'to', 'support', 'myself', 'i', 'd', 'rather', 'be', 'painting', 'what', 'i', 'think', 'and', 'feel', 'but', 'i', 'have', 'to', 'fulfill', 'what', 'the', 'consumer', 'wants', 'jakarta', 'indonesia']
    
    
    ['firefly', 'a', 'new', 'microscope', 'from', 'harvard', 's', 'cohen', 'lab', 'can', 'show', 'how', 'neurons', 'pulse', 'communicate', 'and', 'shine', 'the', 'microscope', 's', 'large', 'field', 'of', 'view', 'and', 'fast', 'imaging', 'capability', 'allows', 'it', 'to', 'image', 'electrical', 'signals', 'quickly', 'traveling', 'from', 'neuron', 'to', 'neuron', 'the', 'images', 'could', 'illuminate', 'how', 'disorders', 'like', 'epilepsy', 'and', 'alzheimer', 's', 'disease', 'affect', 'neuron', 'communication', 'and', 'thus', 'enable', 'researchers', 'to', 'discover', 'how', 'to', 'prevent', 'and', 'treat', 'a', 'host', 'of', 'neurological', 'diseases', 'here', 'each', 'bright', 'dot', 'represents', 'one', 'neuron', 'in', 'the', 'brain', 'of', 'a', 'genetically', 'modified', 'mouse', 'credit', 'vaibhav', 'joshi']
    
    
    ['eric', 'trump', 'and', 'charlie', 'kirk', 'speak', 'at', 'cpac']
    
    
    

Now, we will remove stop words from our posts.


    import nltk
    #nltk.download()


    from nltk.corpus import stopwords
    noStopWords=[]
    for page in lowerCaseWords:
        pagePostsWithoutStopWords=[]
        for post in page:
            words = [w for w in post if not w in stopwords.words("english")]                 
            pagePostsWithoutStopWords.append(words)
        noStopWords.append(pagePostsWithoutStopWords)

lets look at the first post of every page after this stage:


    for page in noStopWords:
        print(page[0])
        print("\n")

    ['today', 'great', 'honor', 'host', 'school', 'safety', 'roundtable', 'white', 'house', 'state', 'local', 'leaders', 'law', 'enforcement', 'officers', 'education', 'officials', 'nothing', 'important', 'protecting', 'children', 'deserve', 'safe', 'deliver']
    
    
    ['today', 'kehinde', 'wiley', 'amy', 'sherald', 'became', 'first', 'black', 'artists', 'create', 'official', 'presidential', 'portraits', 'smithsonian', 'call', 'experience', 'humbling', 'would', 'understatement', 'thanks', 'kehinde', 'amy', 'generations', 'americans', 'young', 'people', 'around', 'world', 'visit', 'national', 'portrait', 'gallery', 'see', 'country', 'new', 'lens', 'walk', 'museum', 'better', 'sense', 'america', 'love', 'clear', 'eyed', 'big', 'hearted', 'inclusive', 'optimistic', 'hope', 'walk', 'empowered', 'go', 'change', 'worlds']
    
    
    ['feel', 'like', 'god', 'given', 'talent', 'art', 'using', 'talent', 'form', 'gratitude', 'years', 'worked', 'automotive', 'workshop', 'try', 'paint', 'came', 'home', 'never', 'energy', 'left', 'finally', 'quit', 'freedom', 'develop', 'talents', 'easy', 'art', 'difficult', 'country', 'really', 'seen', 'profession', 'would', 'easier', 'alone', 'family', 'support', 'lot', 'pressure', 'earn', 'trying', 'tough', 'poverty', 'read', 'lot', 'poetry', 'give', 'strength', 'would', 'never', 'beg', 'right', 'drawing', 'portraits', 'caricatures', 'support', 'rather', 'painting', 'think', 'feel', 'fulfill', 'consumer', 'wants', 'jakarta', 'indonesia']
    
    
    ['firefly', 'new', 'microscope', 'harvard', 'cohen', 'lab', 'show', 'neurons', 'pulse', 'communicate', 'shine', 'microscope', 'large', 'field', 'view', 'fast', 'imaging', 'capability', 'allows', 'image', 'electrical', 'signals', 'quickly', 'traveling', 'neuron', 'neuron', 'images', 'could', 'illuminate', 'disorders', 'like', 'epilepsy', 'alzheimer', 'disease', 'affect', 'neuron', 'communication', 'thus', 'enable', 'researchers', 'discover', 'prevent', 'treat', 'host', 'neurological', 'diseases', 'bright', 'dot', 'represents', 'one', 'neuron', 'brain', 'genetically', 'modified', 'mouse', 'credit', 'vaibhav', 'joshi']
    
    
    ['eric', 'trump', 'charlie', 'kirk', 'speak', 'cpac']
    
    
    

Finally, we join the word list back into a sentence


    cleanedPosts=[]
    for page in noStopWords:
        pageCleanedPosts=[]
        for post in page:
            cleaned = " ".join(post)                 
            pageCleanedPosts.append(cleaned)
        cleanedPosts.append(pageCleanedPosts)

Lets see our fully cleaned posts, the first from each page:


    for page in cleanedPosts:
        print(page[0])
        print("\n")

    today great honor host school safety roundtable white house state local leaders law enforcement officers education officials nothing important protecting children deserve safe deliver
    
    
    today kehinde wiley amy sherald became first black artists create official presidential portraits smithsonian call experience humbling would understatement thanks kehinde amy generations americans young people around world visit national portrait gallery see country new lens walk museum better sense america love clear eyed big hearted inclusive optimistic hope walk empowered go change worlds
    
    
    feel like god given talent art using talent form gratitude years worked automotive workshop try paint came home never energy left finally quit freedom develop talents easy art difficult country really seen profession would easier alone family support lot pressure earn trying tough poverty read lot poetry give strength would never beg right drawing portraits caricatures support rather painting think feel fulfill consumer wants jakarta indonesia
    
    
    firefly new microscope harvard cohen lab show neurons pulse communicate shine microscope large field view fast imaging capability allows image electrical signals quickly traveling neuron neuron images could illuminate disorders like epilepsy alzheimer disease affect neuron communication thus enable researchers discover prevent treat host neurological diseases bright dot represents one neuron brain genetically modified mouse credit vaibhav joshi
    
    
    eric trump charlie kirk speak cpac
    
    
    

### Moving to Structured Data - Bag of words

Now that we have all data in a clean format, we can represent it as a bag of words in order to build our classifier.
We chose max features to be 400 after we tried different numbers and noticed the results.


    from sklearn.feature_extraction.text import CountVectorizer
    vectorizer = CountVectorizer(analyzer = "word",tokenizer = None,preprocessor = None,stop_words = None, max_features = 400)
    labelsForPosts=[]
    index = 0
    for page in cleanedPosts:
        for post in page:
            labelsForPosts.append(index)
        index= index+1
    allCleanedPosts = cleanedPosts[0]+cleanedPosts[1]+cleanedPosts[2]+cleanedPosts[3]+cleanedPosts[4]
    train_data_features = vectorizer.fit_transform(allCleanedPosts)
    train_data_features = train_data_features.toarray()

### Building the classifier

First we split the data into train and test sets. Then, we will try three different classifiers and select the best one.


    import numpy
    msk = numpy.random.rand(len(allCleanedPosts)) < 0.8
    index = 0
    train_x = []
    train_y = []
    test_x = []
    test_y = []
    
    for post in train_data_features:
        if(msk[index]):
            train_x.append(train_data_features[index])
            train_y.append(labelsForPosts[index])   
        else:
            test_x.append(train_data_features[index])
            test_y.append(labelsForPosts[index])   
        index = index + 1

###Random Forest

Training Random Forest model:


    from sklearn.ensemble import RandomForestClassifier
    
    forest = RandomForestClassifier(n_estimators = 90) 
    forest = forest.fit(numpy.vstack(train_x), numpy.ravel(numpy.vstack(train_y )))
    ForestScore = forest.score(numpy.vstack(test_x),numpy.ravel(numpy.vstack(test_y)))
    ForestScore




    0.72661870503597126



###SVM

Training SVM model:


    from sklearn import svm
    SvmModel = svm.SVC()
    SvmModel.fit(train_x, train_y)
    SVMScore = SvmModel.score(numpy.vstack(test_x), numpy.vstack(test_y))
    SVMScore




    0.30935251798561153



###KNN

Training KNN model:


    from sklearn.neighbors import KNeighborsClassifier
    KNNModel = KNeighborsClassifier(n_neighbors=10)
    KNNModel.fit(train_x, train_y)
    KNNScore = KNNModel.score(numpy.vstack(test_x), numpy.vstack(test_y))
    KNNScore




    0.57553956834532372



Lets Comapre the results and decide on our finale classifier:


    import matplotlib.pyplot as plt;
    %matplotlib inline
    import numpy as np
    import matplotlib.pyplot as plt
     
    objects = ('KNN', 'SVM', 'RandomForest')
    y_pos = np.arange(len(objects))
    performance = [KNNScore,SVMScore,ForestScore]
     
    plt.bar(y_pos, performance, align='center', alpha=0.5)
    plt.xticks(y_pos, objects)
    plt.ylabel('Percision(%)')
    plt.title('Algorithm')
     
    plt.show()


![png](/Images/output_59_0.png)


As we can see, Random forset has the best results and thus we will use it as our classifier in step 4.

## Step 3 - Generating posts


    #!pip install keras==1.2
    #!pip install theano
    #from keras import backend as K
    #K.set_image_dim_ordering('th')
    import keras
    print(keras.__version__)
    import theano

    1.2.0
    

We define the following tokens and a vocabulary of size 600, to get only most common words:


    vocabulary_size = 600
    next_post_toekn = "NEXTPOST"
    unknown_token = "UNKNOWNTOKEN"
    sentence_start_token = "SENTENCESTART"
    sentence_end_token = "SENTENCEEND"
    line_break= "NEWLINE"
    separator= "SEPARATOR"

Next, we insert the above tokens into all of the posts: 


    trumpPostsCombined = ''
    obamaPostsCombined = ''
    honyPostsCombined = ''
    harvardPostsCombined = ''
    foxPostsCombined = ''
    allpostsCombined = [trumpPostsCombined,obamaPostsCombined,honyPostsCombined,harvardPostsCombined,foxPostsCombined]
    index = 0
    for page in allposts:
        for post in page:
            post_with_tokens = next_post_toekn + ' ' + sentence_start_token + " " + post
            post_with_tokens = post_with_tokens.replace('\n',' '+ line_break + ' ')
            post_with_tokens = post_with_tokens.replace('--',' '+ separator + ' ')
            post_with_tokens = post_with_tokens.replace('.',' '+sentence_end_token +' '+ sentence_start_token+' ' )
            allpostsCombined[index] = allpostsCombined[index] + ' ' + post_with_tokens
        allpostsCombined[index] = allpostsCombined[index][1:]
        index +=1
    allpostsCombined




    ['NEXTPOST SENTENCESTART Today, it was my great honor to host a School Safety Roundtable at the White House with State and local leaders, law enforcement officers, and education officials SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE There is nothing more important than protecting our children SENTENCEEND SENTENCESTART  They deserve to be safe, and we will deliver! NEXTPOST SENTENCESTART History shows that a school shooting lasts, on average, 3 minutes SENTENCEEND SENTENCESTART  It takes police & first responders approximately 5 to 8 minutes to get to site of crime SENTENCEEND SENTENCESTART  Highly trained, gun adept, teachers/coaches would solve the problem instantly, before police arrive SENTENCEEND SENTENCESTART  GREAT DETERRENT! NEXTPOST SENTENCESTART I will be strongly pushing Comprehensive Background Checks with an emphasis on Mental Health SENTENCEEND SENTENCESTART  Raise age to 21 and end sale of Bump Stocks! NEXTPOST SENTENCESTART Question: If all of the Russian meddling took place during the Obama Administration, right up to January 20th, why aren’t they the subject of the investigation? NEXTPOST SENTENCESTART President Donald J SENTENCEEND SENTENCESTART  Trump\'s schedule for Thursday, February 22nd: NEWLINE · Meeting with State and local officials on school safety NEXTPOST SENTENCESTART I will always remember the time I spent today with courageous students, teachers and families SENTENCEEND SENTENCESTART  So much love in the midst of so much pain SENTENCEEND SENTENCESTART  We must not let them down SENTENCEEND SENTENCESTART  We must keep our children safe!! NEXTPOST SENTENCESTART Statement on the Passing of Reverend Billy Graham: NEWLINE  NEWLINE Melania and I join millions of people around the world in mourning the passing of Billy Graham SENTENCEEND SENTENCESTART  Our prayers are with his children, grandchildren, great-grandchildren and all who worked closely with Reverend Graham in his lifelong ministry SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE Billy’s acceptance of Jesus Christ around his seventeenth birthday not only changed his life—it changed our country and the world SENTENCEEND SENTENCESTART  He was one of the towering figures of the last 100 years—an American hero whose life and leadership truly earned him the title “God’s Ambassador SENTENCEEND SENTENCESTART ”  NEWLINE   NEWLINE Billy’s unshakeable belief in the power of God’s word to transform hearts gave hope to all who listened to his simple message: “God loves you SENTENCEEND SENTENCESTART ” He carried this message around the world through his crusades, bringing entire generations to faith in Jesus Christ SENTENCEEND SENTENCESTART    NEWLINE   NEWLINE In the wake of the September 11th attacks in 2001, America turned to Billy Graham at the National Cathedral, who told us, “God can be trusted, even when life seems at its darkest SENTENCEEND SENTENCESTART ” NEWLINE   NEWLINE Reverend Graham would be the first to say that he did not do it alone SENTENCEEND SENTENCESTART  Before her passing, his wife Ruth was by his side through it all—a true partner, a wonderful mother, and a fellow missionary soul SENTENCEEND SENTENCESTART  He also built an international team and institution that will continue to carry on Christ’s message SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE Melania and I were privileged to get to know Reverend Graham and his extraordinary family over the last several years, and we are deeply grateful for their love and support SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE Billy Graham was truly one of a kind SENTENCEEND SENTENCESTART  Christians and people of all faiths and backgrounds will miss him dearly SENTENCEEND SENTENCESTART  We are thinking of him today, finally at home in Heaven SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Bad ratings CNN and MSNBC got scammed when they covered the anti-Trump Russia rally wall-to-wall SENTENCEEND SENTENCESTART  Two really dishonest newscasters, but the public is wise! NEXTPOST SENTENCESTART The GREAT Billy Graham is dead SENTENCEEND SENTENCESTART  There was nobody like him! He will be missed by Christians and all religions SENTENCEEND SENTENCESTART  A very special man SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART When faced with danger, they each put the lives of others before their own SENTENCEEND SENTENCESTART  God forever bless these GREAT heroes! NEXTPOST SENTENCESTART President Donald J SENTENCEEND SENTENCESTART  Trump\'s schedule for Wednesday, February 21st: NEWLINE · Discuss the Economic Report of the President with the Council of Economic Advisers NEWLINE · Lunch with the Administrator of the Small Business Administration NEWLINE · Meeting with trade union leaders NEWLINE · Listening session with high school students and teachers NEXTPOST SENTENCESTART The U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  economy is looking very good, in my opinion, even better than anticipated SENTENCEEND SENTENCESTART  JOBS, JOBS, JOBS! NEXTPOST SENTENCESTART Republicans are now leading the Generic Poll, perhaps because of the popular Tax Cuts which the Dems want to take away! NEXTPOST SENTENCESTART Main Street is BOOMING thanks to our incredible TAX CUT and Reform law SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE "This shows small-business owners are more than just optimistic, they are ready to grow their businesses SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Thank you to Fox & Friends for the great timeline on all of the failures the Obama Administration had against Russia, including Crimea, Syria and so much more SENTENCEEND SENTENCESTART  We are now starting to win again! NEXTPOST SENTENCESTART President Donald J SENTENCEEND SENTENCESTART  Trump’s schedule for Tuesday, February 20th: NEWLINE · Lunch with Vice President Mike Pence and Secretary of Defense Jim Mattis NEWLINE · Meeting with Secretary of the Treasury Steve Mnuchin NEWLINE · Public Safety Medal of Valor Awards Ceremony NEXTPOST SENTENCESTART You wouldn’t know it by watching the Fake News media, but our economy is BOOMING! NEXTPOST SENTENCESTART Obama was President up to, and beyond, the 2016 Election SENTENCEEND SENTENCESTART  So why didn’t he do something about Russian meddling? NEXTPOST SENTENCESTART Have a great, but very reflective, President’s Day! NEXTPOST SENTENCESTART This President’s Day, I honor all past presidents and reaffirm my commitment to finish the job of those who served before me by making America GREAT again SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART If it was the GOAL of Russia to create discord, disruption and chaos within the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  then, with all of the Committee Hearings, Investigations and Party hatred, they have succeeded beyond their wildest dreams SENTENCEEND SENTENCESTART  They are laughing their asses off in Moscow SENTENCEEND SENTENCESTART  Get smart America! NEXTPOST SENTENCESTART Our entire Nation, with one heavy heart, continues to pray for the victims & their families in Parkland, FL SENTENCEEND SENTENCESTART  To teachers, law enforcement, first responders & medical professionals who responded so bravely in the face of danger: We THANK YOU for your courage! NEXTPOST SENTENCESTART Our entire nation, with one heavy heart, is praying for the victims, their families, and the entire Parkland, FL community SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART On the night of my election, I pledged to be president for ALL Americans, and I’ve upheld that pledge every day since SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART No matter our beliefs, no matter our party, it\'s time for us to remember that we are ALL AMERICANS and we are in this TOGETHER SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Today, it was a tremendous honor for me to sign the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  Department of Veterans Affairs Accountability Act into law, delivering on my promise to our American HEROES! NEXTPOST SENTENCESTART The Fake News won’t tell you about the many Legislative Bills we\'ve passed, or the massive amounts of bad regulations we\'ve eliminated! Mostly with no Democratic support SENTENCEEND SENTENCESTART  Nice! NEXTPOST SENTENCESTART First Lady Melania Trump and I were honored to host our first White House Congressional Picnic SENTENCEEND SENTENCESTART  A wonderful evening and tradition SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART History is written by the DREAMERS, not the doubters SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Why did the Democratic National Committee turn down the Department of Homeland Security’s offer to protect against hacks and REFUSE to turn over its server to the FBI (and still hasn’t)? It’s all a big Dem HOAX! #DrainTheSwamp NEXTPOST SENTENCESTART Great night in Iowa - special people SENTENCEEND SENTENCESTART  Thank you! NEXTPOST SENTENCESTART WATCH LIVE - President Donald J SENTENCEEND SENTENCESTART  Trump\'s rally in Cedar Rapids, IA! NEXTPOST SENTENCESTART Democrats would do much better as a party if they got together with Republicans on Healthcare, Tax Cuts, Security SENTENCEEND SENTENCESTART  Obstruction doesn\'t work! NEXTPOST SENTENCESTART It was a great honor to welcome President Petro Poroshenko of Ukraine to the White House yesterday with Vice President Mike Pence SENTENCEEND SENTENCESTART   NEWLINE —> http://45 SENTENCEEND SENTENCESTART wh SENTENCEEND SENTENCESTART gov/zfnsbA NEXTPOST SENTENCESTART We’re embracing big change, bold thinking, and outsider perspectives to transform government and make it the way it should be SENTENCEEND SENTENCESTART  #TechWeek NEXTPOST SENTENCESTART The Democrats don’t want you to know how much we’ve accomplished SENTENCEEND SENTENCESTART  While they do nothing but obstruct and distract, we continue to work hard on fulfilling our promises to YOU! NEXTPOST SENTENCESTART Melania and I were honored to welcome President Juan Carlos Varela and Mrs SENTENCEEND SENTENCESTART  Varela of Panama to the White House today SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The MAKE AMERICA GREAT AGAIN agenda is doing very well despite the distraction of the Fake News! NEXTPOST SENTENCESTART I am canceling the last administration’s completely one-sided deal with Cuba, and seeking a MUCH better deal for the Cuban people and for the United States of America SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Now that I am President, America will expose the crimes of the Castro regime and stand with the Cuban people in their struggle for freedom SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Despite the phony witch hunt going on in America, we are doing GREAT! Regulations way down, jobs and enthusiasm way up! #MAGA NEXTPOST SENTENCESTART We are all united by our love of our GREAT and beautiful country SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Today we celebrate the dignity of work and the greatness of the American worker SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Expanding Apprenticeships in America: http://bit SENTENCEEND SENTENCESTART ly/2stwYia NEXTPOST SENTENCESTART You are witnessing the single greatest WITCH HUNT in American political history — led by some very bad and conflicted people!  #MAGA NEXTPOST SENTENCESTART Melania and I are grateful for the heroism of our first responders and praying for the swift recovery of all victims of this terrible shooting SENTENCEEND SENTENCESTART  Please take a moment today to cherish those you love, and always remember those who serve and keep us safe SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART 2 million more people just dropped out of ObamaCare SENTENCEEND SENTENCESTART  It’s totally broken SENTENCEEND SENTENCESTART  The Obstructionist Democrats gave up, but we’re going to have a real bill, not ObamaCare! NEXTPOST SENTENCESTART The Fake News Media has never been so wrong or so dirty SENTENCEEND SENTENCESTART  Purposely incorrect stories and phony sources to meet their agenda of hate SENTENCEEND SENTENCESTART  Sad! NEXTPOST SENTENCESTART Heading to the Great State of Wisconsin to talk about JOBS, JOBS, JOBS! Big progress being made as the Real News is reporting SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The fake news MSM doesn\'t report the great economic news we\'ve had since Election Day SENTENCEEND SENTENCESTART  All of our hard work is kicking in and we\'re seeing big results! NEXTPOST SENTENCESTART Thank you Richmond, Virginia! I love you & will see you soon! Donald J SENTENCEEND SENTENCESTART  Trump NEXTPOST SENTENCESTART #FlashbackFriday #CrookedHillary NEXTPOST SENTENCESTART I rarely agree with President Obama- however he is 100% correct about Crooked Hillary Clinton SENTENCEEND SENTENCESTART  Great ad! NEXTPOST SENTENCESTART Thank you California, Montana, New Jersey, New Mexico, and South Dakota! I am honored to be your nominee, and I can assure you - - WE will MAKE AMERICA GREAT AGAIN - - TOGETHER! It is time! NEXTPOST SENTENCESTART THANK YOU to my unbelievable family- for their incredible support of my candidacy SEPARATOR  love you all! NEXTPOST SENTENCESTART Remembering the fallen heroes on #DDay - June 6, 1944 SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The values and contributions of America’s Latino community play a significant role in the history and future of our great country SENTENCEEND SENTENCESTART   I am proud to have their growing support SENTENCEEND SENTENCESTART  #LatinosForTrump #Trump2016 NEXTPOST SENTENCESTART Amazing photos from San Jose rally, on the rope line SENTENCEEND SENTENCESTART  I love my supporters (#TrumpTrain) and appreciate your support! Thank you! NEXTPOST SENTENCESTART Muhammad Ali is dead at 74! A truly great champion and a wonderful guy SENTENCEEND SENTENCESTART  He will be missed by all! NEXTPOST SENTENCESTART Al Baldasaro, New Hampshire State Representative and Veteran - attended my press conference on the $5,600,000 SENTENCEEND SENTENCESTART 00 donated to Veterans groups SENTENCEEND SENTENCESTART  He had a few things to say: NEXTPOST SENTENCESTART Our law enforcement officers deserve our appreciation for the incredible job they do SENTENCEEND SENTENCESTART  We have to appreciate & respect our police!  NEWLINE #MakeAmericaGreatAgain NEXTPOST SENTENCESTART When I am elected POTUS, I will develop an America First energy plan SENTENCEEND SENTENCESTART  My plan will create real jobs and real wage growth while protecting the environment SENTENCEEND SENTENCESTART  #Trump2016 NEWLINE  NEWLINE https://www SENTENCEEND SENTENCESTART donaldjtrump SENTENCEEND SENTENCESTART com/press-releases/an-america-first-energy-plan NEXTPOST SENTENCESTART Crooked Hillary gets caught lying again SENTENCEEND SENTENCESTART  #CrookedHillary  #Trump2016 NEXTPOST SENTENCESTART Today we, together, won the Republican Nomination for President! #Trump2016 NEXTPOST SENTENCESTART A suggestion for the dishonest media SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Is Hillary really protecting women? NEXTPOST SENTENCESTART Wow- another great national poll vs SENTENCEEND SENTENCESTART  Crooked Hillary Clinton! We are only just beginning SENTENCEEND SENTENCESTART  It\'s time to MAKE AMERICA SAFE & GREAT AGAIN! NEXTPOST SENTENCESTART The The New York Times never called me about ungrateful former employee Barbara Res SENTENCEEND SENTENCESTART  If they had- I would\'ve shown her many emails begging for her job back! NEXTPOST SENTENCESTART Thank you Lt SENTENCEEND SENTENCESTART  Steven Rogers SENTENCEEND SENTENCESTART  We will respond to terrorism with strength in 2017! NEXTPOST SENTENCESTART Good morning America! Watch Ivanka Trump on CBS This Morning right here! You can see why I am confident in running for President of the United States of America! The Trump Organization will be left in great hands with my kids- I am so proud! It gives me the opportunity to serve you SENTENCEEND SENTENCESTART  Together we will MAKE AMERICA SAFE AND GREAT AGAIN! NEXTPOST SENTENCESTART THANK YOU OREGON- I will never forget it! Great job on Sean Hannity- last night, Donald Trump Jr SENTENCEEND SENTENCESTART  - it is time to MAKE AMERICA GREAT AGAIN!! NEXTPOST SENTENCESTART Crooked Hillary said her husband is going to be in charge of the economy SENTENCEEND SENTENCESTART  If so, he should run, not her SENTENCEEND SENTENCESTART  Will he bring the "energizer" to D SENTENCEEND SENTENCESTART C SENTENCEEND SENTENCESTART ? NEXTPOST SENTENCESTART GOOD MORNING OREGON! I had a wonderful time with you last week SENTENCEEND SENTENCESTART  If you did not mail in your vote - it is not too late! You can still get out and VOTE TRUMP at your local municipal clerk\'s office- by 8pm! Together - we will MAKE AMERICA GREAT AGAIN! NEXTPOST SENTENCESTART Rowanne Brewer, the most prominently depicted woman in the failing New York Times hit piece on me- joined Fox & Friends this morning SENTENCEEND SENTENCESTART  We have exposed the article as a fraud SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Ivanka Trump joined Gayle Kind at the Forbes Women’s Summit- at Chelsea Piers, here in New York City SENTENCEEND SENTENCESTART  Terrific job- Ivanka! NEXTPOST SENTENCESTART I\'m glad President Obama followed my lead and lowered the flags half-staff SENTENCEEND SENTENCESTART  It\'s about time! NEXTPOST SENTENCESTART Donald J SENTENCEEND SENTENCESTART  Trump Orders Flags to be flown at Half-Staff at all Trump Properties in Honor of the Five Fallen Soldiers NEWLINE  NEWLINE Mr SENTENCEEND SENTENCESTART  Trump Takes Action and Shows Commitment to Military and Vets NEWLINE  NEWLINE (New York, NY) July 20th, 2015- Today Donald J SENTENCEEND SENTENCESTART  Trump has ordered all flags at Trump properties around the country to be flown at half-staff in honor of the five fallen soldiers who were tragically killed in last week\'s shooting SENTENCEEND SENTENCESTART  Mr SENTENCEEND SENTENCESTART  Trump has long been a dedicated supporter of our military and our Veterans and if the President won\'t act, he will SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Mr SENTENCEEND SENTENCESTART  Trump stated, "This disgraceful omission is unacceptable and yet another example of our incompetent politicians SENTENCEEND SENTENCESTART  It is a simple yet meaningful and important gesture that signifies our respect and recognition for these great soldiers who lost their lives SENTENCEEND SENTENCESTART  We must do better for all Americans, especially our military SENTENCEEND SENTENCESTART  We must Make America Great Again!" NEWLINE  NEWLINE In addition to the lowering of the flags, Mr SENTENCEEND SENTENCESTART  Trump has established a hotline (855- VETS- 352) and email address (veterans@donaldtrump SENTENCEEND SENTENCESTART com) for Veterans to share their stories about the need to reform our Veterans Administration SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE If he is elected President he will take care of these and all Veteran complaints very quickly and efficiently like a world-class business man can do, but a politician has no clue SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART We will soon be at a point with our incompetent politicians where we will be treating illegal immigrants better than our veterans SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART John McCain called thousands of people "crazies" when they came to seek help on illegal immigration last week in Phoenix SENTENCEEND SENTENCESTART  He owes apology! NEXTPOST SENTENCESTART I am not a fan of John McCain because he has done so little for our Veterans and he should know better than anybody what the Veterans need, especially in regards to the VA SENTENCEEND SENTENCESTART  He is yet another all talk, no action politician who spends too much time on television and not enough time doing his job and helping the Vets SENTENCEEND SENTENCESTART  He is also allowing our military to decrease substantially in size and strength, something which should never be allowed to happen SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Furthermore, he was extremely disrespectful to the thousands upon thousands of people, many of whom happen to be his constituents, that came to listen to me speak about illegal immigration in Phoenix last week by calling them "crazies" SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE These were not "crazies" SEPARATOR -these were great American citizens SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE I have great respect for all those who serve in our military including those that weren\'t captured and are also heroes SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE I want to strengthen our military and take care of our Veterans SENTENCEEND SENTENCESTART   I want to MAKE AMERICA GREAT AGAIN especially for those that serve to protect our freedom SENTENCEEND SENTENCESTART  I am fighting for our Veterans! NEXTPOST SENTENCESTART Donald J SENTENCEEND SENTENCESTART  Trump Response to Huffington Post NEWLINE   NEWLINE Mr SENTENCEEND SENTENCESTART  Trump is number one in the unimportant Huffington Post poll, along with all other recently released polls including Reuters, FOX, USA Today/Suffolk University and The Economist SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE Mr SENTENCEEND SENTENCESTART  Trump is in first place in Nevada, where he is also number one, by a wide margin, with Hispanics SENTENCEEND SENTENCESTART  He is number one in North Carolina and expects to win Iowa and New Hampshire SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE Mr SENTENCEEND SENTENCESTART  Trump singlehandedly raised the issue of illegal immigration and started a national conversation about what has turned out to be one of the most important topics of this election cycle SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE Likewise, Mr SENTENCEEND SENTENCESTART  Trump is the leader on issues such as the terrible United States trade deals, strengthening our military, taking care of our great Vets and the repeal and replacement of Obamacare SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE If you read previously written Tweets, Mr SENTENCEEND SENTENCESTART  Trump has never been a fan of Arianna Huffington or the money-losing Huffington Post SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE The only clown show in this scenario is the Huffington Post pretending to be a legitimate news source SENTENCEEND SENTENCESTART  Mr SENTENCEEND SENTENCESTART  Trump is not focused on being covered by a glorified blog SENTENCEEND SENTENCESTART  He is focused on Making America Great Again SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Get rid of gun free zones SENTENCEEND SENTENCESTART  The four great marines who were just shot never had a chance SENTENCEEND SENTENCESTART  They were highly trained but helpless without guns SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Thoughts and prayers to the families of the four great Marines killed today SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART I am so happy that people are boycotting Macy\'s SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Have some fun with this- NEXTPOST SENTENCESTART The U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  will invite El Chapo, the Mexican drug lord who just escaped prison, to become a U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  citizen because our "leaders" can\'t say no! NEXTPOST SENTENCESTART Can you envision Jeb Bush or Hillary Clinton negotiating with \'El Chapo\', the Mexican drug lord who escaped from prison? Trump, however, on the other hand would kick his ass! NEXTPOST SENTENCESTART Mexico\'s biggest drug lord escapes from jail SENTENCEEND SENTENCESTART  Unbelievable corruption and USA is paying the price SENTENCEEND SENTENCESTART   I told you so! http://www SENTENCEEND SENTENCESTART cnn SENTENCEEND SENTENCESTART com/2015/07/12/world/mexico-el-chapo-escape/ NEXTPOST SENTENCESTART I want to #MakeAmericaGreatAgain NEXTPOST SENTENCESTART Jeb\'s brother George insisted on a $100,000 fee and $20,000 for a private jet to speak at a charity for severely wounded vets SENTENCEEND SENTENCESTART  Not nice! NEWLINE You mean George W SENTENCEEND SENTENCESTART  Bush sends our soldiers into combat, they are severely wounded, and then he wants $120,000 to make a boring speech to them? NEXTPOST SENTENCESTART "Trump leads GOP field in North Carolina" NEWLINE  NEWLINE http://www SENTENCEEND SENTENCESTART publicpolicypolling SENTENCEEND SENTENCESTART com/main/2015/07/trump-leads-gop-field-in-north-carolina SENTENCEEND SENTENCESTART html NEXTPOST SENTENCESTART Happy Fourth of July to everyone! Make America Great Again! NEXTPOST SENTENCESTART Wow, Huffington Post just stated that I am number 1 in the polls of Republican candidates SENTENCEEND SENTENCESTART  Thank you, but the work has just begun! #MakeAmericaGreatAgain NEXTPOST SENTENCESTART Make our borders strong and stop illegal immigration SENTENCEEND SENTENCESTART  Even President Obama agrees- NEXTPOST SENTENCESTART Look what the President of NBC sent me recently about his stay in my Las Vegas hotel SENTENCEEND SENTENCESTART  Very loyal guy SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Statement on Relationship with NBC- NEXTPOST SENTENCESTART Speaking at the City Club of Chicago SENTENCEEND SENTENCESTART  Sold out in minutes with thousands on the wait list! #MakeAmericaGreatAgain NEXTPOST SENTENCESTART Jorge Ramos- Please send me your new number, your old one’s not working SENTENCEEND SENTENCESTART  Sincerely, Donald J SENTENCEEND SENTENCESTART  Trump NEXTPOST SENTENCESTART Addressing record crowd yesterday at Madison County Iowa GOP Dinner SENTENCEEND SENTENCESTART  We can bring common sense to D SENTENCEEND SENTENCESTART C SENTENCEEND SENTENCESTART  & #MakeAmericaGreatAgain! NEXTPOST SENTENCESTART When somebody challenges you unfairly, fight back - be brutal, be tough - don\'t take it SENTENCEEND SENTENCESTART  It is always important to WIN! NEXTPOST SENTENCESTART Wow - they are really killing Jay Leno - let him go out with dignity! NEXTPOST SENTENCESTART Don\'t blindly pursue a career that others suggest or insist is right for you SENTENCEEND SENTENCESTART  It may be worth taking a pay cut for a job you love SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART It won’t stay a buyer’s market forever SENTENCEEND SENTENCESTART  If you can, take advantage and buy property ASAP SENTENCEEND SENTENCESTART  You’ll thank me! NEXTPOST SENTENCESTART Remember the golden rule of negotiating: He who has the gold makes the rules SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART NBC just announced that all 1 hour The Apprentice episodes are being expanded to 2 hours- it’s amazing what good ratings will do! NEXTPOST SENTENCESTART It was great having @ArsenioHall back on this week\'s @ApprenticeNBC! NEXTPOST SENTENCESTART Don\'t forget to watch The Apprentice tonight at 9PM ET on NBC - GREAT EPISODE! NEXTPOST SENTENCESTART Ernie Els and myself at Trump National Doral SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART So, what did you think of my decision? What would you have done? #CelebApprentice NEXTPOST SENTENCESTART I\'ll always like @OMAROSA because she constantly defends me SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART  SENTENCEEND SENTENCESTART @PiersMorgan is right- he won the show because “I know how to play the game SENTENCEEND SENTENCESTART ” #CelebApprentice NEXTPOST SENTENCESTART  SENTENCEEND SENTENCESTART @IvankaTrump looks like a movie star from the days of glamour and beauty SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART Don\'t worry West Coast etc SENTENCEEND SENTENCESTART  we are not going to tweet who was fired or give any indication there of until after it airs SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART Paul Teutul is always good on the show SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART Even @PiersMorgan is impressed by @THEGaryBusey SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART  SENTENCEEND SENTENCESTART @OMAROSA as a cashier- a big mistake by @BrandenRoderick SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART  SENTENCEEND SENTENCESTART @KellyandMichael are both wonderful people SENTENCEEND SENTENCESTART  Their show is terrific SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART  SENTENCEEND SENTENCESTART @DennisRodman must be thinking of North Korea SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART  SENTENCEEND SENTENCESTART @THEGaryBusey is definitely different SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART  SENTENCEEND SENTENCESTART @PiersMorgan and @OMAROSA really hate each other SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART  SENTENCEEND SENTENCESTART @IvankaTrump and @PiersMorgan will be wonderful advisors SENTENCEEND SENTENCESTART  #CelebApprentice NEXTPOST SENTENCESTART I can’t believe he would choose @OMAROSA as his first choice! She is hard to handle SENTENCEEND SENTENCESTART  #CelebApprentice',
     'NEXTPOST SENTENCESTART Today, Kehinde Wiley and Amy Sherald became the first black artists to create official presidential portraits for the Smithsonian SENTENCEEND SENTENCESTART  To call this experience humbling would be an understatement SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE Thanks to Kehinde and Amy, generations of Americans — and young people from all around the world — will visit the National Portrait Gallery and see this country through a new lens SENTENCEEND SENTENCESTART  They’ll walk out of that museum with a better sense of the America we all love SENTENCEEND SENTENCESTART  Clear-eyed SENTENCEEND SENTENCESTART  Big-hearted SENTENCEEND SENTENCESTART  Inclusive and optimistic SENTENCEEND SENTENCESTART   And I hope they’ll walk out more empowered to go and change their worlds SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART This month, we tell the stories of a special set of heroes: The African American community organizers and active citizens who have, throughout our history, brought people together in pursuit of a more just, inclusive, and generous America SENTENCEEND SENTENCESTART  The pioneers who challenged us to dream bigger SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE C SENTENCEEND SENTENCESTART T SENTENCEEND SENTENCESTART  Vivian is one of those heroes SENTENCEEND SENTENCESTART  A Baptist minister, C SENTENCEEND SENTENCESTART T SENTENCEEND SENTENCESTART  Vivian was one of Dr SENTENCEEND SENTENCESTART  King’s closest advisors SENTENCEEND SENTENCESTART  “Martin taught us,” he says, “that it’s in the action that we find out who we really are SENTENCEEND SENTENCESTART ” And time and again, Reverend Vivian was among the first to be in the action SENTENCEEND SENTENCESTART  In 1947, he joined a sit-in to integrate an Illinois restaurant SENTENCEEND SENTENCESTART  He was one of the first Freedom Riders SENTENCEEND SENTENCESTART  In Selma, he stood on the courthouse steps to register blacks to vote  SEPARATOR  and was beaten, bloodied and jailed for it SENTENCEEND SENTENCESTART  In standing up, he helped inspire a young woman to sit down and hold her ground SENTENCEEND SENTENCESTART  Her name was Rosa Parks SENTENCEEND SENTENCESTART  She said of him, “Even after things had supposedly been taken care of and we had our rights, he was still out there, inspiring the next generation, including me SENTENCEEND SENTENCESTART ” NEWLINE   NEWLINE And me SENTENCEEND SENTENCESTART  Without the sacrifices and hard-won battles of folks like C SENTENCEEND SENTENCESTART T SENTENCEEND SENTENCESTART  Vivian, I may have never had the honor to serve as your president SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE And that’s the beauty of this nation: Each new generation stands on the successes of the last  SEPARATOR  and reaches up to bend the arc of history in the direction of more freedom, more opportunity, more justice SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE As we continue to reflect on the sacrifices and contributions made by generations of African Americans this month, let us resolve to continue our march toward a day when every person knows the unalienable rights to life, liberty, and the pursuit of happiness SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Over the course of the past year, Michelle and I have been working with an extraordinary team to dream up a campus for active citizenship on Chicago’s South Side: The Obama Presidential Center SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE Today, we’re taking a significant step forward and I wanted to share the very latest: Obama SENTENCEEND SENTENCESTART org/The-Center NEXTPOST SENTENCESTART During my presidency, I started a tradition of sharing my reading lists and playlists SENTENCEEND SENTENCESTART  It was a nice way to reflect on the works that resonated with me and lift up authors and artists from around the world SENTENCEEND SENTENCESTART  With some extra time on my hands this year to catch up, I wanted to share the books and music that I enjoyed most SENTENCEEND SENTENCESTART  From songs that got me moving to stories that inspired me, here\'s my 2017 list — I hope you enjoy it and have a happy and healthy New Year SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE The best books I read in 2017: NEWLINE The Power by Naomi Alderman NEWLINE Grant by Ron Chernow NEWLINE Evicted: Poverty and Profit in the American City by Matthew Desmond  NEWLINE Janesville: An American Story by Amy Goldstein NEWLINE Exit West by Mohsin Hamid  NEWLINE Five-Carat Soul by James McBride  NEWLINE Anything Is Possible by Elizabeth Strout NEWLINE Dying: A Memoir by Cory Taylor NEWLINE A Gentleman in Moscow by Amor Towles NEWLINE Sing, Unburied, Sing by Jesmyn Ward NEWLINE *Bonus for hoops fans: Coach Wooden and Me by Kareem Abdul-Jabbar and Basketball (and Other Things) by Shea Serrano NEWLINE   NEWLINE My favorite songs of 2017: NEWLINE Mi Gente by J Balvin & Willy William  NEWLINE Havana by Camila Cabello (feat SENTENCEEND SENTENCESTART  Young Thug) NEWLINE Blessed by Daniel Caesar  NEWLINE The Joke by Brandi Carlile NEWLINE First World Problems by Chance The Rapper (feat SENTENCEEND SENTENCESTART  Daniel Caesar) NEWLINE Rise Up by Andra Day NEWLINE Wild Thoughts by DJ Khaled (feat SENTENCEEND SENTENCESTART  Rihanna and Bryson Tiller) NEWLINE Family Feud by Jay-Z (feat SENTENCEEND SENTENCESTART  Beyoncé) NEWLINE Humble by Kendrick Lamar NEWLINE La Dame et Ses Valises by Les Amazones d’Afrique (feat SENTENCEEND SENTENCESTART  Nneka) NEWLINE Unforgettable by French Montana (feat SENTENCEEND SENTENCESTART  Swae Lee) NEWLINE The System Only Dreams in Total Darkness by The National NEWLINE Chanel by Frank Ocean  NEWLINE Feel It Still by Portugal SENTENCEEND SENTENCESTART  The Man NEWLINE Butterfly Effect by Travis Scott NEWLINE Matter of Time by Sharon Jones & the Dap-Kings NEWLINE Little Bit by Mavis Staples NEWLINE Millionaire by Chris Stapleton NEWLINE Sign of the Times by Harry Styles  NEWLINE Broken Clocks by SZA NEWLINE Ordinary Love (Extraordinary Mix) by U2 NEWLINE *Bonus: Born in the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART A SENTENCEEND SENTENCESTART  by Bruce Springsteen (not out yet, but the blues version in his Broadway show is the best!) NEXTPOST SENTENCESTART On behalf of the Obama family, Merry Christmas! We wish you joy and peace this holiday season SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Wearing a Santa hat helps, too SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART I just got off a call to say thanks to folks who are working hard to help Americans around the country sign up for health care SENTENCEEND SENTENCESTART  But it\'s up to all of us to help to spread the word and tell people they can sign up through this Friday at HealthCare SENTENCEEND SENTENCESTART gov or 1-800-318-2596 SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Every plan that you can shop for right now includes free preventive care, like checkups, mammograms, and contraceptive care SENTENCEEND SENTENCESTART  There are no more annual or lifetime limits on the essential care you receive SENTENCEEND SENTENCESTART  And insurers can\'t discriminate against you if you\'ve got a preexisting condition SENTENCEEND SENTENCESTART  Plus, most people can find plans with monthly premiums under $75 SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Don\'t delay — head over to HealthCare SENTENCEEND SENTENCESTART gov and check out your options SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART I’m about to join a town hall with young people from all over India who are charting the course for a better future SENTENCEEND SENTENCESTART  I hope you’ll tune in SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART From the Obama family to yours, we wish you a Happy Thanksgiving full of joy and gratitude SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Yesterday, I dropped by a service project in Northeast D SENTENCEEND SENTENCESTART C SENTENCEEND SENTENCESTART , where a group of veterans were fixing up a public housing project SENTENCEEND SENTENCESTART  Just think about that for a moment SENTENCEEND SENTENCESTART  On a day dedicated to honoring their sacrifice, these veterans chose to honor their fellow citizens SENTENCEEND SENTENCESTART  They chose to roll up their sleeves and ask, “Now, what else can I do?”  NEWLINE   NEWLINE Today is a day to honor those who have honored our country with its highest form of service SENTENCEEND SENTENCESTART  We owe our veterans our thanks SENTENCEEND SENTENCESTART  Our respect SENTENCEEND SENTENCESTART  Our freedom SENTENCEEND SENTENCESTART  Today, we humbly acknowledge that we can never truly serve our veterans in quite the same way that they served us SENTENCEEND SENTENCESTART  But we can try SENTENCEEND SENTENCESTART  We can practice kindness SENTENCEEND SENTENCESTART  We can volunteer SENTENCEEND SENTENCESTART  We can serve SENTENCEEND SENTENCESTART  We can respect one another SENTENCEEND SENTENCESTART  We can have each other’s backs SENTENCEEND SENTENCESTART  We can ask, “Now, what else can I do?” NEXTPOST SENTENCESTART Starting today, you can sign up for 2018 health coverage SENTENCEEND SENTENCESTART  Head on over to HealthCare SENTENCEEND SENTENCESTART gov and find a plan that meets your needs and your budget SENTENCEEND SENTENCESTART  And spread the word so your friends and family can, too SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Tonight, the ex-Presidents are getting together in Texas to support all our fellow Americans rebuilding from this year’s hurricanes SENTENCEEND SENTENCESTART  Join us SENTENCEEND SENTENCESTART  Tune in and find out how you can help at OneAmericaAppeal SENTENCEEND SENTENCESTART org SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART I feel lucky to spend time with young leaders like these SENTENCEEND SENTENCESTART  Keep up the good work, São Paulo SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART When I left office, I told you all that the single most important thing I could do would be to help prepare the next generation of leaders to take their own crack at changing the world SENTENCEEND SENTENCESTART  The Obama Foundation Fellows program is looking to do just that  SEPARATOR  train and support civic innovators who are solving problems in their communities in creative and powerful ways SENTENCEEND SENTENCESTART  Apply to join our inaugural class of twenty Fellows by Friday, October 6th: www SENTENCEEND SENTENCESTART Obama SENTENCEEND SENTENCESTART org/Fellowship NEXTPOST SENTENCESTART I dropped in on Michelle’s talk at the Pennsylvania Conference for Women to deliver a message on our 25th wedding anniversary SENTENCEEND SENTENCESTART  Asking you to go out with me is the best decision I ever made SENTENCEEND SENTENCESTART  I love you, Michelle SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Michelle and I want the Obama Foundation to inspire and empower people to change the world SENTENCEEND SENTENCESTART  Here\'s how we\'re getting started this fall - I hope you\'ll be a part of it SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART America’s long journey towards equality has been guided by countless small acts of persistence, and fueled by the stubborn willingness of quiet heroes to speak out for what’s right SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE Few were as small in stature as Edie Windsor – and few made as big a difference to America SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE I had the privilege to speak with Edie a few days ago, and to tell her one more time what a difference she made to this country we love SENTENCEEND SENTENCESTART   She was engaged to her partner, Thea, for forty years SENTENCEEND SENTENCESTART   After a wedding in Canada, they were married for less than two SENTENCEEND SENTENCESTART   But federal law didn’t recognize a marriage like theirs as valid – which meant that they were denied certain federal rights and benefits that other married couples enjoyed SENTENCEEND SENTENCESTART   And when Thea passed away, Edie spoke up – not for special treatment, but for equal treatment – so that other legally married same-sex couples could enjoy the same federal rights and benefits as anyone else SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE In my second inaugural address, I said that if we are truly created equal, then surely the love we commit to one another must be equal as well SENTENCEEND SENTENCESTART   And because people like Edie stood up, my administration stopped defending the so-called Defense of Marriage Act in the courts SENTENCEEND SENTENCESTART   The day that the Supreme Court issued its 2013 ruling in United States v SENTENCEEND SENTENCESTART  Windsor was a great day for Edie, and a great day for America – a victory for human decency, equality, freedom, and justice SENTENCEEND SENTENCESTART   And I called Edie that day to congratulate her SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE Two years later, to the day, we took another step forward on our journey as the Supreme Court recognized a Constitutional guarantee of marriage equality SENTENCEEND SENTENCESTART   It was a victory for families, and for the principle that all of us should be treated equally, regardless of who we are or who we love SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE I thought about Edie that day SENTENCEEND SENTENCESTART   I thought about all the millions of quiet heroes across the decades whose countless small acts of courage slowly made an entire country realize that love is love – and who, in the process, made us all more free SENTENCEEND SENTENCESTART   They deserve our gratitude SENTENCEEND SENTENCESTART   And so does Edie SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE Michelle and I offer our condolences to her wife, Judith, and to all who loved and looked up to Edie Windsor SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Americans always answer the call SENTENCEEND SENTENCESTART  OneAmericaAppeal SENTENCEEND SENTENCESTART org NEXTPOST SENTENCESTART Immigration can be a controversial topic SENTENCEEND SENTENCESTART  We all want safe, secure borders and a dynamic economy, and people of goodwill can have legitimate disagreements about how to fix our immigration system so that everybody plays by the rules SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE But that’s not what the action that the White House took today is about SENTENCEEND SENTENCESTART  This is about young people who grew up in America – kids who study in our schools, young adults who are starting careers, patriots who pledge allegiance to our flag SENTENCEEND SENTENCESTART  These Dreamers are Americans in their hearts, in their minds, in every single way but one: on paper SENTENCEEND SENTENCESTART  They were brought to this country by their parents, sometimes even as infants SENTENCEEND SENTENCESTART  They may not know a country besides ours SENTENCEEND SENTENCESTART  They may not even know a language besides English SENTENCEEND SENTENCESTART  They often have no idea they’re undocumented until they apply for a job, or college, or a driver’s license SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Over the years, politicians of both parties have worked together to write legislation that would have told these young people – our young people – that if your parents brought you here as a child, if you’ve been here a certain number of years, and if you’re willing to go to college or serve in our military, then you’ll get a chance to stay and earn your citizenship SENTENCEEND SENTENCESTART  And for years while I was President, I asked Congress to send me such a bill SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE That bill never came SENTENCEEND SENTENCESTART  And because it made no sense to expel talented, driven, patriotic young people from the only country they know solely because of the actions of their parents, my administration acted to lift the shadow of deportation from these young people, so that they could continue to contribute to our communities and our country SENTENCEEND SENTENCESTART  We did so based on the well-established legal principle of prosecutorial discretion, deployed by Democratic and Republican presidents alike, because our immigration enforcement agencies have limited resources, and it makes sense to focus those resources on those who come illegally to this country to do us harm SENTENCEEND SENTENCESTART  Deportations of criminals went up SENTENCEEND SENTENCESTART  Some 800,000 young people stepped forward, met rigorous requirements, and went through background checks SENTENCEEND SENTENCESTART  And America grew stronger as a result SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE But today, that shadow has been cast over some of our best and brightest young people once again SENTENCEEND SENTENCESTART  To target these young people is wrong – because they have done nothing wrong SENTENCEEND SENTENCESTART  It is self-defeating – because they want to start new businesses, staff our labs, serve in our military, and otherwise contribute to the country we love SENTENCEEND SENTENCESTART  And it is cruel SENTENCEEND SENTENCESTART  What if our kid’s science teacher, or our friendly neighbor turns out to be a Dreamer? Where are we supposed to send her? To a country she doesn’t know or remember, with a language she may not even speak? NEWLINE  NEWLINE Let’s be clear: the action taken today isn’t required legally SENTENCEEND SENTENCESTART  It’s a political decision, and a moral question SENTENCEEND SENTENCESTART  Whatever concerns or complaints Americans may have about immigration in general, we shouldn’t threaten the future of this group of young people who are here through no fault of their own, who pose no threat, who are not taking away anything from the rest of us SENTENCEEND SENTENCESTART  They are that pitcher on our kid’s softball team, that first responder who helps out his community after a disaster, that cadet in ROTC who wants nothing more than to wear the uniform of the country that gave him a chance SENTENCEEND SENTENCESTART  Kicking them out won’t lower the unemployment rate, or lighten anyone’s taxes, or raise anybody’s wages SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE It is precisely because this action is contrary to our spirit, and to common sense, that business leaders, faith leaders, economists, and Americans of all political stripes called on the administration not to do what it did today SENTENCEEND SENTENCESTART  And now that the White House has shifted its responsibility for these young people to Congress, it’s up to Members of Congress to protect these young people and our future SENTENCEEND SENTENCESTART  I’m heartened by those who’ve suggested that they should SENTENCEEND SENTENCESTART  And I join my voice with the majority of Americans who hope they step up and do it with a sense of moral urgency that matches the urgency these young people feel SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Ultimately, this is about basic decency SENTENCEEND SENTENCESTART  This is about whether we are a people who kick hopeful young strivers out of America, or whether we treat them the way we’d want our own kids to be treated SENTENCEEND SENTENCESTART  It’s about who we are as a people – and who we want to be SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE What makes us American is not a question of what we look like, or where our names come from, or the way we pray SENTENCEEND SENTENCESTART  What makes us American is our fidelity to a set of ideals – that all of us are created equal; that all of us deserve the chance to make of our lives what we will; that all of us share an obligation to stand up, speak out, and secure our most cherished values for the next generation SENTENCEEND SENTENCESTART  That’s how America has traveled this far SENTENCEEND SENTENCESTART  That’s how, if we keep at it, we will ultimately reach that more perfect union SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART More ways to help Houston: NEXTPOST SENTENCESTART Our politics are divided SENTENCEEND SENTENCESTART   They have been for a long time SENTENCEEND SENTENCESTART   And while I know that division makes it difficult to listen to Americans with whom we disagree, that’s what we need to do today SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE I recognize that repealing and replacing the Affordable Care Act has become a core tenet of the Republican Party SENTENCEEND SENTENCESTART   Still, I hope that our Senators, many of whom I know well, step back and measure what’s really at stake, and consider that the rationale for action, on health care or any other issue, must be something more than simply undoing something that Democrats did SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE We didn’t fight for the Affordable Care Act for more than a year in the public square for any personal or political gain – we fought for it because we knew it would save lives, prevent financial misery, and ultimately set this country we love on a better, healthier course SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE Nor did we fight for it alone SENTENCEEND SENTENCESTART   Thousands upon thousands of Americans, including Republicans, threw themselves into that collective effort, not for political reasons, but for intensely personal ones – a sick child, a parent lost to cancer, the memory of medical bills that threatened to derail their dreams SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE And you made a difference SENTENCEEND SENTENCESTART   For the first time, more than ninety percent of Americans know the security of health insurance SENTENCEEND SENTENCESTART   Health care costs, while still rising, have been rising at the slowest pace in fifty years SENTENCEEND SENTENCESTART   Women can’t be charged more for their insurance, young adults can stay on their parents’ plan until they turn 26, contraceptive care and preventive care are now free SENTENCEEND SENTENCESTART   Paying more, or being denied insurance altogether due to a preexisting condition – we made that a thing of the past SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE We did these things together SENTENCEEND SENTENCESTART   So many of you made that change possible SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE At the same time, I was careful to say again and again that while the Affordable Care Act represented a significant step forward for America, it was not perfect, nor could it be the end of our efforts – and that if Republicans could put together a plan that is demonstrably better than the improvements we made to our health care system, that covers as many people at less cost, I would gladly and publicly support it SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE That remains true SENTENCEEND SENTENCESTART   So I still hope that there are enough Republicans in Congress who remember that public service is not about sport or notching a political win, that there’s a reason we all chose to serve in the first place, and that hopefully, it’s to make people’s lives better, not worse SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE But right now, after eight years, the legislation rushed through the House and the Senate without public hearings or debate would do the opposite SENTENCEEND SENTENCESTART   It would raise costs, reduce coverage, roll back protections, and ruin Medicaid as we know it SENTENCEEND SENTENCESTART   That’s not my opinion, but rather the conclusion of all objective analyses, from the nonpartisan Congressional Budget Office, which found that 23 million Americans would lose insurance, to America’s doctors, nurses, and hospitals on the front lines of our health care system SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE The Senate bill, unveiled today, is not a health care bill SENTENCEEND SENTENCESTART   It’s a massive transfer of wealth from middle-class and poor families to the richest people in America SENTENCEEND SENTENCESTART   It hands enormous tax cuts to the rich and to the drug and insurance industries, paid for by cutting health care for everybody else SENTENCEEND SENTENCESTART   Those with private insurance will experience higher premiums and higher deductibles, with lower tax credits to help working families cover the costs, even as their plans might no longer cover pregnancy, mental health care, or expensive prescriptions SENTENCEEND SENTENCESTART   Discrimination based on pre-existing conditions could become the norm again SENTENCEEND SENTENCESTART   Millions of families will lose coverage entirely SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE Simply put, if there’s a chance you might get sick, get old, or start a family – this bill will do you harm SENTENCEEND SENTENCESTART   And small tweaks over the course of the next couple weeks, under the guise of making these bills easier to stomach, cannot change the fundamental meanness at the core of this legislation SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE I hope our Senators ask themselves – what will happen to the Americans grappling with opioid addiction who suddenly lose their coverage?  What will happen to pregnant mothers, children with disabilities, poor adults and seniors who need long-term care once they can no longer count on Medicaid?  What will happen if you have a medical emergency when insurance companies are once again allowed to exclude the benefits you need, send you unlimited bills, or set unaffordable deductibles?  What impossible choices will working parents be forced to make if their child’s cancer treatment costs them more than their life savings? NEWLINE   NEWLINE To put the American people through that pain – while giving billionaires and corporations a massive tax cut in return – that’s tough to fathom SENTENCEEND SENTENCESTART   But it’s what’s at stake right now SENTENCEEND SENTENCESTART   So it remains my fervent hope that we step back and try to deliver on what the American people need SENTENCEEND SENTENCESTART   NEWLINE   NEWLINE That might take some time and compromise between Democrats and Republicans SENTENCEEND SENTENCESTART   But I believe that’s what people want to see SENTENCEEND SENTENCESTART   I believe it would demonstrate the kind of leadership that appeals to Americans across party lines SENTENCEEND SENTENCESTART   And I believe that it’s possible – if you are willing to make a difference again SENTENCEEND SENTENCESTART   If you’re willing to call your members of Congress SENTENCEEND SENTENCESTART   If you are willing to visit their offices SENTENCEEND SENTENCESTART   If you are willing to speak out, let them and the country know, in very real terms, what this means for you and your family SENTENCEEND SENTENCESTART  NEWLINE   NEWLINE After all, this debate has always been about something bigger than politics SENTENCEEND SENTENCESTART   It’s about the character of our country – who we are, and who we aspire to be SENTENCEEND SENTENCESTART   And that’s always worth fighting for SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Tonight at 9 p SENTENCEEND SENTENCESTART m SENTENCEEND SENTENCESTART  Eastern Time, President Obama delivers his farewell address in Chicago SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Don\'t miss it SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Our goal wasn\'t just to make sure more people have coverage—it was to make sure more people have better coverage SENTENCEEND SENTENCESTART " —President Obama NEXTPOST SENTENCESTART A majority of Americans say they are ready for immigration reform: http://ofa SENTENCEEND SENTENCESTART bo/p9HH NEXTPOST SENTENCESTART Under Obamacare, 16 million people gained health insurance and it\'s not the “job killer” opponents said it would be SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/c4z5 NEXTPOST SENTENCESTART "Climate change once seemed like a problem for future generations, but for most Americans, it\'s already a reality SENTENCEEND SENTENCESTART " —President Obama NEWLINE  NEWLINE See why the President is heading to Alaska SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/a53M NEXTPOST SENTENCESTART The uninsured rate is down to single digits for the first time ever SENTENCEEND SENTENCESTART  That\'s a reason to celebrate SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/f9Cx NEXTPOST SENTENCESTART Women and working families need updated overtime rules SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART President Obama responded to an article on efforts to dismantle protections in the Voting Rights Act SENTENCEEND SENTENCESTART  Check it out: http://ofa SENTENCEEND SENTENCESTART bo/p9Gd NEXTPOST SENTENCESTART Read why expanding overtime protections would make a difference for millions of American workers—especially women SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/r9CT NEXTPOST SENTENCESTART Wind energy is now providing 73,000 jobs—and counting SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/q96a NEXTPOST SENTENCESTART States that embraced Obamacare saw a much bigger drop in uninsured rates this year: http://ofa SENTENCEEND SENTENCESTART bo/g99w NEXTPOST SENTENCESTART President Obama discussed the implications of the historic Iran deal in an exclusive interview with Mic SENTENCEEND SENTENCESTART  Check it out SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/i9BI NEXTPOST SENTENCESTART Cleaner air means healthier people SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "The right to vote is one of the most fundamental rights of any democracy SENTENCEEND SENTENCESTART  Yet for too long, too many of our fellow citizens were denied that right, simply because of the color of their skin SENTENCEEND SENTENCESTART " —President Obama SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Watch the weekly address SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Yesterday was the 50th anniversary of the Voting Rights Act SENTENCEEND SENTENCESTART  Read President Obama\'s thoughts on its legacy—and the work still ahead SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/i9Ao NEXTPOST SENTENCESTART July marks 65 consecutive months of private-sector job growth—the longest streak on record SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/c4xL NEXTPOST SENTENCESTART Read President Obama\'s message on how you can support the Clean Power Plan: http://ofa SENTENCEEND SENTENCESTART bo/h9HC NEXTPOST SENTENCESTART Add your name if you support the Clean Power Plan—the biggest step America has ever taken in the fight against climate change SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/s99U NEXTPOST SENTENCESTART OFA’s card is waiting for your signature SENTENCEEND SENTENCESTART  Help wish the President a happy birthday: http://ofa SENTENCEEND SENTENCESTART bo/f9An NEXTPOST SENTENCESTART The Clean Power Plan will create tens of thousands of jobs, lower the cost of renewable energy, and reduce carbon pollution SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Read more from White House Senior Advisor Brian Deese SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/e4vJ NEXTPOST SENTENCESTART Birthday hat? ✓ NEWLINE  NEWLINE Birthday card? NEWLINE  NEWLINE Sign now: http://ofa SENTENCEEND SENTENCESTART bo/d4zG NEXTPOST SENTENCESTART Support the Clean Power Plan—be a part of the biggest action our country has taken to tackle climate change: http://ofa SENTENCEEND SENTENCESTART bo/j95i NEXTPOST SENTENCESTART The Clean Power Plan is good news for our economy and for public health SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Haven\'t signed President Obama’s birthday card yet? Here’s your chance: http://ofa SENTENCEEND SENTENCESTART bo/p9E0 NEXTPOST SENTENCESTART "If one of the best measures of a country is how it treats its more vulnerable citizens—seniors, the poor, the sick—then America has a lot to be proud of SENTENCEEND SENTENCESTART " —President Obama on the 50th anniversary of Medicare and Medicaid NEWLINE  NEWLINE Watch the weekly address SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Looking at you—have you signed President Obama’s birthday card yet? http://ofa SENTENCEEND SENTENCESTART bo/p9Dl NEXTPOST SENTENCESTART DYK: 50% of uninsured single young adults are eligible for coverage that costs only $50 SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Check out your options today: http://ofa SENTENCEEND SENTENCESTART bo/bKF  NEXTPOST SENTENCESTART BUDGET DEADLINE AT MIDNIGHT: Help with the final push for health care SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/qJi   NEWLINE  NEXTPOST SENTENCESTART You can help with the final push for the Affordable Care Act: http://ofa SENTENCEEND SENTENCESTART bo/eIc  NEWLINE  NEXTPOST SENTENCESTART Watch: Keegan built his own business from the ground up—and the Affordable Care Act helped him do it SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/iIf NEXTPOST SENTENCESTART "We know from our history that our economy grows best from the middle out, when growth is more widely shared SENTENCEEND SENTENCESTART  So we’ve got to restore opportunity for all—the idea that with hard work and responsibility, you can get ahead SENTENCEEND SENTENCESTART "—President Obama NEWLINE  NEWLINE Watch the weekly address: http://ofa SENTENCEEND SENTENCESTART bo/qIc NEXTPOST SENTENCESTART Options for affordable care on the new marketplace are mind-blowing SENTENCEEND SENTENCESTART  See for yourself: http://ofa SENTENCEEND SENTENCESTART bo/aHs NEXTPOST SENTENCESTART Share this story: John was skeptical about the new health care marketplace—until he saw his options SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/cGx NEXTPOST SENTENCESTART What are your 3 SENTENCEEND SENTENCESTART 141592 favorite flavors? Happy Pi Day SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Take a look at your options SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/bHx NEXTPOST SENTENCESTART #ThrowbackThursday: Family edition SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Chip in and be a part of the final health care push: http://ofa SENTENCEEND SENTENCESTART bo/cF0 NEXTPOST SENTENCESTART This calculator will tell you how little your health insurance could cost on the new marketplace: http://ofa SENTENCEEND SENTENCESTART bo/aFG NEXTPOST SENTENCESTART You never know what you could save: http://ofa SENTENCEEND SENTENCESTART bo/eEg NEXTPOST SENTENCESTART Watch: President Obama is interviewed between two ferns SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/jEs NEXTPOST SENTENCESTART Up top SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART There are only a few weeks left to get health coverage that starts in 2014 SENTENCEEND SENTENCESTART  Don\'t wait: http://ofa SENTENCEEND SENTENCESTART bo/jDq NEXTPOST SENTENCESTART Share Joshua\'s health care story: He\'s a healthy, young grad student with a limited budget SENTENCEEND SENTENCESTART  Find out how little he\'s paying for quality health insurance SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/bEM NEXTPOST SENTENCESTART "It’s time to give America a raise SENTENCEEND SENTENCESTART  And it’s time to restore opportunity for all SENTENCEEND SENTENCESTART " —President Obama NEWLINE  NEWLINE Watch the weekly address: http://ofa SENTENCEEND SENTENCESTART bo/sDK NEXTPOST SENTENCESTART Watch President Obama talk health care with a few people you may recognize from YouTube—including some who do a pretty good President Obama SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/dCP NEXTPOST SENTENCESTART Don\'t miss your chance to get covered in 2014 SENTENCEEND SENTENCESTART  Get enrolled now SENTENCEEND SENTENCESTART  http://ofa SENTENCEEND SENTENCESTART bo/jD8 NEXTPOST SENTENCESTART Share Margaret\'s life-changing story: http://ofa SENTENCEEND SENTENCESTART bo/gBo NEXTPOST SENTENCESTART You might qualify for financial assistance on the new health care marketplace SENTENCEEND SENTENCESTART  Check out your options: http://ofa SENTENCEEND SENTENCESTART bo/aCY NEXTPOST SENTENCESTART President Obama is fighting to protect women’s health—say you’ll stand with him: http://OFA SENTENCEEND SENTENCESTART BO/DqH8an NEXTPOST SENTENCESTART A hug in Davenport, Iowa yesterday SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Share this image if you agree that we need to invest in clean energy that’s made right here in America SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Download the new (and free!) Obama smartphone app to get campaign news, updates on events near you, and voting info—right on your iPhone or Android: http://OFA SENTENCEEND SENTENCESTART BO/ZRQfoD NEXTPOST SENTENCESTART Stan Musial NEXTPOST SENTENCESTART Bill Russell NEXTPOST SENTENCESTART Pat Summitt NEXTPOST SENTENCESTART Billie Jean King NEXTPOST SENTENCESTART Share this image so your friends and family know the choice we face on Medicare this fall SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART LAST CALL: Dinner with Michael Jordan and the President SENTENCEEND SENTENCESTART  Pitch in $5 or more before midnight and you’ll be automatically entered to win: http://OFA SENTENCEEND SENTENCESTART BO/3RSDgi NEXTPOST SENTENCESTART Show a little muscle this election season—this tank top is 25% off with the code VOTEOBAMA: http://OFA SENTENCEEND SENTENCESTART BO/zjfSXn NEXTPOST SENTENCESTART “I don’t think your boss should get to control the health care that you get SENTENCEEND SENTENCESTART  I don’t think insurance companies should control the care that you get SENTENCEEND SENTENCESTART  I don’t think politicians should control the care that you get SENTENCEEND SENTENCESTART  I think there\'s one person to make these decisions on health care, and that is you SENTENCEEND SENTENCESTART ”—President Obama on women’s health NEXTPOST SENTENCESTART Team U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART A SENTENCEEND SENTENCESTART  is leading the Track & Field medal count—and there\'s more to be proud of at home SENTENCEEND SENTENCESTART  Share if you\'re proud of our athletes at home and abroad SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART President Obama, Patrick Ewing, and you? Pitch in $5 and you and a guest will be automatically entered to win dinner and a shootaround with some of basketball’s greatest stars: http://OFA SENTENCEEND SENTENCESTART BO/NzV7Fi NEXTPOST SENTENCESTART This morning, we learned that for the third month in a row, we\'ve been outraised by the other side in this race SENTENCEEND SENTENCESTART  Help close the gap—pitch in $5 now, and ask a friend to do the same: http://OFA SENTENCEEND SENTENCESTART BO/UUR46p NEXTPOST SENTENCESTART Share this if you\'re particularly proud of the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART A SENTENCEEND SENTENCESTART  gymnastics program this week SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Made in the USA, 51 years ago today: http://OFA SENTENCEEND SENTENCESTART BO/uNYvDu NEXTPOST SENTENCESTART It\'s Barack\'s birthday today—wish him a happy one by signing his card! http://OFA SENTENCEEND SENTENCESTART BO/HHfZev NEXTPOST SENTENCESTART Good news: As of July, the economy has added private-sector jobs for 29 straight months—for a total of 4 SENTENCEEND SENTENCESTART 5 million jobs during that period SENTENCEEND SENTENCESTART  But we\'ve got more work to do SENTENCEEND SENTENCESTART  Stand with President Obama to keep up this progress: http://OFA SENTENCEEND SENTENCESTART BO/uaTtqr NEXTPOST SENTENCESTART "You guys amaze us the most SENTENCEEND SENTENCESTART " —President Obama, calling the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  women\'s gymnastics team from Air Force One yesterday to congratulate them on their big win NEXTPOST SENTENCESTART The U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART A SENTENCEEND SENTENCESTART  swim program has a lot of reasons to be proud this week—here are a few more SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Life doesn\'t count for much unless you\'re willing to do your small part to leave our children—all of our children—a better world SENTENCEEND SENTENCESTART " —President Obama SENTENCEEND SENTENCESTART  Pitch in $5 or more before tomorrow\'s fundraising deadline to help make sure we keep moving forward—not back: http://OFA SENTENCEEND SENTENCESTART BO/RiAbEo NEXTPOST SENTENCESTART Obama for America - New Hampshire volunteers increased their ranks one person at a time on Saturday and Sunday—that’s what It Takes One is all about: http://OFA SENTENCEEND SENTENCESTART BO/Mooytx NEXTPOST SENTENCESTART Don Cheadle talks to a barber shop owner in Durham while volunteering with Obama for America - North Carolina SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Judy hadn’t canvassed in 40 years, but that didn’t stop her from being part of Obama for America - Ohio’s weekend of action SENTENCEEND SENTENCESTART  Sign up to volunteer in your own community: http://OFA SENTENCEEND SENTENCESTART BO/p9MNKi NEXTPOST SENTENCESTART President Barack Obama makes phone calls from the Oval Office to members of the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  military on Thanksgiving Day, Nov SENTENCEEND SENTENCESTART  25, 2010 SENTENCEEND SENTENCESTART  \r NEWLINE \r NEWLINE (Official White House Photo by Pete Souza) NEXTPOST SENTENCESTART The American people did not vote for gridlock SENTENCEEND SENTENCESTART  They didn’t vote for unyielding partisanship SENTENCEEND SENTENCESTART  They’re demanding cooperation, and they’re demanding progress SENTENCEEND SENTENCESTART  And they’ll hold all of us accountable for it SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Thanksgiving is a holiday that asks us to be thankful for what we have and generous to those who have less SENTENCEEND SENTENCESTART  It’s a time to spend with the ones we love and a chance to show compassion and concern to people we’ve never met SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART President Barack Obama makes phone calls from the Oval Office to members of the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  military on Thanksgiving Day SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Don\'t bet against the American auto industry SENTENCEEND SENTENCESTART  Don\'t bet against American ingenuity and the American worker SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Today, General Motors relaunched itself as a public company, and American taxpayers are now positioned to recover more than my administration invested in GM SENTENCEEND SENTENCESTART  We are finally beginning to see some of the tough decisions that we made in the midst of crisis pay off SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART This administration is working to ensure that patients can receive compassionate care and equal treatment during their hospital stays SENTENCEEND SENTENCESTART  New rules will allow patients to decide their visitors—including same-sex domestic partners SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART President Barack Obama wishes Vice President Joe Biden an early happy birthday after he was presented with a cake during their lunch in the Private Dining Room SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART President Barack Obama offers a toast during the State Dinner at the Istana Negara State Palace Complex in Jakarta, Indonesia, Nov SENTENCEEND SENTENCESTART  9, 2010 SENTENCEEND SENTENCESTART  \r NEWLINE \r NEWLINE (Official White House Photo by Pete Souza) NEXTPOST SENTENCESTART President Barack Obama and Prime Minister Manmohan Singh talk during the State Dinner at Rashtrapati Bhavan, the presidential palace, in New Dehli, India, Nov SENTENCEEND SENTENCESTART  8, 2010 SENTENCEEND SENTENCESTART  \r NEWLINE \r NEWLINE (Official White House Photo by Pete Souza) NEXTPOST SENTENCESTART President Barack Obama and First lady Michelle Obama greet children while touring Humayun\'s tomb in New Delhi, November 7, 2010 SENTENCEEND SENTENCESTART \r NEWLINE \r NEWLINE (Official White House Photo by Chuck Kennedy) NEXTPOST SENTENCESTART President Barack Obama and First Lady Michelle Obama pause for a moment of silence at Rajghat, a memorial to Mahatma Gandhi, in New Delhi, India, Nov SENTENCEEND SENTENCESTART  8, 2010 SENTENCEEND SENTENCESTART  \r NEWLINE \r NEWLINE (Official White House Photo by Pete Souza) NEXTPOST SENTENCESTART President Barack Obama talks with Australian Prime Minister Julia Gillard during the APEC Leader’s Closing Retreat at the Intercontinental Hotel in Yokohama, Japan, Nov SENTENCEEND SENTENCESTART  14, 2010 SENTENCEEND SENTENCESTART  \r NEWLINE \r NEWLINE (Official White House Photo by Pete Souza) NEXTPOST SENTENCESTART President Barack Obama’s motorcade is met by bodyguards on horseback at the main entrance of Rashtrapati Bhavan, the presidential palace, during the official arrival ceremony in New Delhi, India, Nov SENTENCEEND SENTENCESTART  8, 2010 SENTENCEEND SENTENCESTART  \r NEWLINE \r NEWLINE (Official White House Photo by Pete Souza) NEXTPOST SENTENCESTART President Barack Obama and First Lady Michelle Obama stand in a receiving line and greet guests at the State Dinner at Rashtrapati Bhavan, the presidential palace, in New Dehli, India, Nov SENTENCEEND SENTENCESTART  8, 2010 SENTENCEEND SENTENCESTART  \r NEWLINE \r NEWLINE (Official White House Photo by Pete Souza) NEXTPOST SENTENCESTART President Barack Obama visits the Great Buddha of Kamakura with Michiko Sato, director, and Dr SENTENCEEND SENTENCESTART  Takao Sato, Chief Monk, at the Kotoku-In Temple in Kamakura, Japan, Nov SENTENCEEND SENTENCESTART  14, 2010 SENTENCEEND SENTENCESTART  \r NEWLINE \r NEWLINE (Official White House Photo by Pete Souza) NEXTPOST SENTENCESTART President Barack Obama holds a bilateral meeting with President Hu Jintao of China at the Grand Hyatt Hotel in Seoul, South Korea, Nov SENTENCEEND SENTENCESTART  11, 2010 SENTENCEEND SENTENCESTART  \r NEWLINE \r NEWLINE (Official White House Photo by Samantha Appleton)',
     'NEXTPOST SENTENCESTART “I feel like God has given me a talent for art, and using that talent is a form of gratitude SENTENCEEND SENTENCESTART   For years I worked in an automotive workshop SENTENCEEND SENTENCESTART   I’d try to paint when I came home, but I never had any energy left SENTENCEEND SENTENCESTART   So I finally quit SENTENCEEND SENTENCESTART   Now I have more freedom to develop my talents, but it hasn’t been easy SENTENCEEND SENTENCESTART   Art is more difficult in this country SENTENCEEND SENTENCESTART   It’s not really seen as a profession SENTENCEEND SENTENCESTART   It would be easier if I was alone, but I have a family to support so there’s a lot of pressure to earn SENTENCEEND SENTENCESTART   I’m trying to be tough in poverty SENTENCEEND SENTENCESTART   I read a lot of poetry to give me strength SENTENCEEND SENTENCESTART   I would never beg, so right now I’m drawing portraits and caricatures to support myself SENTENCEEND SENTENCESTART   I’d rather be painting what I think and feel, but I have to fulfill what the consumer wants SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE (Jakarta, Indonesia) NEXTPOST SENTENCESTART “Mom raised me on her own since I was young SENTENCEEND SENTENCESTART   My dad didn’t care about our family SENTENCEEND SENTENCESTART   He was there physically, but he wasn’t there SENTENCEEND SENTENCESTART   He kept all his income for himself SENTENCEEND SENTENCESTART   He gambled away our possessions SENTENCEEND SENTENCESTART   We couldn’t buy anything for ourselves because he would sell it SENTENCEEND SENTENCESTART   He sold all our electronics SENTENCEEND SENTENCESTART   He even sold our furniture SENTENCEEND SENTENCESTART   But my mother still did everything for him: cooked his food, did his laundry… everything SENTENCEEND SENTENCESTART   Then one day she came home from work and the house was empty SENTENCEEND SENTENCESTART   There was nothing left but our clothes SENTENCEEND SENTENCESTART   I told her it was time to go SENTENCEEND SENTENCESTART   For the last two years we’ve been living in a rented house together SENTENCEEND SENTENCESTART   Mom seems so much lighter now SENTENCEEND SENTENCESTART   We can own our own things SENTENCEEND SENTENCESTART   We can actually make progress and move forward in life SENTENCEEND SENTENCESTART   I\'ve started working at a pharmacy and I\'m paying my way through college SENTENCEEND SENTENCESTART   I’m still in touch with my father SENTENCEEND SENTENCESTART   I even give him money for rent and food SENTENCEEND SENTENCESTART   I have no interest in taking revenge SENTENCEEND SENTENCESTART   I’m showing him how he should have treated us SENTENCEEND SENTENCESTART ”       NEWLINE  NEWLINE (Jakarta, Indonesia) NEXTPOST SENTENCESTART “I’m a fund manager SENTENCEEND SENTENCESTART   One of my employees stole millions from my clients SENTENCEEND SENTENCESTART   He hid it from me for almost two years SENTENCEEND SENTENCESTART   He was working with a broker to create fake transactions SENTENCEEND SENTENCESTART   He disappeared when I discovered the scheme SENTENCEEND SENTENCESTART   I could call the police but it would be all over the press, and I’d never get another client SENTENCEEND SENTENCESTART   I could declare bankruptcy but I’m not a coward SENTENCEEND SENTENCESTART   So I’m trying to pay my clients back the initial amount they invested SENTENCEEND SENTENCESTART   I sold my other business SENTENCEEND SENTENCESTART   I sold two houses and a car SENTENCEEND SENTENCESTART   That’s why I’m taking trains and online taxis SENTENCEEND SENTENCESTART   Some of my clients feel sorry for me SENTENCEEND SENTENCESTART   Most of them are angry SENTENCEEND SENTENCESTART   A few hate me SENTENCEEND SENTENCESTART   All of them keep calling SENTENCEEND SENTENCESTART   I tell them to give me time, but they keep calling SENTENCEEND SENTENCESTART   I’m smoking four packs of cigarettes a day because of the stress SENTENCEEND SENTENCESTART   I’ve got to find fresh money SENTENCEEND SENTENCESTART   If I can get new clients, then I can slowly repay the old ones with the profits that I make SENTENCEEND SENTENCESTART   But all of them want their money now SENTENCEEND SENTENCESTART ”   NEWLINE  NEWLINE (Jakarta, Indonesia) NEXTPOST SENTENCESTART “Several years ago my father used our house for collateral on a business loan SENTENCEEND SENTENCESTART   When things fell apart, we lost everything SENTENCEEND SENTENCESTART   My dad got depressed and fell sick SENTENCEEND SENTENCESTART   My mother was backed into a corner SENTENCEEND SENTENCESTART   She had four young children but no income SENTENCEEND SENTENCESTART   At the time she was a very feminine housewife SENTENCEEND SENTENCESTART   But she asked our neighbors to teach her how to farm SENTENCEEND SENTENCESTART   Then she convinced our landlord to let her plant an empty field SENTENCEEND SENTENCESTART   He told her, ‘If you can make it yield anything, we’ll split the profits SENTENCEEND SENTENCESTART ’  It didn’t succeed right away SENTENCEEND SENTENCESTART   There were a lot of pests SENTENCEEND SENTENCESTART   Some of the crops dried out SENTENCEEND SENTENCESTART   But my mother harvested whatever was left, and it was enough to support our family SENTENCEEND SENTENCESTART   She cut off her hair during this time SENTENCEEND SENTENCESTART   She got very muscular SENTENCEEND SENTENCESTART   She worked all day in the fields but still found time to make sure we went to school SENTENCEEND SENTENCESTART   If my brothers and I ever started crying, she’d tell us: ‘If I’m strong enough to do this, you’re strong enough to go to school SENTENCEEND SENTENCESTART ’”     NEWLINE  NEWLINE (Jakarta, Indonesia) NEXTPOST SENTENCESTART “I tried to pick my wife up from her religious group one night, but her friends told me that she’d left with someone else SENTENCEEND SENTENCESTART   She told me the guy was just a friend SENTENCEEND SENTENCESTART   But over the next few months, I started to hear more and more things SENTENCEEND SENTENCESTART   Somebody saw her coming out of a hotel with another man SENTENCEEND SENTENCESTART   Then a few weeks later I discovered them on a beach together SENTENCEEND SENTENCESTART   Then finally I came home early one day and found them in my house SENTENCEEND SENTENCESTART   I was heartbroken SENTENCEEND SENTENCESTART   Because I really did love her SENTENCEEND SENTENCESTART   I left town immediately after the divorce because I didn’t want to be reminded of the heartbreak SENTENCEEND SENTENCESTART   I’ve lived in Jakarta for three years now SENTENCEEND SENTENCESTART   I have my own shop SENTENCEEND SENTENCESTART   I’ve saved a lot of money SENTENCEEND SENTENCESTART   I’m doing better than I ever have before SENTENCEEND SENTENCESTART   My plan was to become successful and win my wife back, but my daughter tells me that she’s already remarried SENTENCEEND SENTENCESTART   So I’m hoping to meet someone else SENTENCEEND SENTENCESTART   One of my customers was left by her husband SENTENCEEND SENTENCESTART   She also has older children SENTENCEEND SENTENCESTART   We’ve been talking on the phone a lot lately SENTENCEEND SENTENCESTART ”       NEWLINE  NEWLINE (Jakarta, Indonesia) NEXTPOST SENTENCESTART “We were planning to swim today but then we decided to go looking for foreigners SENTENCEEND SENTENCESTART   Rayhan had never seen one before SENTENCEEND SENTENCESTART   Foreigners are fun and handsome SENTENCEEND SENTENCESTART   If you get a picture with one of them, you’ll get a lot of likes on Facebook SENTENCEEND SENTENCESTART   We found a few already SENTENCEEND SENTENCESTART   There was one family from France that was really nice and let us a take a picture SENTENCEEND SENTENCESTART   Then we found one other girl but she was kind of snobby SENTENCEEND SENTENCESTART   She said some words in English and walked away quickly SENTENCEEND SENTENCESTART   Maybe she was feeling sick SENTENCEEND SENTENCESTART ”             NEWLINE  NEWLINE (Jakarta, Indonesia) NEXTPOST SENTENCESTART “It was spontaneous SENTENCEEND SENTENCESTART   I didn’t ask for permission SENTENCEEND SENTENCESTART   We’d been dating for five months, so I just decided to go for it SENTENCEEND SENTENCESTART   We were standing on a beach SENTENCEEND SENTENCESTART   Watching the sunset SENTENCEEND SENTENCESTART   I waited for the perfect moment when she wasn’t paying attention SENTENCEEND SENTENCESTART   Then I leaned in and kissed her on the cheek SENTENCEEND SENTENCESTART   At first she was silent SENTENCEEND SENTENCESTART   She gave me a bit of a side look SENTENCEEND SENTENCESTART   I thought I’d messed up SENTENCEEND SENTENCESTART   So I pointed at the waves and pretended that I saw something in the ocean SENTENCEEND SENTENCESTART   I let things cool down for a few months before trying again SENTENCEEND SENTENCESTART ”   NEWLINE  NEWLINE (Jakarta, Indonesia) NEXTPOST SENTENCESTART “I tried to start a business right out of high school SENTENCEEND SENTENCESTART   I didn’t have much experience SENTENCEEND SENTENCESTART   But a mutual friend showed me a business model and I agreed to partner with him SENTENCEEND SENTENCESTART   We supplied wholesale groceries like onions, barley, and garlic SENTENCEEND SENTENCESTART  My partner did the purchasing and kept all the books SENTENCEEND SENTENCESTART   I just supplied the money to purchase more stock SENTENCEEND SENTENCESTART   I was completely dependent on the information he gave me SENTENCEEND SENTENCESTART   And at first we showed a lot of profit SENTENCEEND SENTENCESTART   Every time we filled an order, there would be an even bigger order SENTENCEEND SENTENCESTART   So I just kept putting more money into the business SENTENCEEND SENTENCESTART   That went on for four years SENTENCEEND SENTENCESTART   But when I finally tried to take some profits, my partner claimed there was no money SENTENCEEND SENTENCESTART   He showed me an entirely new set of books SENTENCEEND SENTENCESTART   He’d stolen everything, and I couldn’t prove a thing because I hadn’t been keeping my own records SENTENCEEND SENTENCESTART   I hid it from my wife for days SENTENCEEND SENTENCESTART   We had two kids and I’d just lost everything SENTENCEEND SENTENCESTART   But when I finally told her, she supported me SENTENCEEND SENTENCESTART   She only told me that I couldn’t sit around and think about it SENTENCEEND SENTENCESTART    So I got a job as a taxi driver SENTENCEEND SENTENCESTART   I was done with business forever SENTENCEEND SENTENCESTART   That was twenty years ago SENTENCEEND SENTENCESTART   Both my kids are educated now SENTENCEEND SENTENCESTART   They’ve grown up to be good human beings SENTENCEEND SENTENCESTART   And that guy never changed SENTENCEEND SENTENCESTART   He kept cheating people SENTENCEEND SENTENCESTART   Last I heard, he was in hiding somewhere SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE (Dhaka, Bangladesh) NEXTPOST SENTENCESTART “These types of shops are notorious for cheating people SENTENCEEND SENTENCESTART   A lot of them use fake parts to fix your car SENTENCEEND SENTENCESTART   But I take pride in doing things right SENTENCEEND SENTENCESTART   And most of my profits go back into the business SENTENCEEND SENTENCESTART   I could stash all my money in the bank, but I’d rather use it to create jobs SENTENCEEND SENTENCESTART   I support five employees with this shop SENTENCEEND SENTENCESTART   I handpicked them myself SENTENCEEND SENTENCESTART   All of them were being paid horribly at their old jobs SENTENCEEND SENTENCESTART   They were living in horrible conditions SENTENCEEND SENTENCESTART   They had families to support but were barely making enough to feed themselves SENTENCEEND SENTENCESTART   So I paid them double, gave them shelter, and gave them food SENTENCEEND SENTENCESTART   If the work is getting done, they can have as much freedom as they want SENTENCEEND SENTENCESTART   I don’t give them metrics SENTENCEEND SENTENCESTART   The most important thing is that they’re happy SENTENCEEND SENTENCESTART ”       NEWLINE  NEWLINE (Dhaka, Bangladesh) NEXTPOST SENTENCESTART “The land where I grew up was very rich SENTENCEEND SENTENCESTART   The property was empty when my father bought it, but he plowed it with cows and grew many crops there SENTENCEEND SENTENCESTART   He built it up from nothing SENTENCEEND SENTENCESTART   I have many memories there as a child SENTENCEEND SENTENCESTART   The land was next to a river SENTENCEEND SENTENCESTART   There were lots of coconut trees SENTENCEEND SENTENCESTART   The trees didn’t belong to anyone but they felt like my own SENTENCEEND SENTENCESTART   My father died when I was five years old and passed the land on to me SENTENCEEND SENTENCESTART   It was my only possession SENTENCEEND SENTENCESTART   It was my back-up plan SENTENCEEND SENTENCESTART   I worked as a janitor in the city, but I always returned to visit my mother and bring her money SENTENCEEND SENTENCESTART   It was in my twenties that I began to notice that the river was eroding the soil SENTENCEEND SENTENCESTART   Every time I returned, a bit more had fallen into the water SENTENCEEND SENTENCESTART   There was nothing I could do SENTENCEEND SENTENCESTART   We stayed until the water was five feet from our door SENTENCEEND SENTENCESTART   On the day we left, my mother told me: ‘One day you’ll realize how hard your father worked for this SENTENCEEND SENTENCESTART ’  And that’s the hardest part SENTENCEEND SENTENCESTART   The land was my only memory of my father SENTENCEEND SENTENCESTART   And now I can’t show it to my kids SENTENCEEND SENTENCESTART   I feel like it’s not just my inheritance that’s underwater, but all my father’s hard work SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE (Dhaka, Bangladesh) NEXTPOST SENTENCESTART “My mother passed away during childbirth, and a woman from a nearby village tried to purchase me from my grandfather SENTENCEEND SENTENCESTART   At first he was insulted, but the woman kept begging, so he finally agreed to marry her into our family SENTENCEEND SENTENCESTART   She never breastfed me SENTENCEEND SENTENCESTART   She never showed me affection SENTENCEEND SENTENCESTART   And the moment she had children of her own, she abandoned me SENTENCEEND SENTENCESTART   I was ten years old at the time SENTENCEEND SENTENCESTART   Her new husband threw my school bag into a lake and told me to go find work SENTENCEEND SENTENCESTART   I moved straight to the city SENTENCEEND SENTENCESTART   There were times I was so hungry that I made soup out of goat food SENTENCEEND SENTENCESTART   But I never begged and I never stole SENTENCEEND SENTENCESTART   There are a lot of bad people in the city, but I was lucky enough to meet the good ones SENTENCEEND SENTENCESTART   People who showed me the right path SENTENCEEND SENTENCESTART   A restaurant owner let me live with him in exchange for washing dishes SENTENCEEND SENTENCESTART   Then when I was fifteen, an old Pakistani man taught me how to drive, and I became a bus driver SENTENCEEND SENTENCESTART   I met my wife while driving that bus SENTENCEEND SENTENCESTART   We have three children of our own now SENTENCEEND SENTENCESTART   I have nothing to leave them when I’m gone SENTENCEEND SENTENCESTART   But all of us are well fed and well housed SENTENCEEND SENTENCESTART ”        NEWLINE  NEWLINE (Dhaka, Bangladesh) NEXTPOST SENTENCESTART “My boyfriend would do all sorts of crazy things when we first met SENTENCEEND SENTENCESTART   We’d talk until four in the morning SENTENCEEND SENTENCESTART   He said he’d die to be with me SENTENCEEND SENTENCESTART   But the moment he knew that I was falling for him, he suddenly grew cold SENTENCEEND SENTENCESTART   He’d call me names SENTENCEEND SENTENCESTART   He’d disappear for days every time we argued SENTENCEEND SENTENCESTART  And he started spending time with other girls SENTENCEEND SENTENCESTART   He said they were just friends, but it bothered me SENTENCEEND SENTENCESTART   Plus he would never commit SENTENCEEND SENTENCESTART   I asked him for a commitment because my parents are trying to fix my marriage, but he just listened to me cry SENTENCEEND SENTENCESTART   He wouldn’t say a word SENTENCEEND SENTENCESTART   During one of the periods that he wasn’t talking to me, I reconnected with an old crush from my childhood SENTENCEEND SENTENCESTART   And this new guy treated me so well SENTENCEEND SENTENCESTART   He even came to meet my parents to show that he was serious SENTENCEEND SENTENCESTART   But my boyfriend logged onto my Facebook and discovered our messages SENTENCEEND SENTENCESTART   And he started crying and begged me not to leave SENTENCEEND SENTENCESTART   He said he’d marry me if I came back to him SENTENCEEND SENTENCESTART   So I did SENTENCEEND SENTENCESTART   But now he’s saying that he hasn’t decided if he can forgive me SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE (Dhaka, Bangladesh) NEXTPOST SENTENCESTART “I feel like girls aren’t safe anywhere SENTENCEEND SENTENCESTART  Maybe I’m watching too many crime series on television, but I’m scared to ever leave her alone SENTENCEEND SENTENCESTART  I even get nervous when she goes to school because the driver is male SENTENCEEND SENTENCESTART  I do my best to teach her to be safe SENTENCEEND SENTENCESTART  I tell her how to react if a man approaches her SENTENCEEND SENTENCESTART  I tell her what is not appropriate SENTENCEEND SENTENCESTART  But she’s so shy SENTENCEEND SENTENCESTART  She’s silent around adults SENTENCEEND SENTENCESTART  So I’m scared that she won’t stand up for herself SENTENCEEND SENTENCESTART  And that if something happens, I’m afraid she’ll never tell me about it SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE (Dhaka, Bangladesh) NEXTPOST SENTENCESTART “Invention is my hobby SENTENCEEND SENTENCESTART   I want to invent all kinds of inventions SENTENCEEND SENTENCESTART   Many things I have invented already SENTENCEEND SENTENCESTART   This is my first invention, which is quite small SENTENCEEND SENTENCESTART   It is a generator SENTENCEEND SENTENCESTART   One motor can generate electricity from the other motor SENTENCEEND SENTENCESTART   I will make a bigger one when I get some money SENTENCEEND SENTENCESTART   There are so many wonderful inventors SENTENCEEND SENTENCESTART  There is a scientist named Dr SENTENCEEND SENTENCESTART  Hanson who has made a wonderful robot that can talk SENTENCEEND SENTENCESTART  She can’t say her favorite color, but she is still a beautiful robot SENTENCEEND SENTENCESTART   Dr SENTENCEEND SENTENCESTART  Hanson is a great scientist and wonderful man SENTENCEEND SENTENCESTART   I will be a great scientist too SENTENCEEND SENTENCESTART   One day I will go to Australia and make a flying car that doesn’t make pollution SENTENCEEND SENTENCESTART   I already have the idea in my brain SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE (Dhaka, Bangladesh) NEXTPOST SENTENCESTART “I started this business two years ago when I was twelve SENTENCEEND SENTENCESTART   An older friend told me that we could make good money selling fruit SENTENCEEND SENTENCESTART   We buy the fruit from villages and bring it to the city where it gets a much higher price SENTENCEEND SENTENCESTART   My friend is six years older than me, but he couldn’t keep up, so I set off on my own SENTENCEEND SENTENCESTART   I work every day SENTENCEEND SENTENCESTART   I’ve already made enough money to buy some land SENTENCEEND SENTENCESTART   I’m going to build a house and use the rest for farming SENTENCEEND SENTENCESTART   My parents tell me that I should be in school SENTENCEEND SENTENCESTART   I respect their views, but I also make more money than them SENTENCEEND SENTENCESTART   So it’s hard to listen SENTENCEEND SENTENCESTART   Plus I’m learning a lot about business SENTENCEEND SENTENCESTART   Even though I’m skipping school, I don’t think I’m skipping education SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE (Dhaka, Bangladesh) NEXTPOST SENTENCESTART “I always sat in the first row SENTENCEEND SENTENCESTART   I always had the highest rank in class SENTENCEEND SENTENCESTART   I wanted to be a teacher, just like my teachers SENTENCEEND SENTENCESTART   But when it was time to enroll in grade seven, my mother told me we couldn’t afford it SENTENCEEND SENTENCESTART   I cried and begged but she just stayed silent SENTENCEEND SENTENCESTART  My teachers were so sad that they offered to pay half of the tuition SENTENCEEND SENTENCESTART   But it wasn’t enough because we\'d still have to pay for the books and exams SENTENCEEND SENTENCESTART   So my mother made me understand that school was not in my luck SENTENCEEND SENTENCESTART   I’m still seventeen, but now I’m married and I work as a maid for a family SENTENCEEND SENTENCESTART   I wash their clothes, wash their dishes, clean their bathroom SENTENCEEND SENTENCESTART   Their house is near a school SENTENCEEND SENTENCESTART   So every morning I have to watch the children walk by in their uniforms SENTENCEEND SENTENCESTART ”         NEWLINE  NEWLINE (Dhaka, Bangladesh) NEXTPOST SENTENCESTART “My husband passed away when my children were very small SENTENCEEND SENTENCESTART   I taught myself handicrafts to survive, but it barely earned enough for us to eat SENTENCEEND SENTENCESTART   When my oldest son turned eighteen, I found him a wife SENTENCEEND SENTENCESTART   I was hoping that she’d help with the household SENTENCEEND SENTENCESTART   But she abandoned us after my granddaughter was born SENTENCEEND SENTENCESTART   I came home from work one day and found the child alone SENTENCEEND SENTENCESTART   I could only get her to stop crying by soaking an apple in goat’s milk SENTENCEEND SENTENCESTART   I’ve been raising her ever since that day SENTENCEEND SENTENCESTART   She calls me ‘mummy SENTENCEEND SENTENCESTART ’  With a lot of hardships I have made her grow SENTENCEEND SENTENCESTART   She survives on apples and milk SENTENCEEND SENTENCESTART   But I’m old SENTENCEEND SENTENCESTART   And when I’m gone, I don’t know who will take care of her SENTENCEEND SENTENCESTART ”    NEWLINE  NEWLINE (Udaipur, India) NEXTPOST SENTENCESTART Today in microfashion SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE (Calcutta, India) NEXTPOST SENTENCESTART “I want to be a police SENTENCEEND SENTENCESTART   I’ll find the robbers because they have handkerchiefs on their faces SENTENCEEND SENTENCESTART   I’ll tell them that it’s bad to steal SENTENCEEND SENTENCESTART   And to never steal again SENTENCEEND SENTENCESTART   Then I’ll hit them with a stick and their mom and dad will yell at them SENTENCEEND SENTENCESTART   And if they don’t listen, I’ll hit them with a stick again SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE (Udaipur, India) NEXTPOST SENTENCESTART “I’m known for my honesty SENTENCEEND SENTENCESTART   My grandfather taught me that it’s better to carry rocks on your head than to make a dishonest living SENTENCEEND SENTENCESTART   I’ve never taken a bribe SENTENCEEND SENTENCESTART   I’ve worked in the judicial court for decades and have been offered all sorts of money and gifts SENTENCEEND SENTENCESTART   A hospital once offered me an entire floor of their building, if only I’d settle a dispute in their favor SENTENCEEND SENTENCESTART   I told them to take it up with someone else SENTENCEEND SENTENCESTART ”     NEWLINE  NEWLINE (Udaipur, India) NEXTPOST SENTENCESTART “After we met, I got tired of waiting for him to call SENTENCEEND SENTENCESTART   So I dialed his number, let it ring once, and hung up SENTENCEEND SENTENCESTART   When he called back I told him it was an accident SENTENCEEND SENTENCESTART   Then we spoke for an hour SENTENCEEND SENTENCESTART   Now we’re on our honeymoon SENTENCEEND SENTENCESTART ”       NEWLINE  NEWLINE (Udaipur, India) NEXTPOST SENTENCESTART “We’re from different castes so we have to meet in secret SENTENCEEND SENTENCESTART   My parents would never approve SENTENCEEND SENTENCESTART   When my older sister married someone outside of the caste, everyone stopped talking to her SENTENCEEND SENTENCESTART   So we have to be careful SENTENCEEND SENTENCESTART   My parents actually know about him SENTENCEEND SENTENCESTART   He sent me a bunch of heart emojis once and my Mom picked up the phone SENTENCEEND SENTENCESTART   I tried to say we were just friends but she saw the words ‘Darling’ and ‘Sweetheart SENTENCEEND SENTENCESTART ’  She slapped me when she saw it SENTENCEEND SENTENCESTART   She thinks that we don’t talk anymore SENTENCEEND SENTENCESTART   When I’m financially independent, I’ll finally tell her the truth SENTENCEEND SENTENCESTART   But it’s going to be bad SENTENCEEND SENTENCESTART ”     NEWLINE  NEWLINE -Calcutta, India NEXTPOST SENTENCESTART “I want to do something for our nation SENTENCEEND SENTENCESTART   At first I wanted to be a doctor, but I discovered another path to saving lives SENTENCEEND SENTENCESTART   I’m studying agriculture now SENTENCEEND SENTENCESTART   I’d like to help invent new seeds to increase yields SENTENCEEND SENTENCESTART   Small farmers in India are struggling SENTENCEEND SENTENCESTART   They just can’t compete with industrial farms SENTENCEEND SENTENCESTART   Most of them live in isolated villages, and it costs too much to bring their goods to market SENTENCEEND SENTENCESTART   It can be cheaper for them to dump their crops on the side of the road SENTENCEEND SENTENCESTART   Meanwhile people in our country are going hungry!  It doesn’t make any sense SENTENCEEND SENTENCESTART   But if we invent new seeds that increase the yield of their land, small farmers can survive SENTENCEEND SENTENCESTART   If I do my job well, there will be no need for doctors SENTENCEEND SENTENCESTART   Also my mom would like to see my name in the newspaper SENTENCEEND SENTENCESTART ”    NEWLINE  NEWLINE (Udaipur, India) NEXTPOST SENTENCESTART “Marriage is about two things: sexual satisfaction and building generations SENTENCEEND SENTENCESTART  Nothing more SENTENCEEND SENTENCESTART   Only useless people are thinking about love SENTENCEEND SENTENCESTART   The result of a love marriage is never satisfactory SENTENCEEND SENTENCESTART   Divorce, arguments, affairs SENTENCEEND SENTENCESTART   These things don’t happen in arranged marriage SENTENCEEND SENTENCESTART   Arranged marriage is always successful SENTENCEEND SENTENCESTART   Love is for useless people SENTENCEEND SENTENCESTART   But if you’re going to feel love, at the very least, make sure it’s someone of a similar income level SENTENCEEND SENTENCESTART ”  NEWLINE  NEWLINE (Jaipur, India) NEXTPOST SENTENCESTART “We have to keep our relationship secret SENTENCEEND SENTENCESTART   Our parents would not approve and we’re not courageous enough to tell them yet SENTENCEEND SENTENCESTART   So we meet in secret three or four times per month SENTENCEEND SENTENCESTART   Since the beginning of our relationship, we’ve shared a diary SENTENCEEND SENTENCESTART   We take turns keeping it SENTENCEEND SENTENCESTART   Whoever has it will write down our memories SENTENCEEND SENTENCESTART   They’ll also write down what they want from the other person, and how they feel misunderstood SENTENCEEND SENTENCESTART   Then every time we meet—we hand it off SENTENCEEND SENTENCESTART ”       NEWLINE  NEWLINE (Calcutta, India) NEXTPOST SENTENCESTART (1/7) “This was the bench we sat on the night that Avi had his biopsy SENTENCEEND SENTENCESTART   He had been making weird breathing sounds SENTENCEEND SENTENCESTART   The pediatrician sent us here because she saw something on his X-ray SENTENCEEND SENTENCESTART   Avi was eleven at the time SENTENCEEND SENTENCESTART   I didn’t want him to feel scared so I told him it was just a silly little test, and we’d be going home soon SENTENCEEND SENTENCESTART   I walked with my back against the wall to hide all the signs that said ‘cancer SENTENCEEND SENTENCESTART ’  They took Avi in the back and we waited on this bench for a long time SENTENCEEND SENTENCESTART   It was Friday night and the place was empty SENTENCEEND SENTENCESTART   It started getting late SENTENCEEND SENTENCESTART   It was taking too long SENTENCEEND SENTENCESTART   When the doctors finally came back they looked very scared SENTENCEEND SENTENCESTART   The doctor told us, ‘We’re having a difficult time keeping his airway open SENTENCEEND SENTENCESTART ’  I was so confused SENTENCEEND SENTENCESTART   This was just supposed to be a test SENTENCEEND SENTENCESTART   I asked him: ‘What do you mean?’  He said: ‘Avi could die SENTENCEEND SENTENCESTART ’  He kept repeating it: ‘Avi could die SENTENCEEND SENTENCESTART ’  Then he said: ‘It’s time to pray SENTENCEEND SENTENCESTART ’” NEXTPOST SENTENCESTART “The absolute best thing in the world that can happen to me is telling a parent that their child’s tumor is benign SENTENCEEND SENTENCESTART   I live for those moments SENTENCEEND SENTENCESTART   And the worst thing that can happen to me is telling a parent that I’ve lost their kid SENTENCEEND SENTENCESTART   It’s only happened to me five times in thirty years SENTENCEEND SENTENCESTART   And I’ve wanted to kill myself every single time SENTENCEEND SENTENCESTART   Those parents trusted me with their child SENTENCEEND SENTENCESTART   It’s a sacred trust and the ultimate responsibility is always mine SENTENCEEND SENTENCESTART   I lose sleep for days SENTENCEEND SENTENCESTART   I second-guess every decision I made SENTENCEEND SENTENCESTART   And every time I lose a child, I tell the parents: ‘I’d rather be dead than her SENTENCEEND SENTENCESTART ’  And I mean it SENTENCEEND SENTENCESTART   But I go to church every single day SENTENCEEND SENTENCESTART   And I think that I’m going to see those kids in a better place SENTENCEEND SENTENCESTART   And I’m going to tell them that I’m sorry SENTENCEEND SENTENCESTART   And hopefully they’ll say, ‘Forget it SENTENCEEND SENTENCESTART   Come on in SENTENCEEND SENTENCESTART ’” NEWLINE  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  NEWLINE As we learn these stories, we are trying to raise $1,000,000 to help the team at Memorial Sloan Kettering Cancer Center in their fight against pediatric cancer SENTENCEEND SENTENCESTART  Thanks to the 15,000 people who have contributed so far SENTENCEEND SENTENCESTART  We\'re almost 60% of the way there SENTENCEEND SENTENCESTART  Please consider donating: http://bit SENTENCEEND SENTENCESTART ly/1TpFcdy NEXTPOST SENTENCESTART “If there’s a chance then it’s worth a try SENTENCEEND SENTENCESTART   Even if nobody else wants to try, I will try SENTENCEEND SENTENCESTART  A lot of these kids have exhausted all their options SENTENCEEND SENTENCESTART   They may have had several surgeries elsewhere and it’s either hospice or one more try SENTENCEEND SENTENCESTART   My colleagues are amazing SENTENCEEND SENTENCESTART   So I know that if I can get this lump out, the child has a chance SENTENCEEND SENTENCESTART   I view each of these kids as my own SENTENCEEND SENTENCESTART   My team is amazing but I take 100% responsibility for the outcome and I don’t like to lose a drop of blood SENTENCEEND SENTENCESTART   So it’s a lot of stress SENTENCEEND SENTENCESTART   I have four grafts in my heart SENTENCEEND SENTENCESTART   My neck muscles are always tense SENTENCEEND SENTENCESTART   Some of these surgeries have probably taken years off my life SENTENCEEND SENTENCESTART   But tumors kill kids in very horrible ways SENTENCEEND SENTENCESTART   So if there’s a chance, I will try SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  NEWLINE  NEWLINE As we learn these stories, we are trying to raise $1,000,000 to help the team at Memorial Sloan Kettering Cancer Center in their fight against pediatric cancer SENTENCEEND SENTENCESTART  Thanks to the 15,000 people who have contributed so far SENTENCEEND SENTENCESTART  We\'re almost 60% of the way there SENTENCEEND SENTENCESTART   Please consider donating: http://bit SENTENCEEND SENTENCESTART ly/1TpFcdy NEXTPOST SENTENCESTART (3/3) “I think it might be easier for children because they don’t understand what can happen SENTENCEEND SENTENCESTART   They don’t know the ‘what if’s SENTENCEEND SENTENCESTART ’  She can handle how bad it is because she doesn’t know how bad it can get SENTENCEEND SENTENCESTART   All she worries about is playing SENTENCEEND SENTENCESTART   You can’t even tell when she’s feeling bad SENTENCEEND SENTENCESTART   She uses her IV pole as a skateboard SENTENCEEND SENTENCESTART   She skips through the hall and sings Dora SENTENCEEND SENTENCESTART   She climbs rocks and rides her bike SENTENCEEND SENTENCESTART   I always have to remind her that she’s sick SENTENCEEND SENTENCESTART   I’m always telling her that we can do more things once she feels better SENTENCEEND SENTENCESTART   And whenever her friend has a birthday party, she tells me: ‘I’m all better now!’” NEWLINE  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  NEWLINE  NEWLINE As we learn these stories, we are trying to raise $1,000,000 to help the team at Memorial Sloan Kettering Cancer Center in their fight against pediatric cancer SENTENCEEND SENTENCESTART  Thanks to the 15,000 people who have contributed so far SENTENCEEND SENTENCESTART  We\'re more than halfway there! Please consider donating: http://bit SENTENCEEND SENTENCESTART ly/1TpFcdy NEXTPOST SENTENCESTART (2/3)  “I cried all weekend when she was diagnosed SENTENCEEND SENTENCESTART   But I made sure that I ducked into other rooms so nobody would see me SENTENCEEND SENTENCESTART   It’s a little tougher being a man because you feel like you’re supposed to be the rock SENTENCEEND SENTENCESTART   You want to hold yourself together so the family can lean on you SENTENCEEND SENTENCESTART   I’m used to always being in control SENTENCEEND SENTENCESTART   I own my own business SENTENCEEND SENTENCESTART   I’ve always been the ‘go-to-guy’ for everybody else SENTENCEEND SENTENCESTART   But I have no control over this SENTENCEEND SENTENCESTART   And that’s tough SENTENCEEND SENTENCESTART   I just have to watch my daughter suffer and there’s nothing I can do about it SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART (1/3)  “I feel like it’s draining us SENTENCEEND SENTENCESTART   Both emotionally and physically SENTENCEEND SENTENCESTART   Her immune system is so depleted that if she gets sick, it could kill her SENTENCEEND SENTENCESTART   So I’m afraid all the time SENTENCEEND SENTENCESTART   And that fear tends to keep me on the attack SENTENCEEND SENTENCESTART   I can be short tempered with my husband and my boys SENTENCEEND SENTENCESTART   I feel like if I scream, everyone will stay away from her and she’ll be OK SENTENCEEND SENTENCESTART   My husband and I have been fighting a lot SENTENCEEND SENTENCESTART   We’ll snap at each other over little things like the chores or giving her medicine SENTENCEEND SENTENCESTART   Before the diagnosis, we were always sure to talk things out before bed SENTENCEEND SENTENCESTART   But now we’re both so stressed that we hold stuff in SENTENCEEND SENTENCESTART   He doesn’t know how I’ll react SENTENCEEND SENTENCESTART   And I don’t know how he’ll react SENTENCEEND SENTENCESTART   So we just choose not to discuss our problems SENTENCEEND SENTENCESTART   This Saturday we went on our first date since the diagnosis SENTENCEEND SENTENCESTART   It was only two hours at an Italian restaurant, but it was nice to finally talk SENTENCEEND SENTENCESTART   We acknowledged that we’ve been on edge SENTENCEEND SENTENCESTART   And we apologized to each other SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART “Some of my colleagues tell me they can’t imagine working in pediatrics SENTENCEEND SENTENCESTART   Millions of years of evolution have conditioned us to respond to the cries of a child SENTENCEEND SENTENCESTART   We can’t bear to see a child in pain SENTENCEEND SENTENCESTART   And once we have children of our own, it makes the work even more difficult SENTENCEEND SENTENCESTART   We all handle it differently, but everyone cries at some point SENTENCEEND SENTENCESTART   Not in front of the patient, but everyone cries SENTENCEEND SENTENCESTART   Every few months we have a ceremony where we mourn all the children who have passed away SENTENCEEND SENTENCESTART   We have a slideshow SENTENCEEND SENTENCESTART   We make cards SENTENCEEND SENTENCESTART   We talk about them and remember them together SENTENCEEND SENTENCESTART   We acknowledge that we all feel the loss SENTENCEEND SENTENCESTART   And even though our grief is not as significant as the family’s, it’s not trivial either SENTENCEEND SENTENCESTART   And we must take time to acknowledge that SENTENCEEND SENTENCESTART   Or all of us will burn out SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART (2/2)  “What did I do?  It was certainly nothing she did SENTENCEEND SENTENCESTART   She’s just a child SENTENCEEND SENTENCESTART   It feels like we’re being punished for something I did SENTENCEEND SENTENCESTART   But I’m nice to people SENTENCEEND SENTENCESTART   I’ve never cheated on my husband SENTENCEEND SENTENCESTART   I’m nice to my parents SENTENCEEND SENTENCESTART   I feel so guilty SENTENCEEND SENTENCESTART   She was stage four when they discovered it SENTENCEEND SENTENCESTART   I should have known sooner SENTENCEEND SENTENCESTART   I should have listened to her complaints more SENTENCEEND SENTENCESTART   I should have said: ‘Maybe it’s not a pulled muscle SENTENCEEND SENTENCESTART   Let’s go to the doctor right this moment SENTENCEEND SENTENCESTART ’  Only eighty kids per year get this cancer SENTENCEEND SENTENCESTART   When she first got diagnosed it hurt me to look at her friends SENTENCEEND SENTENCESTART   They had their long hair, and they were driving their cars, and going to prom, and having boyfriends SENTENCEEND SENTENCESTART   They’re great kids but I couldn’t look at them without wondering: ‘Why? Why do they get to have a future?’  There’s a 23% survival rate SENTENCEEND SENTENCESTART   I try not to fixate on that number because I get so sad and I don’t want to go there SENTENCEEND SENTENCESTART   So I live as an actress SENTENCEEND SENTENCESTART   I’m playing the role of a happy person, but all I feel like is lying in bed and crying SENTENCEEND SENTENCESTART   The mom inside that hospital room helps her plan for her future SENTENCEEND SENTENCESTART   The mom inside that room believes her when she tells me that she’s not going anywhere SENTENCEEND SENTENCESTART   But the mom out here doesn’t know what to believe SENTENCEEND SENTENCESTART ” NEWLINE  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR - NEWLINE As we learn these stories, we are trying to raise $1,000,000 to help the team at Memorial Sloan Kettering Cancer Center in their fight against pediatric cancer SENTENCEEND SENTENCESTART  Thanks to the 11,000 people who have contributed so far SENTENCEEND SENTENCESTART  We\'re over 40% there SENTENCEEND SENTENCESTART  Please consider donating: http://bit SENTENCEEND SENTENCESTART ly/1TpFcdy NEXTPOST SENTENCESTART (1/2) “I got diagnosed last January SENTENCEEND SENTENCESTART  A mass behind my spine, two masses in my lungs, spots all over my lymph nodes and bone marrow SENTENCEEND SENTENCESTART  The guy who gave me the CT scan threw up afterward SENTENCEEND SENTENCESTART  The doctor said they could guarantee three years SENTENCEEND SENTENCESTART  I was like: ‘Three years SENTENCEEND SENTENCESTART  Holy shit SENTENCEEND SENTENCESTART ’ My biggest worry is that I’m going to die and not do all the things I wanted to do SENTENCEEND SENTENCESTART  The funny thing is—I didn’t even realize how many things I wanted to do until I got diagnosed SENTENCEEND SENTENCESTART  Simple things like meeting a guy, getting married, getting a job, having my own apartment, and even picking out my own furniture SENTENCEEND SENTENCESTART  Those never seemed too interesting to me SENTENCEEND SENTENCESTART  They just seemed like adult things that were guaranteed to happen SENTENCEEND SENTENCESTART  Now I want to do them so bad SENTENCEEND SENTENCESTART  Because I want to know what they feel like SENTENCEEND SENTENCESTART ”  NEWLINE  NEWLINE  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR - NEWLINE  NEWLINE As we learn these stories, we are trying to raise $1,000,000 to help the team at Memorial Sloan Kettering Cancer Center in their fight against pediatric cancer SENTENCEEND SENTENCESTART  Thanks to the 11,000 people who have contributed so far SENTENCEEND SENTENCESTART   We\'re almost almost 40% there SENTENCEEND SENTENCESTART  Please consider donating: http://bit SENTENCEEND SENTENCESTART ly/1TpFcdy NEXTPOST SENTENCESTART “I got cancer in the summer when the pools were opening SENTENCEEND SENTENCESTART   And I really wanted to go swimming but I couldn’t leave the hospital SENTENCEEND SENTENCESTART   I begged her not to go swimming without me SENTENCEEND SENTENCESTART   And she didn’t because I couldn’t SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART (2/2) “You have to have faith and keep working SENTENCEEND SENTENCESTART   Back in the 70’s and 80’s, all of us were hoping for just a single survivor of stage four neuroblastoma SENTENCEEND SENTENCESTART   It was a rare cancer and we just couldn’t cure it SENTENCEEND SENTENCESTART   But eventually we figured it out SENTENCEEND SENTENCESTART   Recently over five hundred people attended a party we threw for neuroblastoma survivors SENTENCEEND SENTENCESTART  So change does happen SENTENCEEND SENTENCESTART   It just happens slowly SENTENCEEND SENTENCESTART   I have a colleague who lost hope recently SENTENCEEND SENTENCESTART   He’s been working on a brain tumor called DIPG, and he’s had nothing but three decades of negative outcomes SENTENCEEND SENTENCESTART   Dozens and dozens of failed trials SENTENCEEND SENTENCESTART   We just couldn’t touch the tumor because it’s in the main center of the brain SENTENCEEND SENTENCESTART   But my colleague stayed optimistic SENTENCEEND SENTENCESTART   He kept cheering us on SENTENCEEND SENTENCESTART   But he finally lost hope SENTENCEEND SENTENCESTART   After three decades of losing kids, he asked to not see any more DIPG patients SENTENCEEND SENTENCESTART   Then guess what happened SENTENCEEND SENTENCESTART   We finally have a survivor on our hands SENTENCEEND SENTENCESTART   Our neurosurgeon Dr SENTENCEEND SENTENCESTART  Souweidane figured out how to insert a catheter directly into the tumor SENTENCEEND SENTENCESTART   And we now have a girl that is 3 SENTENCEEND SENTENCESTART 5 years from diagnosis SENTENCEEND SENTENCESTART   It’s still early, but it’s promising SENTENCEEND SENTENCESTART   She plays tennis SENTENCEEND SENTENCESTART   She plays violin SENTENCEEND SENTENCESTART   And she is gorgeous SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART (1/2) “Last week the Mets were down 3-0 in the seventh inning, and Céspedes tied the game with one swing SENTENCEEND SENTENCESTART   Well that doesn’t ever happen with cancer SENTENCEEND SENTENCESTART   Nothing is ever immediate SENTENCEEND SENTENCESTART   And the hardest part about being an oncologist is trying to be patient SENTENCEEND SENTENCESTART   My daughter is a huge Grey’s Anatomy fan SENTENCEEND SENTENCESTART   She loves those emergency room stories because they provide an immediate fix SENTENCEEND SENTENCESTART   But with cancer, there’s never a simple answer SENTENCEEND SENTENCESTART   And at this hospital we see some of the most difficult, challenging cases SENTENCEEND SENTENCESTART   You want so much to give that toddler’s parent some sort of guarantee, but you can’t SENTENCEEND SENTENCESTART   There’s no guarantee that the treatment will work SENTENCEEND SENTENCESTART   And if it doesn\'t work, there’s no guarantee that a new treatment will be developed in time SENTENCEEND SENTENCESTART   All we can do is try SENTENCEEND SENTENCESTART   And wait SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART (3/3) "I grew up around nature SENTENCEEND SENTENCESTART  I had a wonderful family and a great life and it was so easy to be a believer SENTENCEEND SENTENCESTART  Then over the course of a single weekend, I learned that my one-year-old son was blind, had a seizure disorder, and a brain tumor SENTENCEEND SENTENCESTART  I remember I went to a beach, and a storm was coming in, and I just sat on the edge of the ocean and I wailed SENTENCEEND SENTENCESTART  For an hour I screamed in the pouring rain SENTENCEEND SENTENCESTART  That was thirteen years ago, and there hasn’t been a moment of relaxation since then SENTENCEEND SENTENCESTART  We’ve researched everything SENTENCEEND SENTENCESTART  We’ve tried everything SENTENCEEND SENTENCESTART  Anything to keep the tumor from growing SENTENCEEND SENTENCESTART  And the longer we’ve gone on, the more we’ve tried, and the narrower the choices get SENTENCEEND SENTENCESTART  There is nothing I won’t do to save my child SENTENCEEND SENTENCESTART   There is not a doctor you can keep me from SENTENCEEND SENTENCESTART  I’ll drive across the country for a single conversation SENTENCEEND SENTENCESTART  But I live with such pain SENTENCEEND SENTENCESTART  It’s not rocket science SENTENCEEND SENTENCESTART  Every day could be the day that I lose my child SENTENCEEND SENTENCESTART  But I’m trying to look up SENTENCEEND SENTENCESTART  I’m trying to have gratitude SENTENCEEND SENTENCESTART  And I’m trying to keep my faith SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART (2/3) "Sterling is the sanest one in our family SENTENCEEND SENTENCESTART  He’s our leader SENTENCEEND SENTENCESTART  He gets to be himself SENTENCEEND SENTENCESTART  If we were ourselves, we’d be rather useless because all the time we feel like disintegrating SENTENCEEND SENTENCESTART  We call him the Love Bug SENTENCEEND SENTENCESTART  He is always loving and happy SENTENCEEND SENTENCESTART  Every morning he sings a song while we walk down the driveway SENTENCEEND SENTENCESTART  It goes: ‘Another new day, coming my way! I can’t wait to go to school today!’ I look at him and think, ‘What is my problem?’ Then I tell myself, ‘Just follow the leader SENTENCEEND SENTENCESTART ’" NEWLINE  NEWLINE  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  NEWLINE  NEWLINE As we learn the stories of doctors, nurses, and patients such as Sterling, we are raising money to research and cure the rare pediatric cancers they are battling against SENTENCEEND SENTENCESTART   So far over 8,500 people have donated and we’ve raised over $300,000 SENTENCEEND SENTENCESTART   Thanks to all of you SENTENCEEND SENTENCESTART   Even if only one percent of the people following this page were to make a donation, that would amount to nearly 200,000 donations and over six million dollars SENTENCEEND SENTENCESTART   So please consider being counted SENTENCEEND SENTENCESTART   Many children come to Memorial Sloan Kettering Cancer Center when they are out of options, and new options must be invented SENTENCEEND SENTENCESTART   The study of rare cancers involves small and relentless teams of researchers SENTENCEEND SENTENCESTART   Life-saving breakthroughs are made on very tight budgets SENTENCEEND SENTENCESTART   So your donations will make a difference SENTENCEEND SENTENCESTART   They may save a life SENTENCEEND SENTENCESTART   Please consider being counted: http://bit SENTENCEEND SENTENCESTART ly/1TpFcdy NEXTPOST SENTENCESTART (1/3) "We’ve been fighting this for thirteen years SENTENCEEND SENTENCESTART  Sterling has a brain tumor in the center of his brain where the optic nerves cross SENTENCEEND SENTENCESTART  It’s inoperable SENTENCEEND SENTENCESTART  Our lives center around keeping the tumor from growing SENTENCEEND SENTENCESTART  That’s what we do SENTENCEEND SENTENCESTART  We’re here today to pick up a new experimental medicine SENTENCEEND SENTENCESTART  Sterling’s had over one thousand seizures SENTENCEEND SENTENCESTART  I joke that this whole experience has made me an involuntary Buddhist SENTENCEEND SENTENCESTART  When you live in a world of one thousand seizures, you have no choice but to live in the present SENTENCEEND SENTENCESTART  You’re jolted out of your mind every few minutes SENTENCEEND SENTENCESTART  And you learn about compassion SENTENCEEND SENTENCESTART  Having a special needs child has opened me up to the compassion of other people SENTENCEEND SENTENCESTART  There are so many people who are willing to help SENTENCEEND SENTENCESTART  When we first discovered the tumor, I sent Sterling’s scans to every hospital SENTENCEEND SENTENCESTART  I can’t tell you how many doctors gave me their time and didn’t charge a thing SENTENCEEND SENTENCESTART  Zero billable hours SENTENCEEND SENTENCESTART  Can you believe it? It was like going snorkeling for the first time, and discovering a whole new world of color that I didn’t know existed SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART “The nurse is in that room day in and day out SENTENCEEND SENTENCESTART   You give a piece of yourself to that child SENTENCEEND SENTENCESTART   But intimacy has its dangers SENTENCEEND SENTENCESTART   You have to be able to set it aside SENTENCEEND SENTENCESTART  You can’t come in on your days off SENTENCEEND SENTENCESTART   You have to be able to go home at the end of the day and have a glass of wine, or go rock climbing, or visit with friends SENTENCEEND SENTENCESTART   If you can’t go home and rebuild, you’ll burn out SENTENCEEND SENTENCESTART   You won’t be able to handle the losses if you’re just surviving off the wins SENTENCEEND SENTENCESTART   Because the losses are severe SENTENCEEND SENTENCESTART   You were allowed into that child’s life at their most intimate time, and you were trusted SENTENCEEND SENTENCESTART   And that is a gift SENTENCEEND SENTENCESTART   And even in death, you learned something from that child that made you a better person and a better nurse SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART “A big part of a nurse’s job is translation SENTENCEEND SENTENCESTART   We have to turn medical language into common language SENTENCEEND SENTENCESTART   We explain the ‘why’s SENTENCEEND SENTENCESTART ’  Why they can’t eat SENTENCEEND SENTENCESTART   Why there is pain SENTENCEEND SENTENCESTART   Why their hair is falling out SENTENCEEND SENTENCESTART   You never know what those big medical words mean to a child, so we do everything we can to demystify them SENTENCEEND SENTENCESTART   If they play sports, we may describe their tumor as a baseball SENTENCEEND SENTENCESTART   And everyone knows that baseballs don’t belong in your belly SENTENCEEND SENTENCESTART   Ninety percent of them play video games, so sometimes the cancer is a monster SENTENCEEND SENTENCESTART   We’ve got to shoot the monster SENTENCEEND SENTENCESTART   We’ve got to bomb the monster SENTENCEEND SENTENCESTART   But we’re going to work together and get that monster SENTENCEEND SENTENCESTART   We’ll use any frame of reference that they understand: their favorite TV show, their favorite book, their favorite toy SENTENCEEND SENTENCESTART   And if we have an adolescent who’s a little bit angry, we’ll just shove our foot up the cancer’s ass SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART “The MDs build the treatment plan SENTENCEEND SENTENCESTART   The nurse’s job is to get it done SENTENCEEND SENTENCESTART   We’re the ones who are always there, making sure every single moment of every single day is the best it can possibly be SENTENCEEND SENTENCESTART   What’s going to take away that nausea?  What’s going to take away that pain?  How can we convince the doctor to let this kid see some sunshine?  We know when the kid has a play at school SENTENCEEND SENTENCESTART   We know which massage therapist they love and which member of the family is most likely to persuade them to take their medicine SENTENCEEND SENTENCESTART   These kids rely on certain nurses like they’re gold SENTENCEEND SENTENCESTART   A lot of time these kids won’t listen to the doctor SENTENCEEND SENTENCESTART   But they’ll listen to their nurse SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART (6/6) "If your child has brain cancer, I can give you some advice SENTENCEEND SENTENCESTART  First I would say to you: Oh my gosh! Your child has brain cancer SENTENCEEND SENTENCESTART  I’m so sorry that happened to you SENTENCEEND SENTENCESTART  I’m sure you’re feeling very sad, but don’t be worried because my mom was sad too SENTENCEEND SENTENCESTART  I actually have five words for you: It’s the saddest thing ever SENTENCEEND SENTENCESTART  So you can be sad whenever you want SENTENCEEND SENTENCESTART  If your child is sad, something you can do is tell them to never give up SENTENCEEND SENTENCESTART  If they are getting a needle, you’ll probably feel them squeezing your hand really, really, really tight SENTENCEEND SENTENCESTART  Tell them: ‘Don’t worry SENTENCEEND SENTENCESTART  This is a one time thing SENTENCEEND SENTENCESTART ’ The hardest part will be seeing your child with a line to a machine that gives them weird medications that might hurt and make them sad SENTENCEEND SENTENCESTART  Then you can give your child a lot of hugs because that will make them less sad SENTENCEEND SENTENCESTART  And your child will say: ‘Don’t worry Mom, I love you and I’m going to make it through this SENTENCEEND SENTENCESTART ’ And then you can hug them even more SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR - NEWLINE  NEWLINE We are currently holding a fundraiser to help study and cure rare cancers like Gabe’s SENTENCEEND SENTENCESTART   So far we’ve raised $50,000 from 1400 donations SENTENCEEND SENTENCESTART   That’s a great start SENTENCEEND SENTENCESTART   If even one percent of the people who follow this page were to donate, that would be 175,000 donations and the results would be staggering SENTENCEEND SENTENCESTART   A relatively tiny amount of us could have a giant impact SENTENCEEND SENTENCESTART   So please consider being counted!  Rare and specialized cancers like Gabe’s require innovation SENTENCEEND SENTENCESTART   There are numerous instances of dedicated researchers making life-saving breakthroughs at Memorial Sloan Kettering with small amounts of money SENTENCEEND SENTENCESTART   Please donate here: http://bit SENTENCEEND SENTENCESTART ly/1TpFcdy NEXTPOST SENTENCESTART (5/6) "My biggest challenge? Two words for you: third grade SENTENCEEND SENTENCESTART  It’s kind of like second grade but harder SENTENCEEND SENTENCESTART  I was a very special student in second grade because I had a brain tumor SENTENCEEND SENTENCESTART  A very rare one, actually SENTENCEEND SENTENCESTART  I was the only one in the world with this type of brain tumor SENTENCEEND SENTENCESTART  Everyone who knew me was shocked! Their heads blew up! I’ve been through a lot of things this past year SENTENCEEND SENTENCESTART  But I can tell you, if you get brain cancer, try not to worry! It will be very hard and you will get lots of fevers but you have to be brave SENTENCEEND SENTENCESTART  You have to be brave like me because I’m very brave about this thing SENTENCEEND SENTENCESTART  And if you don’t know how to be brave, I can teach you SENTENCEEND SENTENCESTART  I know the surgery seems scary, but I have four words for you: you’ll be on anesthetics SENTENCEEND SENTENCESTART  When you wake up, your head will be wrapped like a mummy and your mom will take a picture and show you SENTENCEEND SENTENCESTART  When it’s time to get shots, do a countdown from thirty and tell yourself: ‘Calm down, calm down, calm down SENTENCEEND SENTENCESTART ’ Then whenever you’re ready, tell the nurse to go SENTENCEEND SENTENCESTART  And if you need more time, ask for more time!" NEWLINE  NEWLINE  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR  SEPARATOR - NEWLINE  NEWLINE We are currently holding a fundraiser to help study and cure rare cancers like Gabe’s SENTENCEEND SENTENCESTART   Almost 1000 people have donated so far SENTENCEEND SENTENCESTART   I hope that over the course of the series, more people will consider donating SENTENCEEND SENTENCESTART   Rare and specialized cancers like Gabe’s require innovation SENTENCEEND SENTENCESTART   And there are numerous instances of dedicated researchers at Memorial Sloan Kettering Cancer Center making life-saving breakthroughs with small amounts of money SENTENCEEND SENTENCESTART   We can make a difference: http://bit SENTENCEEND SENTENCESTART ly/1TpFcdy NEXTPOST SENTENCESTART (4/6) "After the surgery, we thought it was over SENTENCEEND SENTENCESTART  We think it’s done SENTENCEEND SENTENCESTART  Gabriel is getting better and it’s like nothing happened SENTENCEEND SENTENCESTART  His teachers can’t believe it! We’re even planning on going to the beach SENTENCEEND SENTENCESTART  But the doctors tell us that they can’t identify the tumor SENTENCEEND SENTENCESTART  The surgery was in July SENTENCEEND SENTENCESTART  August passes SENTENCEEND SENTENCESTART  September passes SENTENCEEND SENTENCESTART  Now that the tumor was gone, we were anxious to start treating the cancer, but nobody knows where to start SENTENCEEND SENTENCESTART  Every hospital is saying something different SENTENCEEND SENTENCESTART  Then finally two hospitals gave the same opinion: Descmoplastic Small Round Blue Cell Tumor SENTENCEEND SENTENCESTART  Nobody had ever seen this tumor in the brain before SENTENCEEND SENTENCESTART  They told me not to read about it SENTENCEEND SENTENCESTART  They told me that every case was different and not to read about it SENTENCEEND SENTENCESTART  When you read about it, it’s very bad SENTENCEEND SENTENCESTART  Oh my God SENTENCEEND SENTENCESTART  This cancer always comes back SENTENCEEND SENTENCESTART  And when it comes back, it’s worse SENTENCEEND SENTENCESTART  ‘Less than three years,’ it says SENTENCEEND SENTENCESTART  Oh my God SENTENCEEND SENTENCESTART  What did I do? What did I expose him to? What did I feed him? The chemo is so painful for him SENTENCEEND SENTENCESTART  My family tried to talk me out of it SENTENCEEND SENTENCESTART  They told me that I’m killing my son with my own hands SENTENCEEND SENTENCESTART  But what can I do? There’s nothing I can do SENTENCEEND SENTENCESTART  I want to give blood SENTENCEEND SENTENCESTART  I want to give bone marrow SENTENCEEND SENTENCESTART  But all I can do is watch SENTENCEEND SENTENCESTART  It’s the worst show you can imagine, but you have to watch SENTENCEEND SENTENCESTART  You’re forced to watch SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART (3/6) "The doctors gave us peace of mind SENTENCEEND SENTENCESTART  They seemed so sure of their profession SENTENCEEND SENTENCESTART  They were using all these medical terms and speaking to each other so calmly SENTENCEEND SENTENCESTART  They spoke about the tumor like it was a simple puzzle SENTENCEEND SENTENCESTART  We tried to explain the surgery to Gabe as if it was a cartoon SENTENCEEND SENTENCESTART  He loved cartoons SENTENCEEND SENTENCESTART  We told him that there was a black hole that was sucking all the good energy out of his brain SENTENCEEND SENTENCESTART  We told him that he was going to be cut a little bit, but we did not tell him how much SENTENCEEND SENTENCESTART  I told him that he may have difficulty speaking when he wakes up, but don’t worry, because we’re going to write to Mommy on a notepad SENTENCEEND SENTENCESTART  But I’m thinking inside that I’m never going to hear my son speak again SENTENCEEND SENTENCESTART  During the surgery, my husband and I just walked around aimlessly for hours and cried SENTENCEEND SENTENCESTART  Finally they called and told us they were finished SENTENCEEND SENTENCESTART  We went in to see Gabe and he’s speaking words SENTENCEEND SENTENCESTART  He’s speaking regular words SENTENCEEND SENTENCESTART  My husband is so excited that he’s taking a video SENTENCEEND SENTENCESTART  But I’m looking at Gabe and he’s in a fetal position on the table SENTENCEEND SENTENCESTART  And I remember thinking that the way he was lying there, he looked like he did when he was born SENTENCEEND SENTENCESTART  It was just a bigger version of baby Gabriel SENTENCEEND SENTENCESTART  He had been such a healthy, beautiful baby boy SENTENCEEND SENTENCESTART  And here he is again SENTENCEEND SENTENCESTART  And he’s not well SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART (2/6) “I didn’t tell my husband right away SENTENCEEND SENTENCESTART  I just told him to come meet me at the park, and that’s where I told him SENTENCEEND SENTENCESTART  The whole time Gabe was playing nearby SENTENCEEND SENTENCESTART  My husband took it very hard SENTENCEEND SENTENCESTART  He started crying SENTENCEEND SENTENCESTART  He had a panic attack SENTENCEEND SENTENCESTART  Our lives had not been easy SENTENCEEND SENTENCESTART  It was very difficult for us in Albania SENTENCEEND SENTENCESTART  My husband grew up without a father SENTENCEEND SENTENCESTART  We decided to come to America alone as teenagers SENTENCEEND SENTENCESTART  Neither of us spoke any English SENTENCEEND SENTENCESTART  We had no family here SENTENCEEND SENTENCESTART  It was very lonely SENTENCEEND SENTENCESTART  We came from nothing SENTENCEEND SENTENCESTART  We worked very hard and we went to school at night and we taught ourselves English SENTENCEEND SENTENCESTART  My husband got a job as a steam worker and I got a job in marketing SENTENCEEND SENTENCESTART  We bought a beautiful, sunny one-bedroom apartment SENTENCEEND SENTENCESTART  We had recently paid off the mortgage SENTENCEEND SENTENCESTART  We could even afford to send Gabe to private school SENTENCEEND SENTENCESTART  It felt like we were evolving SENTENCEEND SENTENCESTART  We felt like we had finally made it past the hard times SENTENCEEND SENTENCESTART  Then the rug was pulled out from under us and everything crumbled SENTENCEEND SENTENCESTART  And I didn’t know what to pick up first SENTENCEEND SENTENCESTART  Do I comfort my son, who’s about to go through the worst journey of his life? Or my husband? Or myself?” NEXTPOST SENTENCESTART (1/6) "Gabe was a perfectly healthy boy SENTENCEEND SENTENCESTART  He’d reached all his milestones as a child SENTENCEEND SENTENCESTART  He talked early SENTENCEEND SENTENCESTART  He walked early SENTENCEEND SENTENCESTART  He never got sick except for colds SENTENCEEND SENTENCESTART  He did baseball and swimming and kickboxing SENTENCEEND SENTENCESTART  Then two years ago he began to have a ‘pins and needles’ feeling in his mouth SENTENCEEND SENTENCESTART  Then it grew numb and he had trouble talking SENTENCEEND SENTENCESTART  One day the teacher had him read out loud in class and he drooled all over the paper SENTENCEEND SENTENCESTART  So I raised a flag with the pediatrician SENTENCEEND SENTENCESTART  He thought it was just an allergy, but sent us to a neurologist just in case SENTENCEEND SENTENCESTART  The neurologist thought it was just a ‘tick’ and part of a growing phase SENTENCEEND SENTENCESTART  But he did an MRI just in case SENTENCEEND SENTENCESTART  When the results came in, he asked Gabe to wait outside the room SENTENCEEND SENTENCESTART  That’s when I became scared out of my mind SENTENCEEND SENTENCESTART  It was the worst possible news SENTENCEEND SENTENCESTART  The doctor said it was a tumor the size of a big olive SENTENCEEND SENTENCESTART  In the brain SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART “All doctors have those patients who sit on our shoulder SENTENCEEND SENTENCESTART  Their image is always with you SENTENCEEND SENTENCESTART  One kid will pop into your head every time you hit a wall SEPARATOR  when you encounter a disease that is so unrelenting that you’ve exhausted all therapies and you’re still not even close SENTENCEEND SENTENCESTART  One memory will keep you going SENTENCEEND SENTENCESTART  It’s a different kid for every doctor SENTENCEEND SENTENCESTART  It’s hard to know why they stick with us SENTENCEEND SENTENCESTART  I remember one patient that had red hair just like my son SENTENCEEND SENTENCESTART  And I remember one five-year-old girl who made me laugh, because when I asked her how she was doing, she told me: ‘I don’t know SENTENCEEND SENTENCESTART  You’re the doctor SENTENCEEND SENTENCESTART ’ And then there was the boy early in my career who was born without an immune system SENTENCEEND SENTENCESTART  He’d already lost two older siblings to the same disease SENTENCEEND SENTENCESTART  He lived the first two years of his life in an isolation room with no windows, and his entire exposure to the world was through a black-and-white TV SENTENCEEND SENTENCESTART  We gave him a bone marrow transplant, and suddenly his immune system came online SENTENCEEND SENTENCESTART  And we took him for a walk in the garden SENTENCEEND SENTENCESTART  This boy who had spent his entire life in a windowless room SENTENCEEND SENTENCESTART  And a sparrow landed on a bush, and he pointed at it, and said: ‘Bird SENTENCEEND SENTENCESTART ’ That moment will always be with me SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART "We just came from Model UN SENTENCEEND SENTENCESTART   We were representing Russia, but we didn\'t win any awards SENTENCEEND SENTENCESTART " NEWLINE "What was your biggest accomplishment of the day?" NEWLINE "Annoying Chad and Bolivia SENTENCEEND SENTENCESTART   We were on a committee with them, but they wouldn\'t let us have any input, so we voted against our own resolution SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART “I knew I was spoiled growing up, and that we’d always had money, but I just assumed that my dad made all his money from driving a bread truck SENTENCEEND SENTENCESTART   Then a few years ago, I was at my cousin’s wedding in the Bahamas, and some of my cousins got drunk and told me that my father was a huge coke dealer SENTENCEEND SENTENCESTART   Then my sister started laughing, and said: ‘You never figured it out?’  Turns out that I was the only one in the family who didn’t know SENTENCEEND SENTENCESTART   I didn’t handle it very well SENTENCEEND SENTENCESTART   I broke down sobbing, then called my dad and started demanding money SENTENCEEND SENTENCESTART   Then I didn’t speak to him for three months SENTENCEEND SENTENCESTART   I don’t even think I really cared about the coke SENTENCEEND SENTENCESTART   I was more embarrassed that I’d been so naive SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART “It’s a mind fuck SENTENCEEND SENTENCESTART  You go to audition after audition, and there are one thousand more ‘no’s’ than ‘yes’s SENTENCEEND SENTENCESTART ’  And you try to find that one little thing that you can change that will make all the difference SENTENCEEND SENTENCESTART   ‘Maybe if I lose 5 more lbs SENTENCEEND SENTENCESTART ’ Or ‘Maybe if I had gone to that school SENTENCEEND SENTENCESTART ’  Or ‘Maybe if I had worked on the lines for 30 more minutes SENTENCEEND SENTENCESTART ’  And it’s hard to step back and realize that it’s not even personal SENTENCEEND SENTENCESTART   It wasn’t about your talent SENTENCEEND SENTENCESTART   It’s not that you’re bad or you’re good SENTENCEEND SENTENCESTART   Most likely, the casting director already had a person in their head they were looking for—and you weren’t it SENTENCEEND SENTENCESTART   Or even worse, the role had already been filled, and they were just holding auditions to follow protocol SENTENCEEND SENTENCESTART   Even when you get chosen for a role, success is so fickle and fleeting SENTENCEEND SENTENCESTART   A gig today doesn’t mean a gig tomorrow SENTENCEEND SENTENCESTART   Unless you’re Brad Pitt or Will Smith, and you can make your own demands, you’re always going to be waiting for the approval of someone else SENTENCEEND SENTENCESTART   In order to stay sane, you’ve got to find other things or people in your life that bring you value SENTENCEEND SENTENCESTART   You can’t just be that weird actor person SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART “I had this feeling in acting school SEPARATOR  that once we got out of the contained environment of school, and into the real world, everything would work itself out SENTENCEEND SENTENCESTART   I thought that the people with talent would naturally rise to the top SENTENCEEND SENTENCESTART   But you learn that the entertainment industry is also a very contained environment SENTENCEEND SENTENCESTART   You learn how little talent actually matters SENTENCEEND SENTENCESTART   So much relies on things that are outside of your control: what you look like, who you know, what agency represents you SENTENCEEND SENTENCESTART   And I knew that race would matter, but I didn’t realize how much SENTENCEEND SENTENCESTART   I just thought my school wasn’t doing plays that catered to ethnicities because we didn’t have many people of color SENTENCEEND SENTENCESTART   I thought in the real world, I’d have more roles to choose from SENTENCEEND SENTENCESTART   But things didn’t change much in that respect, either SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART "I want to be a ballerina SENTENCEEND SENTENCESTART " NEWLINE "What\'s the best part about being a ballerina?" NEWLINE "Dancing SENTENCEEND SENTENCESTART " NEWLINE "What\'s the hardest part about being a ballerina?" NEWLINE "Dancing in front of people SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART “I’m proud to say that my kids’ friends invite me to their parties, because I never judge SENTENCEEND SENTENCESTART   I sat down my kids early and told them: ‘It’s OK to get stoned SENTENCEEND SENTENCESTART   Just don’t be a stoner SENTENCEEND SENTENCESTART   Because stoners are boring SENTENCEEND SENTENCESTART ’  And I told them to talk to me first before they smoke, because I’ll get them the good pot SENTENCEEND SENTENCESTART   And now that they’re older, they get me even better pot!” NEXTPOST SENTENCESTART “I retired six months ago SENTENCEEND SENTENCESTART   I moved from five acres in Texas to a small apartment in Harlem, and I just love it SENTENCEEND SENTENCESTART   I can do whatever I want, all day long SENTENCEEND SENTENCESTART   This morning I explored the Garment District SENTENCEEND SENTENCESTART   Right now I’m going home to eat some chicken and waffles with my neighbor SENTENCEEND SENTENCESTART   Tonight I’ll probably smoke some pot SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART "I\'m new to my job, I\'m new to the city, and I just had my first big cry SENTENCEEND SENTENCESTART   It wasn\'t about anything in particular SENTENCEEND SENTENCESTART   But I needed it, it\'s done, and I feel a lot better SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART “I feel like I’m on autopilot every day SENTENCEEND SENTENCESTART   I go to work, go home, listen to some music, smoke my blunt, and go to sleep SENTENCEEND SENTENCESTART   And that’s a scary place to be SENTENCEEND SENTENCESTART   Cause I’ve got dreams SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART "I\'m creating a screenplay from a novel I wrote about making a film SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I came to study design SENTENCEEND SENTENCESTART   The Americans in my class are very good at speaking English SENTENCEEND SENTENCESTART   So they talk, talk talk SENTENCEEND SENTENCESTART   I\'m not very good with my English SENTENCEEND SENTENCESTART   So I just learn, learn, learn SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "My dad passed away two weeks ago SENTENCEEND SENTENCESTART   I couldn\'t go home because I\'m still working on my paperwork SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART “Many years ago I had a martial arts master from Korea SENTENCEEND SENTENCESTART   It was just like in the movies—he’d make me do crazy things like carry buckets of water up flights of stairs SENTENCEEND SENTENCESTART   He was very unaccepting of apathy SENTENCEEND SENTENCESTART   I was young at the time, so when he’d asked me a question, I’d often answer by shrugging, or saying ‘I don’t know’ or ‘I guess SENTENCEEND SENTENCESTART ’  Then one day, he told me: ‘You never give me straight answer SENTENCEEND SENTENCESTART   Your life is too complicate SENTENCEEND SENTENCESTART   I make simple for you SENTENCEEND SENTENCESTART   Do you want to be weak or strong?’ NEWLINE I said, ‘Strong SENTENCEEND SENTENCESTART ’ NEWLINE ‘Do you want to be slow or fast?’ NEWLINE ‘Fast SENTENCEEND SENTENCESTART ’ NEWLINE ‘Do you want to be smart or stupid?’ NEWLINE ‘Smart SENTENCEEND SENTENCESTART ’ NEWLINE ‘Do you want to be alive or dead?’ NEWLINE ‘Alive SENTENCEEND SENTENCESTART ’ NEWLINE ‘You shrug one more time, you’ll want to be dead SENTENCEEND SENTENCESTART ’” NEXTPOST SENTENCESTART "I only wear my own dresses SENTENCEEND SENTENCESTART   Whenever I sit, I knit SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "No regrets SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART “My father has a lot of flaws SENTENCEEND SENTENCESTART   He showed me both the father that I want to be, and the father that I don’t want to be SENTENCEEND SENTENCESTART   There were times when it all got to be too much for him and he couldn’t handle it SENTENCEEND SENTENCESTART   But he was always the one that pushed me SENTENCEEND SENTENCESTART   He decided at an early age that I was going to run, and he signed me up for races SENTENCEEND SENTENCESTART   My first race was a pee wee race when I was two years old SENTENCEEND SENTENCESTART   The crowd made me so nervous that I laid down in the middle of the race and started crying SENTENCEEND SENTENCESTART   At the age of three, he took me to an ice skating rink, hung my crutches on the wall, and pushed me around on a skate SENTENCEEND SENTENCESTART   When I got older, we ran a 5k together every year on Father’s Day SENTENCEEND SENTENCESTART   Because of my disabilities, it made other members of my family nervous that my father pushed me so hard SENTENCEEND SENTENCESTART   But he taught me persistence and he taught me survival SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART Big news SENTENCEEND SENTENCESTART   We’ve got our first 100% confirmed HONY romance!  I photographed Alex about eight months ago while he was rollerblading through midtown SENTENCEEND SENTENCESTART   Apparently, after seeing the post on HONY, Jordan sent him a Facebook message saying: "I can’t even rollerblade on two legs SENTENCEEND SENTENCESTART "  Intrigued by Jordan’s eloquence, Alex arranged a meeting and they spent a day together SENTENCEEND SENTENCESTART   They’ve been dating ever since SENTENCEEND SENTENCESTART   I was lucky enough to run into them on the street a few days ago, and they told me the entire story SENTENCEEND SENTENCESTART   We were even close enough to our original meeting spot to go back and recreate the original photograph SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Seeing The Future NEXTPOST SENTENCESTART "We need to improve his dishwashing just a bit SENTENCEEND SENTENCESTART   I\'d say for every ten times that I wash the dishes, he does them one-and-a-half times SENTENCEEND SENTENCESTART  Once completely on his own SENTENCEEND SENTENCESTART  And once where he does the rinsing SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART “A lot of people have a hard time comprehending the concept of a woman who doesn’t want kids SENTENCEEND SENTENCESTART   I’m constantly being told that I still have time to change my mind SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART "She\'s the reason I don\'t get high anymore SENTENCEEND SENTENCESTART   Well, one of the reasons SENTENCEEND SENTENCESTART   The other reason is that I didn\'t want to get high anymore SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I\'m a banker SENTENCEEND SENTENCESTART " NEWLINE "What was your greatest moment of banking glory?" NEWLINE "I discovered a former dictator was laundering money through our bank SENTENCEEND SENTENCESTART   So I got him kicked him out SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I haven\'t felt guilt since I last had a belief system SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "Her mother and I were going through a dark time when we had her, so we named her Sunshine SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I proved that every hypercube of suitable dimension can be edge decomposed by copies of an arbitrary tree SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "We\'d been having a sort of tacit conversation about it for a couple years SENTENCEEND SENTENCESTART   Then one day, his sister, who already knew, was teasing him about having a crush on a boy at school SENTENCEEND SENTENCESTART   And I heard him say: \'Well, maybe it\'s true!\'  So I said: \'Son, we\'ve never really talked about this SENTENCEEND SENTENCESTART   Are you gay?\'  And even though he was 6\'4", he came over to me, curled up in my lap and just sobbed and sobbed SENTENCEEND SENTENCESTART   It was one of the most beautiful moments of my life, actually SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I\'m not much of a public person SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I\'m studying art history SENTENCEEND SENTENCESTART   Though I like the art much more than the history SENTENCEEND SENTENCESTART   Especially contemporary art SENTENCEEND SENTENCESTART " NEWLINE "Any particular kind of contemporary art?" NEWLINE "Anything that uses light SENTENCEEND SENTENCESTART " NEWLINE "Why light?" NEWLINE "A lot of artists use light and mirrors to create a space that is real, but not physical SENTENCEEND SENTENCESTART   It\'s sort of like a dream SENTENCEEND SENTENCESTART   When you dream, you\'re in a space, but it has no physical properties SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I wish I\'d partied a little less SENTENCEEND SENTENCESTART   People always say \'be true to yourself SENTENCEEND SENTENCESTART \'  But that\'s misleading, because there are two selves SENTENCEEND SENTENCESTART   There\'s your short term self, and there\'s your long term self SENTENCEEND SENTENCESTART   And if you\'re only true to your short term self, your long term self slowly decays SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I\'ve got some woman issues SENTENCEEND SENTENCESTART   I\'m trying to get them out SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "Before the chemo, I had this long, beautiful hair that everyone was always commenting on SENTENCEEND SENTENCESTART   But when we first met, he walked up to me and said: \'Has anyone ever told you that you have the most beautiful smile?\'" NEXTPOST SENTENCESTART "A coworker asked for my number the other day SENTENCEEND SENTENCESTART   My friends overheard and said: \'He must have a thing for Indians SENTENCEEND SENTENCESTART \'  I was like, \'Or maybe I\'m just really fucking cool SENTENCEEND SENTENCESTART \'" NEXTPOST SENTENCESTART "Do you mind if I take your photograph?  I run a popular website called SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART " NEWLINE "I don\'t give a damn!" NEXTPOST SENTENCESTART "If you could give one piece of advice to other kids, what would it be?" NEWLINE "God is real and he can do miracles SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I\'m taking a puddlegram SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "The army stationed me down South when I was younger, and I couldn\'t even use the same bathroom as white people SENTENCEEND SENTENCESTART   But things have changed so much SENTENCEEND SENTENCESTART   The younger generation isn\'t nearly as racist SENTENCEEND SENTENCESTART   I\'ve been sitting here for fifty years SENTENCEEND SENTENCESTART   So much has changed SENTENCEEND SENTENCESTART   This neighborhood used to be all black SENTENCEEND SENTENCESTART   A white person couldn\'t even walk down this street SENTENCEEND SENTENCESTART   All the races kept to themselves SENTENCEEND SENTENCESTART   Now you\'ve got Indians talking to Pakistanis, blacks talking to whites, everybody is here and learning from each other\'s cultures SENTENCEEND SENTENCESTART   I\'ve been sitting here for 50 years SENTENCEEND SENTENCESTART   Things are getting better SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I\'ve climbed over 100 mountains SENTENCEEND SENTENCESTART   For some reason, I just like to get on top of things SENTENCEEND SENTENCESTART " NEWLINE "Why do you think that is?" NEWLINE "I think it\'s because climbing forces me to rely on myself SENTENCEEND SENTENCESTART   In the city, anything I need is a phone call away SENTENCEEND SENTENCESTART   When I\'m climbing, I\'ve got to figure it out on my own SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "With all the good it\'s brought, technology is destroying our communication skills SEPARATOR  especially the ability to listen SENTENCEEND SENTENCESTART  The older generation can still listen, but many of the youngsters can\'t even look you in the eye while you speak SENTENCEEND SENTENCESTART  If they aren\'t looking at their mobiles, they\'re looking over your shoulder or glancing around the room SENTENCEEND SENTENCESTART " NEWLINE "Why is verbal communication more important than communication through a device?" NEWLINE "Because there\'s only so much you can learn from your Facebook friends SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "If you were to give one piece of advice to a large group of people, what would it be?" NEWLINE "Well, if you\'d asked me a week ago, I\'d say: \'pay it forward SENTENCEEND SENTENCESTART \'  Now I\'d say \'be careful SENTENCEEND SENTENCESTART \'" NEXTPOST SENTENCESTART "I used to be 260 lbs SENTENCEEND SENTENCESTART " NEWLINE "What triggered the change?" NEWLINE "I think it just came from me being a competitive guy SENTENCEEND SENTENCESTART   My brother used to tease me and call me \'fat boy SENTENCEEND SENTENCESTART \' So I was like, \'Fuck you SENTENCEEND SENTENCESTART   I\'m not fat SENTENCEEND SENTENCESTART   I\'m a skinny guy in a fat body SENTENCEEND SENTENCESTART \'" NEXTPOST SENTENCESTART I asked what he wanted to be when he grew up SENTENCEEND SENTENCESTART    NEWLINE He screamed: "A benny!"  NEWLINE "What\'s a benny?" I asked SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE "That\'s his name," said his mom SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART She was collecting rocks SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Do you remember the happiest moment of your life?" NEWLINE "Impossible to pick one moment SENTENCEEND SENTENCESTART " NEWLINE "Do you remember the saddest moment of your life?" NEWLINE "Impossible to pick one moment SENTENCEEND SENTENCESTART " NEWLINE "Do you remember the time you felt most afraid?" NEWLINE "When my ex-girlfriend\'s brother tried to kill me SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I just got my eyes dilated, so you look like a huge blob right now SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "You photograph normal people on the street?  I go to photography museums, so trust me, if you want to be successful, you must take crazier photos than this SENTENCEEND SENTENCESTART   Try photos with naked people SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART I asked for her portrait, and she struck a pose SENTENCEEND SENTENCESTART   Then I said: "Ok, now let\'s do a regular one SENTENCEEND SENTENCESTART "  So she struck another pose SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "The happiest moment of my life was when my daughter was born SENTENCEEND SENTENCESTART   The second happiest moment was when I made this jacket SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART The HONY Book just quietly spent its 18th consecutive week on the NYT bestseller list SENTENCEEND SENTENCESTART   This feat was achieved by clinging to the bottom of the list like a frightened barnacle, while the whales battle it out for the top spots SENTENCEEND SENTENCESTART   Anyway, it\'s been several weeks since I mentioned the book on the blog SENTENCEEND SENTENCESTART   (Hope you noticed, I try SENTENCEEND SENTENCESTART )  So for all you newcomers: there\'s a HONY book SENTENCEEND SENTENCESTART   It\'s freaking awesome SENTENCEEND SENTENCESTART   And it\'s been dominating the absolute bottom of the bestseller list SENTENCEEND SENTENCESTART   Like a barnacle SENTENCEEND SENTENCESTART    NEWLINE  NEWLINE AMAZON: http://amzn SENTENCEEND SENTENCESTART to/1gy0gQX NEWLINE BARNES AND NOBLE: http://bit SENTENCEEND SENTENCESTART ly/18Namem NEWLINE INDIEBOUND: http://bit SENTENCEEND SENTENCESTART ly/1nWvwbk NEXTPOST SENTENCESTART "We\'re gay refugees from Iran SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "What\'s your greatest fear?" NEWLINE "Getting sliced in half but still being alive SENTENCEEND SENTENCESTART   I saw that on the internet once SENTENCEEND SENTENCESTART   It looks awful SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "Where do you want to be in ten years?"\r NEWLINE "I just want to be happy SENTENCEEND SENTENCESTART "\r NEWLINE "Well, are you happy now?"\r NEWLINE " SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART happy-ish SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I want to play in the NFL SENTENCEEND SENTENCESTART "\r NEWLINE "What\'s your second choice?"\r NEWLINE "Archaeologist SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART I was transporting Susie The Dog across town, when this man walked up and started admiring her SENTENCEEND SENTENCESTART   It quickly became apparent that he really, really loved dogs SENTENCEEND SENTENCESTART   "I\'ve trained them all my life," he explained SENTENCEEND SENTENCESTART   Soon he was getting emotional SENTENCEEND SENTENCESTART   "All you do is feed them," he said, "and they love you so much for it SENTENCEEND SENTENCESTART   I still cry for my first dog SENTENCEEND SENTENCESTART   She would not eat unless I was eating SENTENCEEND SENTENCESTART   I once had a fever for three weeks, and she would not eat because I couldn\'t eat SENTENCEEND SENTENCESTART "  He started to tear up a bit SENTENCEEND SENTENCESTART   "She ended up getting stolen," he said SENTENCEEND SENTENCESTART   "When you lose a dog, it\'s just as bad as losing a human SENTENCEEND SENTENCESTART   They may be less intelligent but the emotional connection is the same SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "Like the prophecy says SEPARATOR  if you have love and goodness in your heart, you must share it SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "I\'m in the middle of a spiritual process SENTENCEEND SENTENCESTART   I can\'t wear color for another six months SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Got to hang out with the second group of fundraiser winners at Tumblr HQ on Wednesday SENTENCEEND SENTENCESTART   Again, an extremely intelligent group of young people SENTENCEEND SENTENCESTART    Not only did David extend the tour an extra hour, but to set a good example, the staff of Tumblr confined their after-work "Happy Hour" to a single conference room SENTENCEEND SENTENCESTART   It was a humorous sight to see 40 people with dixie cups packed into a single meeting room, while 15 young adults roamed around the office SENTENCEEND SENTENCESTART \r NEWLINE \r NEWLINE Thanks Tumblr! NEXTPOST SENTENCESTART Ironically spent the entire Calvin Klein show trying to get a sweet pic of Bill Cunningham SENTENCEEND SENTENCESTART   He\'s been photographing for decades, yet still giggles with delight when he snaps a good shot SENTENCEEND SENTENCESTART   What a great soul SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Seen on the subway SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Seen in the subway SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Found this complete original at Milk Studios SENTENCEEND SENTENCESTART   He looks like a permanent installation, but he was just passing through SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "I\'m hanging it in my son\'s room SENTENCEEND SENTENCESTART " NEWLINE "How old is he?" NEWLINE "He hasn\'t been born yet SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "There\'s a naked guy in the next car!" *\r NEWLINE \r NEWLINE *Confirmed NEXTPOST SENTENCESTART Seen in Chinatown NEXTPOST SENTENCESTART Seen at Lincoln Center NEXTPOST SENTENCESTART Even the cat thinks this lady likes cats a little too much SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Seen in Chinatown NEXTPOST SENTENCESTART Seen in Central Park NEXTPOST SENTENCESTART This guy looked like a champ SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Is this for Fashion Week?"\r NEWLINE "Nah, I just got out of jail SENTENCEEND SENTENCESTART   I\'ve been wearing this shit for two weeks SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART The Queen is not pleased SENTENCEEND SENTENCESTART \r NEWLINE Heads will roll SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Seen in Chelsea NEXTPOST SENTENCESTART "My goal is to be the Creative Director of a magazine SENTENCEEND SENTENCESTART   Basically, I want to be Grace Coddington\'s** Mini-Me SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE **Corrected from Grace Cunningham SENTENCEEND SENTENCESTART   That\'s what I get for throwing up a post then running out the door SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "I started a skipping club, and today is our first meeting!" NEXTPOST SENTENCESTART "I used to be a poet SENTENCEEND SENTENCESTART   I made a switch to art, but I wanted to keep working with books SENTENCEEND SENTENCESTART   So now I make sculptures out of them SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART I asked her a few questions, but she completely clammed up SENTENCEEND SENTENCESTART   No matter what I said, she just smiled shyly and stared at the ground SENTENCEEND SENTENCESTART   After a couple minutes of this, I turned to her friend, and asked "Can you tell me something about your friend?"  NEWLINE  NEWLINE "Yeah," she replied SENTENCEEND SENTENCESTART  "She\'s really loud SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Manhattan overrun by demonic vampire rabbits SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Dear HONY, NEWLINE  NEWLINE I wanted to contact you about a Human of New York who passed away this weekend SENTENCEEND SENTENCESTART   Aidan Seeger was a 7 year old Brooklyn boy, son of Bobby and Elisa Seeger, who own Indian Larry Motorcycles in Brooklyn SENTENCEEND SENTENCESTART   Aidan suffered from a rare degenerative brain disease called ALD SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE Mostly this blue eyed boy represented what is best about New York SENTENCEEND SENTENCESTART  His childhood was filled with Coney Island, L&B pizza, trips to Prospect Park to feed the ducks, and italian ices at Uncle Luigi\'s SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE He walked around Brooklyn with all the confidence of a grown man, he charmed strangers and was called the RULER because he ruled NY SENTENCEEND SENTENCESTART   He will be greatly missed SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Thank you for your time, NEWLINE Gina NEXTPOST SENTENCESTART Grab a mop because your heart\'s about to melt SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART I think I just felt jealous of a dog SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The painter heard me laughing, and without looking down, said: "Boogie\'s looking at you sideways, isn\'t he?" NEXTPOST SENTENCESTART This man was sitting next to me on the subway today, asking me very technical questions about my camera SENTENCEEND SENTENCESTART    NEWLINE I said: "You seem to know more about my camera than I do SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE He made this face, and said: "Well, I did invent the world\'s largest x-ray focusing telescope when scientists around the world were claiming it could not be done SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Here\'s a guaranteed smile to help get you through your Wednesday SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART I found this man on 7th Avenue in Park Slope SENTENCEEND SENTENCESTART   He was leaning heavily on his cane, looking down, wearing a grimaced face SENTENCEEND SENTENCESTART   I felt bad for him, so I smiled and waved when I walked past SENTENCEEND SENTENCESTART   His face changed completely SENTENCEEND SENTENCESTART   He lit up, smiled wide, and gave me a cheery greeting SENTENCEEND SENTENCESTART   There was nothing forced about it SENTENCEEND SENTENCESTART   He seemed like a man who went through life looking for the smallest excuses to be happy SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE I walked 50 feet down the sidewalk, turned around, and walked back to him SENTENCEEND SENTENCESTART   "I want to take your photo," I told him, "because of how big you smiled when I walked by SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE He said: "Well I saw someone smiling at me who I didn\'t even know SENTENCEEND SENTENCESTART   So I thought: \'By God!  I Better do something!\'" NEXTPOST SENTENCESTART The Flower Storm NEXTPOST SENTENCESTART Bad news, everyone SENTENCEEND SENTENCESTART   I ran into HONY regular Twink the Puppy today, and he\'s feeling pretty down due to a broken leg SENTENCEEND SENTENCESTART   "Like" this photo if you hope Twink feels better SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Now for the million dollar question SENTENCEEND SENTENCESTART " NEWLINE "What\'s that?" NEWLINE "Did you break the foot while riding the unicycle?" NEWLINE "No, I didn\'t SENTENCEEND SENTENCESTART " NEWLINE "OH MAN, I thought I was going to have a great caption SENTENCEEND SENTENCESTART " NEWLINE "Well, there is good news SENTENCEEND SENTENCESTART " NEWLINE "What\'s that?" NEWLINE "I broke it playing Quidditch SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART These two shots were taken one month apart SENTENCEEND SENTENCESTART  NEWLINE Now that\'s dedication SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART I cannot tell you guys how long I have searched for this man SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "My story?  Well I\'m 90 years old and I ride this thing around everywhere SENTENCEEND SENTENCESTART   I don\'t see why more people don\'t use them SENTENCEEND SENTENCESTART   I carry my cane in the basket, I get all my shopping done, I can go everywhere SENTENCEEND SENTENCESTART   I\'ve never hit anyone and never been hit SENTENCEEND SENTENCESTART   Of course, I ride on the sidewalk, which I don\'t think I\'m supposed to do, but still SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Kinda sad that I\'m 20 years older than this kid, and he looks better walking home from school than I\'ve ever looked in my life SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART A collection of my favorite portraits from Humans of New York SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART This portrait has a Wes Anderson feel to it SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Fur coat in the winter NEWLINE Look like a polar bear NEWLINE Stuntin\' in the snow NEWLINE These haters, they can go somewhere" NEXTPOST SENTENCESTART I love when people bring their own personal flare to a well-trod fashion statement SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART CAPTION CONTEST: We haven\'t had a caption contest in awhile SENTENCEEND SENTENCESTART   Submit your best caption SENTENCEEND SENTENCESTART   "Like" your favorite SENTENCEEND SENTENCESTART   The caption with the most "likes" will go on the main site tomorrow SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "I got kicked out of my house cause their lawyer was bigger than my lawyer SENTENCEEND SENTENCESTART   Cause I don\'t got one SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "You better not get me hit SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Sometimes on a dark, cold day, you find a bright ray of golden sunshine SENTENCEEND SENTENCESTART  NEWLINE And she gives you Chinese candy SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Hold on SENTENCEEND SENTENCESTART   I have one more move to try SENTENCEEND SENTENCESTART " NEWLINE "Oh don\'t worry  SEPARATOR  I\'m not going anywhere SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART He kinda looks like he\'s showing off something he just lassoed SENTENCEEND SENTENCESTART ',
     'NEXTPOST SENTENCESTART Firefly, a new microscope from Harvard\'s Cohen Lab, can show how neurons pulse, communicate, and shine SENTENCEEND SENTENCESTART  The microscope\'s large field of view and fast imaging capability allows it to image electrical signals quickly traveling from neuron to neuron SENTENCEEND SENTENCESTART ⠀ NEWLINE ⠀ NEWLINE The images could illuminate how disorders like epilepsy and Alzheimer’s disease affect neuron communication and thus enable researchers to discover how to prevent and treat a host of neurological diseases SENTENCEEND SENTENCESTART ⠀ NEWLINE ⠀ NEWLINE Here, each bright dot represents one neuron in the brain of a genetically modified mouse SENTENCEEND SENTENCESTART ⠀ NEWLINE ⠀ NEWLINE Credit: Vaibhav Joshi NEXTPOST SENTENCESTART At Harvard Business School\'s "Crossover into Business" program, athletes from the WNBA, NBA, and NFL worked with their M SENTENCEEND SENTENCESTART B SENTENCEEND SENTENCESTART A SENTENCEEND SENTENCESTART  student-mentors discussing the "LeBron James" case and endorsement opportunities SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART 73°F in 360°! NEXTPOST SENTENCESTART Inside the Harvard Semitic Museum, a new exhibit blends important artifacts with immersive technology SENTENCEEND SENTENCESTART  Take a look! NEXTPOST SENTENCESTART Beautiful weather has arrived! David Chang \'17 "rescues" Waverly He \'18 from being holed up inside the Northwest Building writing her thesis on an unseasonably warm February day SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Oh, and that\'s a 21-foot killer whale skeleton in the background SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Photo: Rose Lincoln/Harvard Staff Photographer NEXTPOST SENTENCESTART If you see this little robot scurrying down the hall at Harvard, try not to squish it SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Happy Presidents\' Day! Did you know there are eight U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  presidents with Harvard degrees? NEXTPOST SENTENCESTART Thanks to a breakthrough gift from Susan Shallcross Swartz and her husband, James R SENTENCEEND SENTENCESTART  Swartz ’64, Andover Hall will undergo a renewal, its first since construction more than 100 years ago SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Department chair Daniel Lord Smail lauded the Dakota descendant as “hands-down the leading authority in Native American history SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Your physician may have less of a role in day-to-day well-being than the facilities manager where you work SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Solange Knowles will be recognized at Harvard\'s annual award ceremony on March 3 SENTENCEEND SENTENCESTART  Congratulations! NEXTPOST SENTENCESTART Master potter Ben Owen III demonstrated his throwing process and discussed his work at a recent workshop at the Harvard Ceramics Building in Allston SENTENCEEND SENTENCESTART  http://hrvd SENTENCEEND SENTENCESTART me/maste6cc8 NEXTPOST SENTENCESTART Has this flu season peaked? A panel of experts discussed the flu, vaccination, and future research SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The papers of famed activist Angela Davis are now part of Radcliffe’s Schlesinger Library SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART When Harvard student Kaitlyn was in middle school, her mother made this Valentine\'s Day sweater out of a dish towel! She\'s worn it every Valentine\'s Day since SENTENCEEND SENTENCESTART  ❤️ NEWLINE  NEWLINE Photo: Rose Lincoln/Harvard Staff Photographer NEXTPOST SENTENCESTART These Harvard professors study the science of love SENTENCEEND SENTENCESTART  Happy Valentine\'s Day! NEXTPOST SENTENCESTART A record 42,742 students applied for admission to Harvard’s Class of 2022, breaking last year’s record of 39,506 for the current freshman class SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART What you need to know about the flu - experts discuss the flu season, prevention, and treatment SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Here\'s what our community — from deans to faculty to students — said about Harvard\'s 29th president SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Ryan Donato \'19 is part of the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  Olympic Men’s Ice Hockey Team competing in South Korea SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART “I really see this as an opportunity to not just serve Harvard, but at this particular moment in time, to serve higher education SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART Today, we announce Harvard\'s 29th president! NEXTPOST SENTENCESTART On their quest to restore hearing through gene therapy, scientists have long sought ways to improve gene delivery into hair cells SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The free, online HarvardX course “Super-Earths and Life” uses adaptive learning to explore how the technology can improve online learning outcomes SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART See Harvard through this collection of double exposure images, where iconic elements of campus overlap and converge in surprising ways SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART He is already known as one of Hollywood’s most versatile screen stars, but Ryan Reynolds earned an offbeat claim to fame with his visit to Harvard to collect the Hasty Pudding Theatricals’ 2017 Man of the Year award SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Learn how to spot fake news and connect with reputable resources with this guide from the Harvard Library: http://hvrd SENTENCEEND SENTENCESTART me/qHoO308EtQ7 NEXTPOST SENTENCESTART To commemorate one of America’s oldest continued partnerships, the Harvard Library curated “To Serve Better Thy Country,” an exhibit of the interwoven histories of the two storied institutions, assembling letters, photographs, and objects that show Harvard affiliates’ tradition of service from the earliest years of our country to today SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Top reporters and editors discuss the future of news, as well as the opportunities and the challenges the industry faces in what many observers call the “post-truth” era SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Paola Villarreal, a fellow at the Berkman Klein Center for Internet & Society at Harvard University is using data visualization to shed light on inequality in health, housing, and more SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART In response to travel restrictions to the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  from seven Muslim-majority nations, Harvard leaders issued strong statements supporting the University’s international community SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART On Franklin D SENTENCEEND SENTENCESTART  Roosevelt\'s birthday, look around the FDR Suite in Adams House, where the 32nd president lived while at Harvard from 1900-1904 SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART “Benefiting from the talents and energy, the knowledge and ideas of people from nations around the globe is not just a vital interest of the University; it long has been, and it fully remains, a vital interest of our nation SENTENCEEND SENTENCESTART ” Read President Faust\'s letter to the Harvard community SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART For all her acting success, Spencer said her dream role “is that of a producer, a woman behind the scenes who creates roles for diversity in film SENTENCEEND SENTENCESTART  When I say diversity, I mean I want to see women of all shapes and sizes, people of all shapes and sizes SENTENCEEND SENTENCESTART  It’s about creating a landscape that demonstrates what our society is as a whole SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART “I think that the real power of the trans movement is that it is so multifocal, it can be so many things SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART Makeda Best has been named the new Robert L SENTENCEEND SENTENCESTART  Menschel Curator of Photography at the Harvard Art Museums SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Nearly a century after it was theorized, Harvard scientists have succeeded in creating metallic hydrogen SENTENCEEND SENTENCESTART  “This is the Holy Grail of high-pressure physics,” Professor Isaac Silvera said of the quest to find the material SENTENCEEND SENTENCESTART  Read more: http://hvrd SENTENCEEND SENTENCESTART me/APEk308qvcW NEXTPOST SENTENCESTART Harvard scientists have created the rarest material on the planet, which could become its most valuable SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART What does big data say about Broadway? NEXTPOST SENTENCESTART Kelvin Apari (\'20), left, throws a ball on the bowling ice lane placed in The Plaza, while Max Mangum (\'20), right, looks at him SENTENCEEND SENTENCESTART  For the Plaza WinterFest 2017 the Harvard Common Spaces has turned The Plaza into a place of meeting and playing despite the cold weather SENTENCEEND SENTENCESTART  From January 20th to March 10th students can enjoy the ice lanes featuring curling, shuffleboard, and bowling SENTENCEEND SENTENCESTART  The Plaza hosts seating around fire pits, games like ping pong, foosball, cornhole, and some food trucks SENTENCEEND SENTENCESTART  Photo by Silvia Mazzocchin NEXTPOST SENTENCESTART Members of the Harvard community served in local military units for almost 150 years before the nation’s founding SENTENCEEND SENTENCESTART  An exhibition at Pusey Library highlights the connections SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The Hasty Pudding Theatricals, the oldest theatrical organization in the United States, has named Oscar-winning actress Octavia Spencer as its Woman of the Year SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART A new study shows that advances in pediatric brain cancer may be turning a corner thanks to precision medicine SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "La La Land" composer Justin Hurwitz met his Hollywood collaborator, Damien Chazelle, and charted his musical path while at Harvard SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The Wyss Institute’s human gut-on-a-chip technology is used to co-culture gut microbiome and human intestinal cells, which could spur innovation of novel therapies for inflammatory bowel diseases SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Probing the mind-set behind terrorism, and the mind-set it inspires SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The need for continuous rigorous and relevant climate science will be more important than ever SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART A decade-long research inside Mexico\'s National Archive revealed "a window into a story of Mexico that stands in stark contrast to the official story SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Harvard T SENTENCEEND SENTENCESTART H SENTENCEEND SENTENCESTART  Chan School of Public Health researcher will direct The Planetary Health Alliance, a new project in partnership with the Wildlife Conservation Society SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Early Action decisions have been announced to applicants for the Class of 2020 SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART What can we learn about the law from that galaxy far, far away? NEXTPOST SENTENCESTART How will you know if your moody teen is hanging out with the right people? Recent research addresses some aspects of this question SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Arguably Harvard\'s most stylish School, the Graduate School of Design is where minimalism intersects with maximum individualism NEXTPOST SENTENCESTART Happy birthday, Grace Hopper! The technology pioneer was responsible for writing what became the world’s first computer programming manual SENTENCEEND SENTENCESTART  While at Harvard, she programmed Mark I, conceived by physics professor Howard Aiken SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Scholars gathered at Harvard’s Observatory of the Spanish Language to ponder how Spanish can continue thriving as the second-most-common language in the United States SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART A flavoring chemical linked to cases of severe respiratory disease was found in more than 75% of flavored e-cigarettes and refill liquids tested by researchers SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Harvard alumni are making an impact through entrepreneurship, board service, and volunteerism NEXTPOST SENTENCESTART 115 years ago today, the Athletic Committee officially recognized the sport of men’s basketball at Harvard SENTENCEEND SENTENCESTART  John Kirkland Clark then became the Crimson’s first captain and coach of the team pictured here SENTENCEEND SENTENCESTART  Harvard’s first intercollegiate game was a 20-10 victory over Holy Cross SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Two Harvard experts discussed climate change at the UN conference in Paris SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART There\'s hope for the fight against diabetes SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Happy birthday, Walt Disney! The famous cartoonist came to campus with some special friends for Commencement in 1938, when he earned an honorary degree SENTENCEEND SENTENCESTART  Photo via Boston Public Library SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART These are a few of their favorite things SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Documentarians made a film that\'s both a grim account of destruction, and a hopeful look toward the future SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART “The United States must lead in this fight, but we need others to go with us; we need others to do more,” said Secretary of Defense Ash Carter SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART It’s twice as costly to a business to have a toxic worker than it is to have a superstar worker, according to a new Harvard Business School paper SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Professor Cass Sunstein was joined by some special guests for his talk, "The World According to Star Wars SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART At Harvard, we are acting on climate change through research that occurs across disciplines and throughout the world SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "We haven’t gotten nearly as serious about the obesity epidemic as we have about smoking SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Just beyond the leading edge of biomedical research lie the medical tools of the future, and Harvard students are being trained to take full advantage of them - http://hvrd SENTENCEEND SENTENCESTART me/BZA4k NEXTPOST SENTENCESTART Seventy percent of Harvard students receive some form of financial aid SENTENCEEND SENTENCESTART  A Harvard education is more affordable than a state school for 90% of American families SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART From above, Harvard’s newest architectural wonder simply sparkles: a gleaming geometric rooftop poking above the trees SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART What made Beyoncé\'s surprise 2013 album release so successful? A new Harvard Business School case study examines how she pulled it off SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART In an op-ed on The Huffington Post, Harvard University President Drew Faust and Stanford University President John L SENTENCEEND SENTENCESTART  Hennessy discuss what universities can do about climate change SENTENCEEND SENTENCESTART  http://hvrd SENTENCEEND SENTENCESTART me/BRYrZ NEXTPOST SENTENCESTART Book covers can be visual invitations to read SENTENCEEND SENTENCESTART  The books in this  "InsideOUT" exhibition evoke wonder as well as curiosity SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The \'Soft Robotics Toolkit\' provides open-source plans and how-to videos to help you design, build, and operate soft robots SENTENCEEND SENTENCESTART  What would you build? NEXTPOST SENTENCESTART Take a look at Harvard\'s iconic towers—and learn how architecture on campus has changed over the years—in this photo gallery: http://hvrd SENTENCEEND SENTENCESTART me/BOspE NEXTPOST SENTENCESTART Could the stars be aligned for Pluto to reassume its place in the galaxy? NEXTPOST SENTENCESTART A giant exoplanet may be causing the star it orbits to act much older than it actually is, according to new data from NASA\'s Chandra X-ray Observatory: http://hvrd SENTENCEEND SENTENCESTART me/BGsyY NEXTPOST SENTENCESTART In shedding light on violence among chimps, a new study also may offer insight in the same tendencies in humans SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Experts examine the rise in parents refusing to vaccinate their children SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "I didn’t even know mathematicians got them, because mathematics isn’t the kind of work in which you need money — or to travel somewhere and be there to do mathematics SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART A new device, called a “biospleen,” exceeded expectations with its ability to cleanse human blood of pathogens SENTENCEEND SENTENCESTART  It may radically transform the way doctors treat sepsis SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Join Sheryl Sandberg ’91, MBA ’95 and the Lean In community for a live discussion and Q&A to kick off Lean In Circles on campus this year SENTENCEEND SENTENCESTART  Tune into the live stream on Thursday, September 18, at 7:30pm ET: http://leanin SENTENCEEND SENTENCESTART org/livestream NEXTPOST SENTENCESTART Congratulations to Harvard mathematics professor Jacob Lurie, named a 2014 MacArthur Fellow! Read more about his work: http://www SENTENCEEND SENTENCESTART macfound SENTENCEEND SENTENCESTART org/fellows/921/ NEWLINE  NEWLINE (Photo courtesy of the John D SENTENCEEND SENTENCESTART  & Catherine T SENTENCEEND SENTENCESTART  MacArthur Foundation) NEXTPOST SENTENCESTART Historian Niall Ferguson discusses what’s at stake as Scotland weighs independence SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The radically uninhibited novel “Ulysses” shocked and bewildered many of its readers, and was banned in the United States for more than a decade SENTENCEEND SENTENCESTART  What made it so revolutionary? NEXTPOST SENTENCESTART In his new book, Adam Tanner discusses the rise of data collection, as well as some of the privacy-enhancing tools that citizens can use to protect themselves SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "But I will never forget the tears on my mother’s face as she hugged me goodbye, and told me to live my dream SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART A $350 million gift to the School of Public Health will ensure that Harvard has the resources to develop innovative solutions to international health problems - http://hvrd SENTENCEEND SENTENCESTART me/Brjct NEXTPOST SENTENCESTART Harvard\'s “shopping week” lets students try out classes before formally registering SENTENCEEND SENTENCESTART  Take a look inside the classrooms: http://hvrd SENTENCEEND SENTENCESTART me/BqaMW NEXTPOST SENTENCESTART New findings suggest there could be ways to target and kill cancer cells without affecting healthy cells SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Harvard scholars mark the 200th birthday of "The Star-Spangled Banner" with thoughts on its legacy and language: http://hvrd SENTENCEEND SENTENCESTART me/Bqb6B NEXTPOST SENTENCESTART Seeing how the Hepatitis C virus builds ion channels could help researchers find new drugs to fight the disease - http://hvrd SENTENCEEND SENTENCESTART me/12GI29i NEXTPOST SENTENCESTART A double rainbow appeared in the sky over Harvard Law School after yesterday\'s storm SENTENCEEND SENTENCESTART  \r NEWLINE \r NEWLINE Via Harvard Law School on Instagram: http://instagram SENTENCEEND SENTENCESTART com/harvardlaw NEXTPOST SENTENCESTART It’s one thing to conduct good science SENTENCEEND SENTENCESTART  It’s another to get people to notice SENTENCEEND SENTENCESTART  http://hvrd SENTENCEEND SENTENCESTART me/13XICMp NEXTPOST SENTENCESTART Congratulations to National Aeronautics and Space Administration - NASA 2013 Astronaut Candidate Class member Jessica U SENTENCEEND SENTENCESTART  Meir, Ph SENTENCEEND SENTENCESTART D SENTENCEEND SENTENCESTART , 35, Assistant Professor at Harvard Medical School - http://hvrd SENTENCEEND SENTENCESTART me/11KFl1l NEXTPOST SENTENCESTART As part of Professor Gonzalo Giribet’s Biology of Invertebrates class, students make closely observed, highly detailed sketches of animals they study in the lab SENTENCEEND SENTENCESTART  http://hvrd SENTENCEEND SENTENCESTART me/13LuKor NEXTPOST SENTENCESTART James E SENTENCEEND SENTENCESTART  Ryan, who will become the new dean of the Harvard Graduate School of Education this fall, discusses the importance of #education SENTENCEEND SENTENCESTART  http://hvrd SENTENCEEND SENTENCESTART me/198Re6g NEXTPOST SENTENCESTART These photo journals offer unique ways to look at Harvard, on campus and around the world - http://hvrd SENTENCEEND SENTENCESTART me/Ngfg5S #photography NEXTPOST SENTENCESTART A nebula 5,500 light-years from Earth is forming stars so rapidly that it is undergoing a “mini-starburst” - http://hvrd SENTENCEEND SENTENCESTART me/14j014B NEXTPOST SENTENCESTART In this video, Fanaye Yirga delivers the Latin Oration at the 2013 Harvard Commencement ceremony - http://hvrd SENTENCEEND SENTENCESTART me/17RXAKh NEXTPOST SENTENCESTART The year in pictures slideshow captures highlights and quiet moments during the 2012-13 academic year at Harvard SENTENCEEND SENTENCESTART   - http://hvrd SENTENCEEND SENTENCESTART me/11X6m08 NEXTPOST SENTENCESTART One Harvard: Becoming a whole that is greater than the sum of its thriving parts - http://hvrd SENTENCEEND SENTENCESTART me/17RWuy3 NEXTPOST SENTENCESTART Meet James E SENTENCEEND SENTENCESTART  Ryan, who will become the new dean of the Harvard Graduate School of Education this fall -   NEWLINE http://hvrd SENTENCEEND SENTENCESTART me/11xTjDz NEXTPOST SENTENCESTART Tony Award-winning director Diane Paulus on innovation and her relationship with Harvard - http://hvrd SENTENCEEND SENTENCESTART me/ZFDkWE NEXTPOST SENTENCESTART In Houghton Library\'s collection, you’ll find a piece of the earliest surviving work by Abraham Lincoln SENTENCEEND SENTENCESTART  It holds mathematics exercises Lincoln wrote in 1825, at the age of 16 SENTENCEEND SENTENCESTART  http://hvrd SENTENCEEND SENTENCESTART me/ZF7ZTL NEXTPOST SENTENCESTART These photo journals offer unique ways to look at Harvard, on campus and around the world - http://hvrd SENTENCEEND SENTENCESTART me/Ngfg5S NEXTPOST SENTENCESTART Diane Paulus, artistic director at the American Repertory Theater, took home the coveted Tony Award for best direction of a musical for her restaging of the 1970s musical “Pippin” http://hvrd SENTENCEEND SENTENCESTART me/12eKKm7 NEXTPOST SENTENCESTART Graduating with dual degrees from Harvard Medical School and Harvard Business School, Benedict Nwachukwu is passionate about orthopedics, management, and global health - http://hvrd SENTENCEEND SENTENCESTART me/19PYFRd NEXTPOST SENTENCESTART HarvardX announces its new edX online course offerings, expected to roll out through this fall - http://hvrd SENTENCEEND SENTENCESTART me/11yZni9 NEXTPOST SENTENCESTART Josh Wortzel’s childhood interest in mice led him to Harvard to explore how to re-create damaged tissue - http://hvrd SENTENCEEND SENTENCESTART me/10VXMzZ NEXTPOST SENTENCESTART After a car crash left Jennifer Cloutier \'13 paralyzed at the age of six, her love of learning brought her to Harvard - http://hvrd SENTENCEEND SENTENCESTART me/18QNp8n NEXTPOST SENTENCESTART Watch "In Praise of Clip-On Ties," the graduate English address by Jon Murad at 2013 Commencement - http://www SENTENCEEND SENTENCESTART youtube SENTENCEEND SENTENCESTART com/watch?v=bdwTBesf2zE NEXTPOST SENTENCESTART Today is National Running Day! Learn how researchers are chasing down a better way to run - http://hvrd SENTENCEEND SENTENCESTART me/138RFd5 NEXTPOST SENTENCESTART Researchers use barium levels in fossil teeth to show how breast-feeding behavior changed among early humans - http://hvrd SENTENCEEND SENTENCESTART me/18Qryhh NEXTPOST SENTENCESTART Read Harvard University President Drew Faust\'s 2013 Commencement speech - http://hvrd SENTENCEEND SENTENCESTART me/15vcHUT NEXTPOST SENTENCESTART “A college is a place that tries to preserve centripetal force in a spinning world,” said Delbanco, the Mendelson Family Chair of American Studies and the Julian Clarence Levi Professor in the Humanities at Columbia University SENTENCEEND SENTENCESTART  “A true college must also be a shelter from the preprofessional crosswinds SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART Harvard\'s Arnold Arboretum in Jamaica Plain, Massachusetts NEXTPOST SENTENCESTART A lecture on Winslow Homer at the Sackler Museum NEXTPOST SENTENCESTART Harvard\'s Arnold Arboretum in Jamaica Plain, Massachusetts NEXTPOST SENTENCESTART Harvard\'s Arnold Arboretum in Jamaica Plain, Massachusetts NEXTPOST SENTENCESTART Harvard\'s Arnold Arboretum in Jamaica Plain, Massachusetts NEXTPOST SENTENCESTART What weighs 16 tons, has 3,049 pipes and took 8 months to tune? Harvard\'s new Fisk organ, Opus 139 SENTENCEEND SENTENCESTART  VIDEO NEXTPOST SENTENCESTART “The significance of bees to agriculture cannot be underestimated,” says Alex Lu, associate professor of environmental exposure biology SENTENCEEND SENTENCESTART  “And it apparently doesn’t take much of the pesticide to affect the bees SENTENCEEND SENTENCESTART  Our experiment included pesticide amounts below what is normally present in the environment SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART Black holes feed on stars http://hvrd SENTENCEEND SENTENCESTART me/I11dP2 NEXTPOST SENTENCESTART Harvard University is home to world class collections in art, science, natural history, archeology, and more SENTENCEEND SENTENCESTART  These objects and artifacts give students the opportunity to experience a physical connection to history SENTENCEEND SENTENCESTART  In this video, Peter Galison joined other faculty members to speak about "Teaching with Collections" at Harvard SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Looking to sharpen your quantitative understanding of the world around you? This course is an introduction to probability as a language and set of tools for understanding statistics, science, risk, and randomness SENTENCEEND SENTENCESTART  The ideas and methods are useful in statistics, science, philosophy, engineering, economics, finance  SEPARATOR  and everyday life NEXTPOST SENTENCESTART “Violence has been in decline for long stretches of time, and today may be the most peaceful era in our species’ history”  SEPARATOR  Professor Steven Pinker SENTENCEEND SENTENCESTART  NEWLINE  NEXTPOST SENTENCESTART A view of flowering trees on Holyoke Street at Harvard University in the spring SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE Stephanie Mitchell/Harvard Staff Photographer NEXTPOST SENTENCESTART Admissions acceptances are sorted and checked SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE Chase/Harvard Staff Photographer NEXTPOST SENTENCESTART Students celebrate the Festival of Colors SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE Kris Snibbe/Harvard Staff Photographer NEXTPOST SENTENCESTART Bringing electricity to remote areas in developing countries is a challenge Harvard graduates Jessica Matthews AB \'10 and Julia Silverman AB \'10 are tackling head on SENTENCEEND SENTENCESTART  As students, they developed sOccket, a soccer SEPARATOR ball SEPARATOR shaped device that harnesses the kinetic energy generated as users kick, dribble, or throw it around SENTENCEEND SENTENCESTART  Once the energy is stored, small electrical devices such as LED lights can be plugged into sOccket SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Letters and email notifications of admission to Harvard College have been sent to 2,032 students SENTENCEEND SENTENCESTART  More than 60 percent of families of students admitted to the Class of 2016 will benefit from an unprecedented $172 million in undergraduate financial aid SENTENCEEND SENTENCESTART  Congratulations to the incoming Harvard College Class of \'16! We look forward to seeing you this fall! NEXTPOST SENTENCESTART The toy was complex, with no fewer than 26 solid moving elements and 48 rotating hinges, but it inspired something simpler SENTENCEEND SENTENCESTART  The result is the buckliball, a hollow silicone sphere SENTENCEEND SENTENCESTART  It has no moving parts, but is fashioned with 24 carefully spaced dimples SENTENCEEND SENTENCESTART  When the air is sucked out of a buckliball with a syringe, the thin ligaments between dimples collapse SENTENCEEND SENTENCESTART  NEWLINE  NEXTPOST SENTENCESTART Planet starship http://hvrd SENTENCEEND SENTENCESTART me/Hdfmac NEXTPOST SENTENCESTART Humans “need to arrive at meaning,” said Kentridge, and what better way than to fall in with an artist “filling sheets of paper with signs and images SENTENCEEND SENTENCESTART ” In trying to capture reality in an image, he said, “the drawing becomes a meeting point” between image and reality SENTENCEEND SENTENCESTART  It becomes meaning, in all its glorious imprecision SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART In a drying Amazon, change looms http://hvrd SENTENCEEND SENTENCESTART me/H7fXud NEXTPOST SENTENCESTART The Harvard EdCast is a weekly series that features a 15-20 minute conversation with thought leaders in the field of education from across the country and around the world SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "With these computer science techniques, however, we can immediately come up with an enormous map which is methodologically very interesting, but which also shows the staggering amount of human occupation over the last 7,000 or 8,000 years SENTENCEEND SENTENCESTART "  SEPARATOR Jason Ur, the John L SENTENCEEND SENTENCESTART  Loeb Associate Professor of the Social Sciences NEXTPOST SENTENCESTART Tour Harvard Yard with any web-enabled phone! This free, self-guided tour features text descriptions at each stop as well as audio, video and images — including pictures from the University archives and exclusive inside views of Harvard buildings today SENTENCEEND SENTENCESTART   NEXTPOST SENTENCESTART Here is a peek into this year\'s exhibit of thesis work by graduating seniors in the Department of Visual and Environmental Studies SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Joseph S SENTENCEEND SENTENCESTART  Nye Jr SENTENCEEND SENTENCESTART , Juliette Kayyem, and R SENTENCEEND SENTENCESTART  Nicholas Burns offer their perspectives on the death of Osama bin Laden and what it means for U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  foreign policy and the future of Islamic nations SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART “When we told our music students, it was absolute joy,” CRLS band director Robert Ponte said SENTENCEEND SENTENCESTART  “They looked like they were going to jump out of their skins SENTENCEEND SENTENCESTART  Wynton is the Michael Jordan of jazz, and they were going to play for him SENTENCEEND SENTENCESTART  You should have seen their faces SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART “Humans are unique in their creation of an institution of war that is designed to organize violence, define its purposes, declare its onset, ratify its conclusion, and establish its rules SENTENCEEND SENTENCESTART  War, like literature, is a distinctively human product SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART “Music is the art of the invisible,” said Wynton Marsalis SENTENCEEND SENTENCESTART  “It gives shape and focus to our innermost inclinations, and can clearly evidence our internal lives with shocking immediacy SENTENCEEND SENTENCESTART "  NEXTPOST SENTENCESTART Eradicating malaria from the planet is a tall order, according to a roundtable discussion on the topic that marked World Malaria Day SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Harvard researchers have found that mindfulness meditators more quickly adjust the brain wave that screens out distraction, which could explain their superior ability to rapidly remember and incorporate new facts SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART At Harvard\'s W SENTENCEEND SENTENCESTART E SENTENCEEND SENTENCESTART B SENTENCEEND SENTENCESTART  Du Bois Institute for African and African American Research, a powerful new exhibition by 96-year-old artist Elizabeth Catlett offers viewers a sense of despair, and hope SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART “This is a great day SENTENCEEND SENTENCESTART  We have been dreaming about this program, and now here you are,” said Drew Faust, who told the students that with a Harvard education comes the responsibility to figure out how to “use that education to have an impact on the world SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART Harvard partnered with the Boston schools to create the half-day program, which was filled with motivational talks and provided a real taste of college life, including lunch at Annenberg Hall and small campus tours led by Harvard students and staff SENTENCEEND SENTENCESTART ',
     'NEXTPOST SENTENCESTART Eric Trump and Charlie Kirk speak at CPAC 2018 SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART During a fight late last year, a family friend told a dispatcher that Nikolas Cruz “bought a gun from Dick’s last week and is now going to pick it up,” according to the 911 call log SENTENCEEND SENTENCESTART  She added Cruz had “bought tons of ammo,” “used a gun against (people) before” and "has put the gun to others [sic] heads in the past SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART President Donald J SENTENCEEND SENTENCESTART  Trump on Thursday floated the possibility of paying teachers a “bonus” to carry concealed firearms, stressing the importance of school security even as he calls for strengthening gun laws in the wake of the Parkland mass shooting SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART BREAKING NEWS: A federal grand jury filed a 32-count indictment Thursday against former Trump campaign chairman Paul Manafort and aide Rick Gates SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Both men are charged with multiple counts of preparing false tax returns, bank fraud, conspiracy to commit bank fraud and failure to disclose income from foreign sources SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Coral Springs Police hold a press conference on last week\'s school shooting in Parkland, Florida SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE (Credit: WFOR) NEXTPOST SENTENCESTART A man eating at Arby\'s rushed to help a Georgia state trooper who was struggling with a suspect SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "“[Video images were] delayed 20 minutes and nobody told us that SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE Police officers responding to last week’s school shooting in Parkland, Florida reportedly thought they were tracking Nikolas Cruz live on surveillance video — but then realized the footage was delayed, tossing roadblocks into the frantic efforts to capture the 19-year-old shooting suspect SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART EXCLUSIVE: The centerpiece of a The New Yorker story on Karen McDougal, who says she had an affair with Donald J SENTENCEEND SENTENCESTART  Trump, is scribbled notes kept by the former Playboy playmate SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE A telltale marking on the entries, reproduced by the magazine, shows that McDougal wrote these pages either during or since the 2016 campaign—relying on memories that were at least a decade old SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Senator Ted Cruz speaks at CPAC 2018 SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The White House Principal Deputy Press Secretary Raj Shah holds a briefing SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART HEARTWARMING: Students arriving at Westlake High School were recently greeted by police officers ready to give them a hug or high five SENTENCEEND SENTENCESTART  The officers wanted to show their support after the recent school shooting in Parkland, Florida SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Fear is not in my vocabulary SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE An 88-year-old British military veteran said all he wanted to do was help the woman being attacked SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART JUST IN: A driver intentionally crashed his car into the emergency room entrance at Connecticut’s Middlesex Hospital on Thursday morning and set himself on fire, officials said SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Marion Maréchal-Le Pen addresses CPAC 2018 SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART A funeral is held for Aaron Feis, the football coach who died after placing himself in front of bullets to protect students from a gunman inside a Florida high school last week SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2EFuAeN  NEWLINE  NEWLINE (Courtesy: http://live SENTENCEEND SENTENCESTART cbglades SENTENCEEND SENTENCESTART com/) NEXTPOST SENTENCESTART Moments ago at CPAC 2018, Vice President Mike Pence slammed the media for its criticisms of his actions at the Olympic Games SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2FlLgWD NEXTPOST SENTENCESTART Vice President Mike Pence addresses CPAC 2018 SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2FlLgWD NEXTPOST SENTENCESTART Attorney General Jeff Sessions and other law enforcement officials hold a news conference to announce a significant law enforcement action SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Moments ago, President Donald J SENTENCEEND SENTENCESTART  Trump tweeted his support for the NRA - National Rifle Association of America SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART NRA - National Rifle Association of America CEO Wayne LaPierre addresses the Florida school shooting at CPAC 2018 SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART WATCH LIVE: The Conservative Political Action Conference (CPAC 2018) is held in Maryland SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART SpaceX launches a Falcon 9 rocket with  PAZ satellite to low-Earth orbit from Space Launch Complex 4 East (SLC-4E) at Vandenberg Air Force Base, California SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The second day of CPAC begins with the Presentation of Colors, Pledge of Allegiance and prayer SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Moments ago, President Donald J SENTENCEEND SENTENCESTART  Trump tweeted about new changes to gun policies in the wake of the Florida school shooting SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2FnSKYU NEXTPOST SENTENCESTART "He and my uncle both believed Acts 17:26 - \'Of one blood, God made all people SENTENCEEND SENTENCESTART \'" NEWLINE  NEWLINE On "Fox & Friends," Dr SENTENCEEND SENTENCESTART  Alveda King talked about the relationship between Rev SENTENCEEND SENTENCESTART  Billy Graham and her uncle the Rev SENTENCEEND SENTENCESTART  Dr SENTENCEEND SENTENCESTART  Martin Luther King Jr SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "How many other FISA warrants and applications were based on other junk science? There\'s plenty of other Fusion GPS\'s out there SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Fox & Friends," Michelle Malkin warned that the government surveillance abuses alleged in the controversial FISA memo could be just the "tip of the iceberg SENTENCEEND SENTENCESTART " http://bit SENTENCEEND SENTENCESTART ly/2GIC3YA NEXTPOST SENTENCESTART "What I do know is: To date, not one bit of evidence that shows any time of coordination by the Trump campaign and Russians to influence the election SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Fox & Friends," Rep SENTENCEEND SENTENCESTART  Jim Jordan reflected on the FISA memo, Jim Comey, and the Russia investigation SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/2EBgQzd NEXTPOST SENTENCESTART Bryan Llenas takes us inside an active shooter situation drill, filled with gunfire and smoke bombs SENTENCEEND SENTENCESTART  See how one small business is taking steps to prepare its employees for worst-case scenarios SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2FEs456 NEXTPOST SENTENCESTART "The Democrats will do anything to keep the truth from coming out SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  This has been a conspiracy to actually reverse the results of the election SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Cavuto Live," Ben Stein discussed the FISA memo and the mainstream media\'s response SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2nymkTR NEXTPOST SENTENCESTART “It was just a huge honor to be at the State of the Union, sitting next to Melania Trump SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Preston Sharp gained national attention last year for starting a campaign to salute deceased veterans across the country with a flag and a flower on their headstones - and he talked about his trip to the State of the Union on "Fox & Friends SENTENCEEND SENTENCESTART " http://bit SENTENCEEND SENTENCESTART ly/2GKF7n6 NEXTPOST SENTENCESTART "It is now the most consequential - no question - political scandal in American history SENTENCEEND SENTENCESTART " – Dan Bongino NEXTPOST SENTENCESTART What does this hippo know that we don\'t? Fiona the Hippo registered her Super Bowl LII pick at the Cincinnati Zoo & Botanical Garden on Thursday SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART In an op-ed entitled "The Founding Fathers Would Have Been Against DACA," student Naweed Tahmas wrote that "support for \'undocumented immigrants\' is a quasi-religion" at UC Berkeley SENTENCEEND SENTENCESTART  The student newspaper rejected the article SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/2DWptHt NEXTPOST SENTENCESTART WATCH: "I think the American people understand that the FBI should not go to secret courts using information that was paid for by the Democrats to open up investigations and get warrants on people of the other political party SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Rep SENTENCEEND SENTENCESTART  Devin Nunes spoke exclusively to Bret Baier Friday, just hours after a memo was released alleging intelligence abuse by the DOJ and FBI during the 2016 Trump campaign SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2nE8LBx NEXTPOST SENTENCESTART "The release of this memo has created a lot of momentum for the release of more stuff SENTENCEEND SENTENCESTART "  NEWLINE  NEWLINE The "Special Report" panel discusses the release of the memo alleging intelligence abuse by the DOJ and the FBI during Donald J SENTENCEEND SENTENCESTART  Trump\'s presidential campaign SENTENCEEND SENTENCESTART   http://fxn SENTENCEEND SENTENCESTART ws/2nE8LBx NEXTPOST SENTENCESTART A distraught father made an emotional apology after charging at disgraced doctor Larry Nassar in a Michigan courtroom Friday SENTENCEEND SENTENCESTART  He finished his statement with a few parting words for Nassar: NEWLINE  NEWLINE "I believe in God almighty SENTENCEEND SENTENCESTART  I believe in heaven and hell SENTENCEEND SENTENCESTART  And I can only hope when the day comes that Larry Nassar has ended his days on this Earth that he will be escorted to one of the deepest, darkest, hottest pits in hell there is SENTENCEEND SENTENCESTART "  http://fxn SENTENCEEND SENTENCESTART ws/2s2PGP6 NEXTPOST SENTENCESTART "Just this week, [the Democrats] literally sat on their hands when the National Anthem was being mentioned SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Fox & Friends," Kellyanne Conway slammed the Democrats as being elitist and out of touch SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART President Donald J SENTENCEEND SENTENCESTART  Trump arrives in Florida SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "We have 300 employees and we\'re giving $1,000 bonuses to the people who\'ve been there 10 years or more, and then like $900 to 9 years, $800 to 8-year employees, and so on SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Fox & Friends," Mincey Marbel CEO Donna Mincey explained how tax reform has led her to give her employees bonuses SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART President Donald J SENTENCEEND SENTENCESTART  Trump departs Joint Base Andrews aboard Air Force One en route to Mar-A-Lago SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "There\'s no doubt black lives matter, but I don\'t think you can protest on your job SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Fox & Friends," Herschel Walker slammed athletes protesting the National Anthem SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/2rZn6y5 NEXTPOST SENTENCESTART "We now know what happened: the Democratic Party hired the Perkins law firm SENTENCEEND SENTENCESTART  The Perkins law firm hired Fusion GPS SENTENCEEND SENTENCESTART  Fusion GPS hired Christopher Steele to go and dig up the dossier, and then they hired Nellie Ohr, the wife of Bruce Ohr, to get the fake dossier from a political environment into the bloodstream of the intelligence community SENTENCEEND SENTENCESTART " – Rep SENTENCEEND SENTENCESTART  Matt Gaetz fxn SENTENCEEND SENTENCESTART ws/2GJ2w8n NEXTPOST SENTENCESTART "There\'s no Mueller investigation without the dossier paid for by Clinton and DNC funds - so the whole thing is subject to really, I think, being called off now by the Justice Department, if they\'re brave enough SENTENCEEND SENTENCESTART " – Judicial Watch President Tom Fitton http://fxn SENTENCEEND SENTENCESTART ws/2GJ2w8n NEXTPOST SENTENCESTART Vice President Mike Pence makes remarks at a Rick Saccone for Congress event SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART BREAKING: The full House Intelligence Committee Report On FISA Abuses SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2GJ2w8n NEXTPOST SENTENCESTART Excerpt from Devin Nunes Memo: "Steele admitted to Ohr his feelings against then Candidate Trump, in September of 2016, when Steele told Ohr, that he Steele \'was desperate that Donald Trump not get elected and was passionate about him not being president SENTENCEEND SENTENCESTART \'" http://fxn SENTENCEEND SENTENCESTART ws/2GJ2w8n NEXTPOST SENTENCESTART "A lot of people should be ashamed of themselves and much worse than that SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE MOMENTS AGO: President Donald J SENTENCEEND SENTENCESTART  Trump commented on the GOP memo release during his meeting with North Korean defectors in the Oval Office SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/2nBJmZy NEXTPOST SENTENCESTART BREAKING NEWS: A disputed dossier was used by the FBI to obtain a warrant to spy on members of the Trump team, and the FISA court was never told the dossier was political opposition research, according to a House Intelligence Committee memo expected to be released in full today SENTENCEEND SENTENCESTART  fxn SENTENCEEND SENTENCESTART ws/2GJ2w8n NEXTPOST SENTENCESTART "Totally disrespectful SENTENCEEND SENTENCESTART  It gave a clear mark of what the Black Congressional Congress represents which is not us, the black families and the black people of America SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Fox & Friends," Bishop Leon Benjamin reacted to Democrats who remained seated at President Donald J SENTENCEEND SENTENCESTART  Trump\'s State of the Union address SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "The American public needs to see this [memo] because we need to know was an entire investigation of a president based on an accusation from a campaign opposition research?" NEWLINE  NEWLINE Moments ago, Sen SENTENCEEND SENTENCESTART  Rand Paul explained why he wants the GOP FISA memo released SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2FCZjG1 NEXTPOST SENTENCESTART "I think the guy is clairvoyant SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Fox & Friends," Mike Huckabee criticized Senator Chuck Schumer for essentially tearing apart the Senate Republicans\' health care bill before the ink was dry SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/2tGFNmR NEXTPOST SENTENCESTART "The Democratic Party is a party of coastal liberal elites SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Journal Editorial Report," Mary Kissel talked about why the Democratic Party is out of touch and what it needs to do to win back the American people SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Hillary Clinton didn\'t win the election and they\'ve got to find an excuse SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Fox & Friends," Mike Huckabee said that Democrats are desperate to explain their loss in 2016 and that\'s the source of the ongoing Russia accusations SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/2tGFNmR NEXTPOST SENTENCESTART Sean Spicer on former President Obama\'s criticism of the health bill: "The real meanness is allowing the American people to believe that ObamaCare is still alive SENTENCEEND SENTENCESTART  ObamaCare has failed SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  ObamaCare is dead SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART President Donald J SENTENCEEND SENTENCESTART  Trump during a Fox & Friends interview with Ainsley Earhardt: “Boy would the people love to see two parties getting together and coming up with the perfect healthcare plan SENTENCEEND SENTENCESTART ” http://fxn SENTENCEEND SENTENCESTART ws/2t2nJGv NEXTPOST SENTENCESTART Guests at a Florida hotel spotted this waterspout barreling towards them as Tropical Storm Cindy looms SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2rYslbT NEXTPOST SENTENCESTART Jose Aristimuno, a former deputy press secretary for the DNC, said Democrats want to create more middle class jobs, while simultaneously expanding the immigration system SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE "Have you thought this through at all?" Tucker Carlson said SENTENCEEND SENTENCESTART  "Maybe that\'s why you keep losing, because you say dumb things that don\'t make any sense SENTENCEEND SENTENCESTART " http://bit SENTENCEEND SENTENCESTART ly/2rXgRKJ NEXTPOST SENTENCESTART WATCH: President Donald J SENTENCEEND SENTENCESTART  Trump and Melania Trump host the Congressional Picnic SENTENCEEND SENTENCESTART  The annual event brings members of Congress and their families together for an informal evening with the president and first lady SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Some windsurfers were caught up in a "magical" event near the Golden Gate Bridge: They found themselves swimming with humpback whales SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART LIVE NOW: President Donald J SENTENCEEND SENTENCESTART  Trump holds a "Make America Great Again" rally to highlight his administration’s efforts to increase internet connectivity to rural and agricultural areas nationwide SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Live Blog: http://fxn SENTENCEEND SENTENCESTART ws/2sCnZJy NEXTPOST SENTENCESTART Watch: President Donald J SENTENCEEND SENTENCESTART  Trump arrives in Iowa ahead of tonight\'s rally SENTENCEEND SENTENCESTART  (Courtesy KGAN) http://fxn SENTENCEEND SENTENCESTART ws/2srUo7b NEXTPOST SENTENCESTART BREAKING UPDATE: Authorities say 50-year-old Canadian Amor Ftouhi yelled “Allah Akbar” as he stabbed an officer at Flint Bishop Airport in Michigan SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE LIVE: Authorities give an update on the the stabbing of a police officer at a Michigan airport on Wednesday that is being investigated by the FBI as a possible terror attack SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2sCnS0z NEXTPOST SENTENCESTART THE EAGLE HAS LANDED: But only for a moment to steal this fisherman’s bait SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/2tOPvmG NEXTPOST SENTENCESTART Press Secretary Sean Spicer says president Donald J SENTENCEEND SENTENCESTART  Trump “worked really hard to do what he could to secure the release” of Otto Warmbier during Tuesday\'s press briefing SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2rNAd53 NEXTPOST SENTENCESTART Julissa Arce, an undocumented immigrant and former Goldman Sachs executive, said the University of California was correct to allow illegal immigrants to receive preferential treatment versus out-of-state citizens SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/2tqEzMD NEXTPOST SENTENCESTART Five men were arrested in a botched home invasion after the homeowner fought back — with a machete SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2rKm9V7 NEXTPOST SENTENCESTART Greg Gutfeld Calls Out Hollywood For Meddling In The Georgia Election NEXTPOST SENTENCESTART WATCH LIVE: The White House Press Secretary Sean Spicer leads Tuesday\'s press briefing amid reports that he\'s being promoted within the Donald J SENTENCEEND SENTENCESTART  Trump administration SENTENCEEND SENTENCESTART  LIVE BLOG: http://fxn SENTENCEEND SENTENCESTART ws/2rNAd53 NEXTPOST SENTENCESTART Watch live: Vice President Mike Pence speaks at Day 1 of the 2017 Manufacturing Summit SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART A Florida deputy thinks he knows why the bear crossed the road SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE “He was probably just looking for donuts and you know, what better place to look than a cop car?” http://fxn SENTENCEEND SENTENCESTART ws/2rOalBb NEXTPOST SENTENCESTART Greg Gutfeld: The Left Attacks Scalise’s Politics During His Recovery NEWLINE  NEWLINE Read more: http://bit SENTENCEEND SENTENCESTART ly/2rPn32h NEXTPOST SENTENCESTART "It\'s a brutal regime, and we\'ll be able to handle it SENTENCEEND SENTENCESTART "  NEWLINE  NEWLINE President Trump reacted to the death of college student Otto Warmbier, who was in a coma when North Korea released him last week SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/2tlO7Ir NEXTPOST SENTENCESTART “The VA has already been able to reduce the average wait time for claim establishment from 25 days to eight SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE WATCH: Jared Kushner spoke at a summit on Monday aimed at gathering ideas for modernizing the government SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/2tG8cst NEXTPOST SENTENCESTART "This is the definition of fake news SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Fox & Friends," Amy Holmes talked about media reports that very soon Mike Pence will assume the presidency and will select his own VP SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "We\'ve seen the left over the last 20-25 years get more and more and more militant SENTENCEEND SENTENCESTART " – Newt Gingrich NEXTPOST SENTENCESTART “Half of them didn’t even realize an AR-15 was not used, or that an AR-15 is not an assault weapon and that these weapons were obtained legally SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE Kimberly Guilfoyle sounded off on The New York Times editorial board for saying that the National Rifle Association is “complicit” in the Orlando terror attack SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "The motives of this killer may have been different than the mass shooters in Aurora or Newtown SENTENCEEND SENTENCESTART  But the instruments of death were so similar SENTENCEEND SENTENCESTART  Now another forty-nine innocent people are dead SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Moments ago, President Obama spoke in Orlando, Florida, in the wake of the deadly terror attack SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The National Rifle Association released a statement on meeting with Donald J SENTENCEEND SENTENCESTART  Trump over not allowing people on terrorist watch lists to buy guns SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Live Now: Officials in Florida give an update on the Orlando gator attack SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART President Barack Obama calls for more gun control in the wake of the #OrlandoShooting: "Reinstate the assault weapons ban SENTENCEEND SENTENCESTART  Make it harder for terrorists to use these weapons to kill us SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "What exactly would using this label accomplish?" NEWLINE  NEWLINE In a speech a short time ago, President Obama fired back at those who have criticized him for failing to use the phrase "radical Islam SENTENCEEND SENTENCESTART " http://bit SENTENCEEND SENTENCESTART ly/1OnAZve NEXTPOST SENTENCESTART Mourners around the world gathered to remember the 49 victims of Sunday\'s shooting at the Pulse nightclub SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Donald J SENTENCEEND SENTENCESTART  Trump reacts to the #OrlandoShooting: “When it comes to radical Islamic terrorism, ignorance is not bliss SENTENCEEND SENTENCESTART  It’s deadly SENTENCEEND SENTENCESTART ” NEXTPOST SENTENCESTART FBI Director James Comey on #OrlandoShooting: "I don\'t see anything in reviewing our work that our agents should have done differently, but we\'ll look at it in an open and honest way and be transparent about it SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Florida Senator Bill Nelson: "The Congress ought to declare war against ISIS SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Early Monday, the man who sent these chilling text messages to his mom from inside Pulse nightclub during the #OrlandoShooting was confirmed among the victims SENTENCEEND SENTENCESTART  Eddie Justice was 30 years old SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/1UdsySx NEXTPOST SENTENCESTART "This phrase of \'lone wolf terrorism\'  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  is a phrase created by the administration to make US citizens stupid SENTENCEEND SENTENCESTART  There\'s no such thing as \'lone wolf terrorism SENTENCEEND SENTENCESTART "\' NEWLINE  NEWLINE Dr SENTENCEEND SENTENCESTART  Sebastian Gorka said we have to understand that "this is a different, theologically-motivated terrorist SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART Chris Ortiz, a survivor of the #OrlandoShooting shares his harrowing story of survival: "There was no other place to hide SENTENCEEND SENTENCESTART  There was no other place to run  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  the bullets were going crazy and crazy SENTENCEEND SENTENCESTART " NEXTPOST SENTENCESTART "Because Obama has allowed this cancer to metastasize so broadly, it\'s going to be a decades-long fight, even if we fight it seriously SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Lt SENTENCEEND SENTENCESTART  Col SENTENCEEND SENTENCESTART  Ralph Peters criticized the President\'s handling of fighting the terror threat from ISIS and other groups SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "This containment garbage is getting people killed SENTENCEEND SENTENCESTART  You don\'t contain evil SENTENCEEND SENTENCESTART  You destroy it or it destroys you SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Bill O\'Reilly called out President Barack Obama for “putting the emphasis on guns rather than Islamic terrorism” in the wake of the #OrlandoShooting SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Watch Florida Senator Bill Nelson give an update on the tragic shooting that killed 50 people and injured 53 at a local nightclub SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Watch: The White House lowers the flags for the victims of Orlando where the deadliest shooting in modern U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  history took place early this morning SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART “We know enough to say that this was an act of terror and an act of hate SENTENCEEND SENTENCESTART ”  NEWLINE  NEWLINE Moments ago, President Barack Obama addressed the city of Orlando facing the tragedy of the deadliest shooting in modern U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  history SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Breaking News: Santa Monica authorities reportedly arrest a man who had assault rifles, ammunition and materials that could be used to create explosives inside his car ahead of a Gay Pride parade SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/1XhgoeG NEXTPOST SENTENCESTART Mourners around the world gathered to remember the 50 people who lost their lives at the Pulse nightclub, in what is the deadliest mass shooting in modern US history SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/21geARN NEXTPOST SENTENCESTART “Just to look into the eyes of our officers told the whole story…You can tell that they were all shaken by this incident by what they saw inside the club SENTENCEEND SENTENCESTART ”  NEWLINE  NEWLINE Watch police explain the horror inside the Orlando nightclub where 50 people were killed and at least 53 were injured SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Really bad shooting in Orlando SENTENCEEND SENTENCESTART  Police investigating possible terrorism SENTENCEEND SENTENCESTART  Many people dead and wounded SENTENCEEND SENTENCESTART "  NEWLINE  NEWLINE Donald J SENTENCEEND SENTENCESTART  Trump reacts to the horrific nightclub shooting that happened in Orlando last night: NEXTPOST SENTENCESTART Watch Donald J SENTENCEEND SENTENCESTART  Trump fire back in response to Mitt Romney\'s attacks SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/1S01UYE NEXTPOST SENTENCESTART Jesse Watters: "Do you think Hillary has been clear and confident?" NEWLINE  NEWLINE Answer: "Absolutely SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Watters: "Then why hasn\'t she held a single press conference in 2016?" NEWLINE  NEWLINE More: http://bit SENTENCEEND SENTENCESTART ly/25PxLbM NEXTPOST SENTENCESTART “Freedom of any kind means no one should be judged by their race or their color and the color of their skin…Right now, we have a very divided nation SENTENCEEND SENTENCESTART  We’re going to bring our nation together SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Donald J SENTENCEEND SENTENCESTART  Trump addresses the Faith and Freedom Coalition conference in Washington D SENTENCEEND SENTENCESTART C SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART The murder rate in Baltimore is the highest it has been in two decades SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE Milwaukee County Sheriff David Clarke weighed in on \'America\'s Newsroom\' and said "a failure of leadership" is to blame SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/1HRIGp9 NEXTPOST SENTENCESTART "When citizens become dependent on government to live day to day, that weakens the entire nation SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE In tonight\'s Talking Points Memo, Bill O\'Reilly discussed the consequences of having about a third of the population living in households that receive government assistance SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "I just respectfully disagree with Senator Paul SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE On "Your World with Neil Cavuto" today, Gov SENTENCEEND SENTENCESTART  Scott Walker reacted to the claims made by Sen SENTENCEEND SENTENCESTART  Rand Paul about ISIS being created by GOP "hawks SENTENCEEND SENTENCESTART " http://bit SENTENCEEND SENTENCESTART ly/1Rs9DRQ NEXTPOST SENTENCESTART Leland Vittert asked Mayor Stephanie Rawlings-Blake directly if the huge increase in violence in Baltimore was a result of her policies SENTENCEEND SENTENCESTART  (via Special Report with Bret Baier) http://fxn SENTENCEEND SENTENCESTART ws/1SF6qQo NEXTPOST SENTENCESTART Video below shows the San Jacinto River flooding at I-45 near Conroe, TX SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART #HugACop is back!  NEWLINE  NEWLINE Share the love with your friends SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "First of all, we have a president who has no clue what he\'s doing SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Donald J SENTENCEEND SENTENCESTART  Trump went "On The Record with Greta Van Susteren" to share his unvarnished opinion of President Barack Obama\'s ISIS strategy SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/1dyMrTD NEXTPOST SENTENCESTART "When the bill comes due, and something awful happens, this country will quickly snap back to traditional thinking SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE In tonight\'s Talking Points Memo, Bill O\'Reilly discussed a poll that shows that the number of social liberals actually matches the number of conservatives in the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/1EyyHx6 NEXTPOST SENTENCESTART In Case of Emergency: SpaceX designed and tested a vehicle to help carry astronauts to safety SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "Our Founding Fathers didn\'t like the idea of general warrants SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  That\'s what the Fourth Amendment is about SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Sen SENTENCEEND SENTENCESTART  Rand Paul joined The Kelly File to explain his criticism of the NSA - National Security Agency SENTENCEEND SENTENCESTART http://bit SENTENCEEND SENTENCESTART ly/1FNZKKj NEXTPOST SENTENCESTART Authorities in central Texas ended their search late Monday for 12 people missing after a vacation home where they were staying was swept away by a flash flood SENTENCEEND SENTENCESTART  NEWLINE  NEWLINE READ: http://fxn SENTENCEEND SENTENCESTART ws/1ertwKF NEXTPOST SENTENCESTART People across the United States honor the fallen on Memorial Day SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Do you find PacSun\'s shirt that pictures an upside-down American flag disrespectful? NEXTPOST SENTENCESTART A storm system dropped record amounts of rainfall across the southern Plains Sunday, causing flash floods in normally dry riverbeds, spawning tornadoes, destroying homes, and forcing at least 2,000 people to flee SENTENCEEND SENTENCESTART  Three people died and several others are missing SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART “I didn’t know what I was going to pray about but as I stepped up to the podium, I said, ‘Lord, let your will be done SENTENCEEND SENTENCESTART ’” NEWLINE  NEWLINE Senior Christian Crawford jumped up to lead the audience in prayer when a woman had a seizure during his high school graduation ceremony in Alabama: http://bit SENTENCEEND SENTENCESTART ly/1LyRa21 NEXTPOST SENTENCESTART Wounded warriors reflect on what Memorial Day means to them SENTENCEEND SENTENCESTART  Fox News and the Wounded Warrior Project honor the heroic military men and women who gave their lives fighting for our great country SENTENCEEND SENTENCESTART  #ProudAmerican NEXTPOST SENTENCESTART Video shows the wreath-laying ceremony at the Tomb of the Unknown Soldier at Arlington National Cemetery on Memorial Day SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE Watch President Barack Obama\'s remarks live: http://bit SENTENCEEND SENTENCESTART ly/P54fcL NEXTPOST SENTENCESTART WATCH a New Jersey community come together by placing more than 15,000 flags in the outfield of a ball park to honor those who have made the ultimate sacrifice SENTENCEEND SENTENCESTART  #ProudAmerican NEXTPOST SENTENCESTART Peek-a-boo!  NEWLINE  NEWLINE (Via: San Diego Zoo) NEXTPOST SENTENCESTART "To tell a group of military graduates that climate change is a defense priority borders on delusion SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Bill O\'Reilly sounded off on President Barack Obama\'s address on global climate change to the graduating class at the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  Coast Guard Academy SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/1F4RmlJ NEXTPOST SENTENCESTART A recent Pew poll that saw the amount of Americans identifying as Christian drop substantially in the last eight years also shows that the number of Evangelical Christians in the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  has risen by roughly two million people SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/1SkzhJN NEXTPOST SENTENCESTART Heroic Rescue: Video shows the moment a police officer jumped into rushing waters to save a dog in Colombia SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/1FF2R9d NEXTPOST SENTENCESTART Police believe Daron Dylon Wint – the suspect in the D SENTENCEEND SENTENCESTART C SENTENCEEND SENTENCESTART  quadruple murder – may be in Brooklyn, New York SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE DETAILS: http://fxn SENTENCEEND SENTENCESTART ws/1EkRR9J NEXTPOST SENTENCESTART "But despite all the carnage, President Obama and the Pentagon did not develop any cohesive plan to defeat ISIS SENTENCEEND SENTENCESTART  And to this day there is no campaign SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE In his Talking Points Memo Wednesday night, Bill O\'Reilly wondered if the jihad will defeat America SENTENCEEND SENTENCESTART  WATCH more: http://bit SENTENCEEND SENTENCESTART ly/1Bd3Ztj NEXTPOST SENTENCESTART Quadruplets! NEWLINE  NEWLINE (Photo Credits: Ingrid Barrentine/ Point Defiance Zoo & Aquarium) NEXTPOST SENTENCESTART “With all due respect Mr SENTENCEEND SENTENCESTART  President, you created this mess - now you fix it! You invited them in, now you solve it [ SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART ] Illegally entering the United States is not a right to citizenship SENTENCEEND SENTENCESTART  Don\'t tell us we\'re heartless because we believe in the rule of law, which is the foundation upon which our country was founded SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE In last night\'s Opening Statement, Judge Jeanine Pirro blasted President Barack Obama for the illegal immigration crisis, accusing him of attempting to change the demographics and electorate of the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  Watch her entire Opening Statement: http://tinyurl SENTENCEEND SENTENCESTART com/m28vcal NEXTPOST SENTENCESTART "We sacrificed thousands of our American heroes liberating Iraq, only for President Obama to pull out all the troops, leaving Iraq now to be taken over by the terrorists SENTENCEEND SENTENCESTART  What does he say about that? Nothing SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE Tonight on \'Huckabee,\' actor Jon Voight shares an important message for America at 8p ET SENTENCEEND SENTENCESTART  (via The Huckabee Show) NEXTPOST SENTENCESTART “She misspoke? What part? When she said five guys were determining what forms of contraception are legal? […] Give me a break SENTENCEEND SENTENCESTART  One of the marks of a real leader is owning up to one’s mistakes SENTENCEEND SENTENCESTART  Oh yeah, honesty helps a lot too SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE Hear Megyn Kelly’s full remarks on Rep SENTENCEEND SENTENCESTART  Nancy Pelosi’s claim that she “misspoke” on the Supreme Court’s Hobby Lobby ruling: http://tinyurl SENTENCEEND SENTENCESTART com/mr6z2vw NEXTPOST SENTENCESTART Cops in Florida and Indiana are using a new high-tech spray known as "SmartWater" to help catch criminals SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE WATCH: http://bit SENTENCEEND SENTENCESTART ly/W4d8Xx (via Fox & Friends) NEXTPOST SENTENCESTART Gov SENTENCEEND SENTENCESTART  Rick Perry and Texas border patrol agents took Sean Hannity on a personal tour of the U SENTENCEEND SENTENCESTART S SENTENCEEND SENTENCESTART  border to highlight the challenges and the security efforts made to protect the border SENTENCEEND SENTENCESTART  For more: http://tinyurl SENTENCEEND SENTENCESTART com/qxuczxs NEXTPOST SENTENCESTART "We feel [Tahmooressi\'s] hearing was a success SENTENCEEND SENTENCESTART   We feel we are finally on the right track in order to achieve what every one of us is ultimately hoping for SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE More \'On the Record\' RIGHT NOW with Fernando Benitez, attorney for Sgt SENTENCEEND SENTENCESTART  Andrew Tahmooressi SENTENCEEND SENTENCESTART  #MarineHeldinMexico NEXTPOST SENTENCESTART Texas Gov SENTENCEEND SENTENCESTART  Rick Perry says President Obama must act now to stop the surge of illegal immigrants flooding across his state’s border SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/1zqMJ56  NEWLINE  NEWLINE Tune in tonight at 10p ET as Sean Hannity tours the border with Gov SENTENCEEND SENTENCESTART  Perry SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART On The Kelly File, Sen SENTENCEEND SENTENCESTART  Ted Cruz called President Obama an “absentee president” who is not focused on the people “who are paying the cost for his failed policies SENTENCEEND SENTENCESTART ” Watch the interview: http://tinyurl SENTENCEEND SENTENCESTART com/l3fx3ua NEXTPOST SENTENCESTART 2-year-old Kayden Kinckle became a viral sensation after he took his first steps on prosthetics and exclaimed, "I got it!" NEWLINE  NEWLINE This morning on Fox & Friends, Kayden joined us by walking on set! Full interview: http://bit SENTENCEEND SENTENCESTART ly/U4mzVf NEXTPOST SENTENCESTART ‘Princeton Mom’ Susan Patton, whose comments went viral after advising female students to find husbands in college, now says women need to be smart and appreciative enough to hold on to their husbands SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE “If you’re in your mid-thirties or older, the idea that you’re going to find yourself another husband — almost impossible," she said on Fox & Friends earlier today SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Former POW Jessica Lynch said it was “heartbreaking” to learn that one of her rescuers Command Sgt SENTENCEEND SENTENCESTART  Maj SENTENCEEND SENTENCESTART  Martin Barreras had died from wounds suffered in Afghanistan SENTENCEEND SENTENCESTART    NEWLINE  NEWLINE “I thank him every day for his service, of coming in, risking his life and coming to get me,” Lynch said SENTENCEEND SENTENCESTART  Watch her interview on The Huckabee Show: http://tinyurl SENTENCEEND SENTENCESTART com/nrc9rhv NEXTPOST SENTENCESTART Members of Congress only plan to work 28 days until the midterm elections in November, which is about 1 SENTENCEEND SENTENCESTART 5 days per week SENTENCEEND SENTENCESTART  Fox News contributor Juan Williams said this is the least productive Congress in history on "Cashin’ In SENTENCEEND SENTENCESTART " Watch more of the debate: http://tinyurl SENTENCEEND SENTENCESTART com/pjwu5u5 NEXTPOST SENTENCESTART TONIGHT: What makes America different? \'The Kelly File\' gets an inside look at Dinesh D\'Souza\'s new film, "America" at 9p ET SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Janice Dean is keeping an eye on Hurricane Arthur SENTENCEEND SENTENCESTART  Get the latest on the path of the storm: http://tinyurl SENTENCEEND SENTENCESTART com/mj64ca9 NEXTPOST SENTENCESTART Thank you for your #ProudAmerican photos SENTENCEEND SENTENCESTART  Keep sharing them and help us celebrate America\'s birthday! http://fxn SENTENCEEND SENTENCESTART ws/1rXmQVO NEXTPOST SENTENCESTART Jenna Lee and Jon Scott reflect on what it means to have loved ones who are willing to sacrifice their lives for America\'s freedom SENTENCEEND SENTENCESTART  http://fxn SENTENCEEND SENTENCESTART ws/TFO5bh NEWLINE  NEWLINE In honor of the 4th of July, the Fox News family shares their pride SENTENCEEND SENTENCESTART  Share yours with #ProudAmerican SENTENCEEND SENTENCESTART  Don\'t miss the Fox News special "Proud to Be An American," on Friday July 4th at 12p ET SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART Here\'s your patriotic wake up call!  NEWLINE  NEWLINE The United States Marine Drum & Bugle Corps performed live this morning on Fox & Friends SENTENCEEND SENTENCESTART  #ProudAmerican NEXTPOST SENTENCESTART ICYMI: Larry the Cable Guy surprised troops at Fort Bragg on Fox & Friends! #ProudAmerican NEXTPOST SENTENCESTART President Obama to Republicans in Congress: “So, sue me SENTENCEEND SENTENCESTART ”  NEWLINE  NEWLINE WATCH what former White House senior advisor Karl Rove had to say about Obama\'s blunt remarks: http://tinyurl SENTENCEEND SENTENCESTART com/kdpkxnd NEXTPOST SENTENCESTART It has been 77 years since Amelia Earhart disappeared on a flight around the world SENTENCEEND SENTENCESTART  Now another pilot, who is her namesake, is attempting to fly the exact same route SENTENCEEND SENTENCESTART  http://tinyurl SENTENCEEND SENTENCESTART com/ngdyac7 NEXTPOST SENTENCESTART "When I think about what this country means to me, it brings tears to my eyes  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  SENTENCEEND SENTENCESTART  I started my life in a country that was anything but free SENTENCEEND SENTENCESTART " -Dr SENTENCEEND SENTENCESTART  Manny Alvarez shares his emotional story http://fxn SENTENCEEND SENTENCESTART ws/1lU8cxP NEWLINE  NEWLINE As we approach the 4th of July, the Fox News family shares their pride SENTENCEEND SENTENCESTART  Share yours with #ProudAmerican SENTENCEEND SENTENCESTART   NEWLINE  NEWLINE Don\'t miss the Fox News special "Proud to Be An American," on Friday July 4th at 12p ET SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART After learning his town was cancelling its Fourth of July parade due to a “lack of interest,” Army veteran Glen Phillips took to Facebook and wrote, “If there was only 1 American left in America, that 1 person on the 4th of July should carry our flag and celebrate it because freedom is not free SENTENCEEND SENTENCESTART ” NEWLINE  NEWLINE Thanks to this #ProudAmerican, Liberty, Kentucky’s Independence Day parade is back on! http://bit SENTENCEEND SENTENCESTART ly/1rXl477 NEXTPOST SENTENCESTART As we approach the 4th of July, the Fox News family shares their pride! Share yours with #ProudAmerican SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/1vpauWB NEWLINE  NEWLINE Don\'t miss the Fox News special "Proud to Be An American," this Friday July 4th at 12p ET SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART "You could have murdered somebody with those bombs SENTENCEEND SENTENCESTART " NEWLINE  NEWLINE Megyn Kelly presses Weather Underground co-founder Bill Ayers on his multiple alleged bombings of government buildings during his time with the violent radical group SENTENCEEND SENTENCESTART  Don\'t miss it TONIGHT on \'The Kelly File\' at 9p ET SENTENCEEND SENTENCESTART  NEXTPOST SENTENCESTART As we approach the 4th of July, the Fox News family shares their pride SENTENCEEND SENTENCESTART  Share yours with #ProudAmerican SENTENCEEND SENTENCESTART  http://bit SENTENCEEND SENTENCESTART ly/1vpauWB NEWLINE  NEWLINE “Freedom isn\'t free, individual responsibility will make the difference, but America is the greatest country in the world because of opportunity SENTENCEEND SENTENCESTART ” – Bill O\'Reilly NEWLINE  NEWLINE Don\'t miss the Fox News special "Proud to Be An American," this Friday July 4th at 12p ET SENTENCEEND SENTENCESTART ']



After tokenning our posts, we split them to words:


    from keras.preprocessing.text import text_to_word_sequence
    allpostsAsSequence= []
    for post in allpostsCombined:
        allpostsAsSequence.append(text_to_word_sequence(post, lower=False, split=" "))
    allpostsAsSequence




    [['NEXTPOST',
      'SENTENCESTART',
      'Today',
      'it',
      'was',
      'my',
      'great',
      'honor',
      'to',
      'host',
      'a',
      'School',
      'Safety',
      'Roundtable',
      'at',
      'the',
      'White',
      'House',
      'with',
      'State',
      'and',
      'local',
      'leaders',
      'law',
      'enforcement',
      'officers',
      'and',
      'education',
      'officials',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'There',
      'is',
      'nothing',
      'more',
      'important',
      'than',
      'protecting',
      'our',
      'children',
      'SENTENCEEND',
      'SENTENCESTART',
      'They',
      'deserve',
      'to',
      'be',
      'safe',
      'and',
      'we',
      'will',
      'deliver',
      'NEXTPOST',
      'SENTENCESTART',
      'History',
      'shows',
      'that',
      'a',
      'school',
      'shooting',
      'lasts',
      'on',
      'average',
      '3',
      'minutes',
      'SENTENCEEND',
      'SENTENCESTART',
      'It',
      'takes',
      'police',
      'first',
      'responders',
      'approximately',
      '5',
      'to',
      '8',
      'minutes',
      'to',
      'get',
      'to',
      'site',
      'of',
      'crime',
      'SENTENCEEND',
      'SENTENCESTART',
      'Highly',
      'trained',
      'gun',
      'adept',
      'teachers',
      'coaches',
      'would',
      'solve',
      'the',
      'problem',
      'instantly',
      'before',
      'police',
      'arrive',
      'SENTENCEEND',
      'SENTENCESTART',
      'GREAT',
      'DETERRENT',
      'NEXTPOST',
      'SENTENCESTART',
      'I',
      'will',
      'be',
      'strongly',
      'pushing',
      'Comprehensive',
      'Background',
      'Checks',
      'with',
      'an',
      'emphasis',
      'on',
      'Mental',
      'Health',
      'SENTENCEEND',
      'SENTENCESTART',
      'Raise',
      'age',
      'to',
      '21',
      'and',
      'end',
      'sale',
      'of',
      'Bump',
      'Stocks',
      'NEXTPOST',
      'SENTENCESTART',
      'Question',
      'If',
      'all',
      'of',
      'the',
      'Russian',
      'meddling',
      'took',
      'place',
      'during',
      'the',
      'Obama',
      'Administration',
      'right',
      'up',
      'to',
      'January',
      '20th',
      'why',
      'aren’t',
      'they',
      'the',
      'subject',
      'of',
      'the',
      'investigation',
      'NEXTPOST',
      'SENTENCESTART',
      'President',
      'Donald',
      'J',
      'SENTENCEEND',
      'SENTENCESTART',
      "Trump's",
      'schedule',
      'for',
      'Thursday',
      'February',
      '22nd',
      'NEWLINE',
      '·',
      'Meeting',
      'with',
      'State',
      'and',
      'local',
      'officials',
      'on',
      'school',
      'safety',
      'NEXTPOST',
      'SENTENCESTART',
      'I',
      'will',
      'always',
      'remember',
      'the',
      'time',
      'I',
      'spent',
      'today',
      'with',
      'courageous',
      'students',
      'teachers',
      'and',
      'families',
      'SENTENCEEND',
      'SENTENCESTART',
      'So',
      'much',
      'love',
      'in',
      'the',
      'midst',
      'of',
      'so',
      'much',
      'pain',
      'SENTENCEEND',
      'SENTENCESTART',
      'We',
      'must',
      'not',
      'let',
      'them',
      'down',
      'SENTENCEEND',
      'SENTENCESTART',
      'We',
      'must',
      'keep',
      'our',
      'children',
      'safe',
      'NEXTPOST',
      'SENTENCESTART',
      'Statement',
      'on',
      'the',
      'Passing',
      'of',
      'Reverend',
      'Billy',
      'Graham',
      'NEWLINE',
      'NEWLINE',
      'Melania',
      'and',
      'I',
      'join',
      'millions',
      'of',
      'people',
      'around',
      'the',
      'world',
      'in',
      'mourning',
      'the',
      'passing',
      'of',
      'Billy',
      'Graham',
      'SENTENCEEND',
      'SENTENCESTART',
      'Our',
      'prayers',
      'are',
      'with',
      'his',
      'children',
      'grandchildren',
      'great',
      'grandchildren',
      'and',
      'all',
      'who',
      'worked',
      'closely',
      'with',
      'Reverend',
      'Graham',
      'in',
      'his',
      'lifelong',
      'ministry',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'Billy’s',
      'acceptance',
      'of',
      'Jesus',
      'Christ',
      'around',
      'his',
      'seventeenth',
      'birthday',
      'not',
      'only',
      'changed',
      'his',
      'life—it',
      'changed',
      'our',
      'country',
      'and',
      'the',
      'world',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'was',
      'one',
      'of',
      'the',
      'towering',
      'figures',
      'of',
      'the',
      'last',
      '100',
      'years—an',
      'American',
      'hero',
      'whose',
      'life',
      'and',
      'leadership',
      'truly',
      'earned',
      'him',
      'the',
      'title',
      '“God’s',
      'Ambassador',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'NEWLINE',
      'NEWLINE',
      'Billy’s',
      'unshakeable',
      'belief',
      'in',
      'the',
      'power',
      'of',
      'God’s',
      'word',
      'to',
      'transform',
      'hearts',
      'gave',
      'hope',
      'to',
      'all',
      'who',
      'listened',
      'to',
      'his',
      'simple',
      'message',
      '“God',
      'loves',
      'you',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'He',
      'carried',
      'this',
      'message',
      'around',
      'the',
      'world',
      'through',
      'his',
      'crusades',
      'bringing',
      'entire',
      'generations',
      'to',
      'faith',
      'in',
      'Jesus',
      'Christ',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'In',
      'the',
      'wake',
      'of',
      'the',
      'September',
      '11th',
      'attacks',
      'in',
      '2001',
      'America',
      'turned',
      'to',
      'Billy',
      'Graham',
      'at',
      'the',
      'National',
      'Cathedral',
      'who',
      'told',
      'us',
      '“God',
      'can',
      'be',
      'trusted',
      'even',
      'when',
      'life',
      'seems',
      'at',
      'its',
      'darkest',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'NEWLINE',
      'NEWLINE',
      'Reverend',
      'Graham',
      'would',
      'be',
      'the',
      'first',
      'to',
      'say',
      'that',
      'he',
      'did',
      'not',
      'do',
      'it',
      'alone',
      'SENTENCEEND',
      'SENTENCESTART',
      'Before',
      'her',
      'passing',
      'his',
      'wife',
      'Ruth',
      'was',
      'by',
      'his',
      'side',
      'through',
      'it',
      'all—a',
      'true',
      'partner',
      'a',
      'wonderful',
      'mother',
      'and',
      'a',
      'fellow',
      'missionary',
      'soul',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'also',
      'built',
      'an',
      'international',
      'team',
      'and',
      'institution',
      'that',
      'will',
      'continue',
      'to',
      'carry',
      'on',
      'Christ’s',
      'message',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'Melania',
      'and',
      'I',
      'were',
      'privileged',
      'to',
      'get',
      'to',
      'know',
      'Reverend',
      'Graham',
      'and',
      'his',
      'extraordinary',
      'family',
      'over',
      'the',
      'last',
      'several',
      'years',
      'and',
      'we',
      'are',
      'deeply',
      'grateful',
      'for',
      'their',
      'love',
      'and',
      'support',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'Billy',
      'Graham',
      'was',
      'truly',
      'one',
      'of',
      'a',
      'kind',
      'SENTENCEEND',
      'SENTENCESTART',
      'Christians',
      'and',
      'people',
      'of',
      'all',
      'faiths',
      'and',
      'backgrounds',
      'will',
      'miss',
      'him',
      'dearly',
      'SENTENCEEND',
      'SENTENCESTART',
      'We',
      'are',
      'thinking',
      'of',
      'him',
      'today',
      'finally',
      'at',
      'home',
      'in',
      'Heaven',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Bad',
      'ratings',
      'CNN',
      'and',
      'MSNBC',
      'got',
      'scammed',
      'when',
      'they',
      'covered',
      'the',
      'anti',
      'Trump',
      'Russia',
      'rally',
      'wall',
      'to',
      'wall',
      'SENTENCEEND',
      'SENTENCESTART',
      'Two',
      'really',
      'dishonest',
      'newscasters',
      'but',
      'the',
      'public',
      'is',
      'wise',
      'NEXTPOST',
      'SENTENCESTART',
      'The',
      'GREAT',
      'Billy',
      'Graham',
      'is',
      'dead',
      'SENTENCEEND',
      'SENTENCESTART',
      'There',
      'was',
      'nobody',
      'like',
      'him',
      'He',
      'will',
      'be',
      'missed',
      'by',
      'Christians',
      'and',
      'all',
      'religions',
      'SENTENCEEND',
      'SENTENCESTART',
      'A',
      'very',
      'special',
      'man',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'When',
      'faced',
      'with',
      'danger',
      'they',
      'each',
      'put',
      'the',
      'lives',
      'of',
      'others',
      'before',
      'their',
      'own',
      'SENTENCEEND',
      'SENTENCESTART',
      'God',
      'forever',
      'bless',
      'these',
      'GREAT',
      'heroes',
      'NEXTPOST',
      'SENTENCESTART',
      'President',
      'Donald',
      'J',
      'SENTENCEEND',
      'SENTENCESTART',
      "Trump's",
      'schedule',
      'for',
      'Wednesday',
      'February',
      '21st',
      'NEWLINE',
      '·',
      'Discuss',
      'the',
      'Economic',
      'Report',
      'of',
      'the',
      'President',
      'with',
      'the',
      'Council',
      'of',
      'Economic',
      'Advisers',
      'NEWLINE',
      '·',
      'Lunch',
      'with',
      'the',
      'Administrator',
      'of',
      'the',
      'Small',
      'Business',
      'Administration',
      'NEWLINE',
      '·',
      'Meeting',
      'with',
      'trade',
      'union',
      'leaders',
      'NEWLINE',
      '·',
      'Listening',
      'session',
      'with',
      'high',
      'school',
      'students',
      'and',
      'teachers',
      'NEXTPOST',
      'SENTENCESTART',
      'The',
      'U',
      'SENTENCEEND',
      'SENTENCESTART',
      'S',
      'SENTENCEEND',
      'SENTENCESTART',
      'economy',
      'is',
      'looking',
      'very',
      'good',
      'in',
      'my',
      'opinion',
      'even',
      'better',
      'than',
      'anticipated',
      'SENTENCEEND',
      'SENTENCESTART',
      'JOBS',
      'JOBS',
      'JOBS',
      'NEXTPOST',
      'SENTENCESTART',
      'Republicans',
      'are',
      'now',
      'leading',
      'the',
      'Generic',
      'Poll',
      'perhaps',
      'because',
      'of',
      'the',
      'popular',
      'Tax',
      'Cuts',
      'which',
      'the',
      'Dems',
      'want',
      'to',
      'take',
      'away',
      'NEXTPOST',
      'SENTENCESTART',
      'Main',
      'Street',
      'is',
      'BOOMING',
      'thanks',
      'to',
      'our',
      'incredible',
      'TAX',
      'CUT',
      'and',
      'Reform',
      'law',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'This',
      'shows',
      'small',
      'business',
      'owners',
      'are',
      'more',
      'than',
      'just',
      'optimistic',
      'they',
      'are',
      'ready',
      'to',
      'grow',
      'their',
      'businesses',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Thank',
      'you',
      'to',
      'Fox',
      'Friends',
      'for',
      'the',
      'great',
      'timeline',
      'on',
      'all',
      'of',
      'the',
      'failures',
      'the',
      'Obama',
      'Administration',
      'had',
      'against',
      'Russia',
      'including',
      'Crimea',
      'Syria',
      'and',
      'so',
      'much',
      'more',
      'SENTENCEEND',
      'SENTENCESTART',
      'We',
      'are',
      'now',
      'starting',
      'to',
      'win',
      'again',
      'NEXTPOST',
      'SENTENCESTART',
      'President',
      'Donald',
      'J',
      'SENTENCEEND',
      'SENTENCESTART',
      'Trump’s',
      'schedule',
      'for',
      'Tuesday',
      'February',
      '20th',
      'NEWLINE',
      '·',
      'Lunch',
      'with',
      'Vice',
      'President',
      'Mike',
      'Pence',
      'and',
      'Secretary',
      'of',
      'Defense',
      'Jim',
      'Mattis',
      'NEWLINE',
      '·',
      'Meeting',
      'with',
      'Secretary',
      'of',
      'the',
      'Treasury',
      'Steve',
      'Mnuchin',
      'NEWLINE',
      '·',
      'Public',
      'Safety',
      'Medal',
      'of',
      'Valor',
      'Awards',
      'Ceremony',
      'NEXTPOST',
      'SENTENCESTART',
      'You',
      'wouldn’t',
      'know',
      'it',
      'by',
      'watching',
      'the',
      'Fake',
      'News',
      'media',
      'but',
      'our',
      'economy',
      'is',
      'BOOMING',
      'NEXTPOST',
      'SENTENCESTART',
      'Obama',
      'was',
      'President',
      'up',
      'to',
      'and',
      'beyond',
      'the',
      '2016',
      'Election',
      'SENTENCEEND',
      'SENTENCESTART',
      'So',
      'why',
      'didn’t',
      'he',
      'do',
      'something',
      'about',
      'Russian',
      'meddling',
      'NEXTPOST',
      'SENTENCESTART',
      'Have',
      'a',
      'great',
      'but',
      'very',
      'reflective',
      'President’s',
      'Day',
      'NEXTPOST',
      'SENTENCESTART',
      'This',
      'President’s',
      'Day',
      'I',
      'honor',
      'all',
      'past',
      'presidents',
      'and',
      'reaffirm',
      'my',
      'commitment',
      'to',
      'finish',
      'the',
      'job',
      'of',
      'those',
      'who',
      'served',
      'before',
      'me',
      'by',
      'making',
      'America',
      'GREAT',
      'again',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'If',
      'it',
      'was',
      'the',
      'GOAL',
      'of',
      'Russia',
      'to',
      'create',
      'discord',
      'disruption',
      'and',
      'chaos',
      'within',
      'the',
      'U',
      'SENTENCEEND',
      'SENTENCESTART',
      'S',
      'SENTENCEEND',
      'SENTENCESTART',
      'then',
      'with',
      'all',
      'of',
      'the',
      'Committee',
      'Hearings',
      'Investigations',
      'and',
      'Party',
      'hatred',
      'they',
      'have',
      'succeeded',
      'beyond',
      'their',
      'wildest',
      'dreams',
      'SENTENCEEND',
      'SENTENCESTART',
      'They',
      'are',
      'laughing',
      'their',
      ...],
     ['NEXTPOST',
      'SENTENCESTART',
      'Today',
      'Kehinde',
      'Wiley',
      'and',
      'Amy',
      'Sherald',
      'became',
      'the',
      'first',
      'black',
      'artists',
      'to',
      'create',
      'official',
      'presidential',
      'portraits',
      'for',
      'the',
      'Smithsonian',
      'SENTENCEEND',
      'SENTENCESTART',
      'To',
      'call',
      'this',
      'experience',
      'humbling',
      'would',
      'be',
      'an',
      'understatement',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'Thanks',
      'to',
      'Kehinde',
      'and',
      'Amy',
      'generations',
      'of',
      'Americans',
      '—',
      'and',
      'young',
      'people',
      'from',
      'all',
      'around',
      'the',
      'world',
      '—',
      'will',
      'visit',
      'the',
      'National',
      'Portrait',
      'Gallery',
      'and',
      'see',
      'this',
      'country',
      'through',
      'a',
      'new',
      'lens',
      'SENTENCEEND',
      'SENTENCESTART',
      'They’ll',
      'walk',
      'out',
      'of',
      'that',
      'museum',
      'with',
      'a',
      'better',
      'sense',
      'of',
      'the',
      'America',
      'we',
      'all',
      'love',
      'SENTENCEEND',
      'SENTENCESTART',
      'Clear',
      'eyed',
      'SENTENCEEND',
      'SENTENCESTART',
      'Big',
      'hearted',
      'SENTENCEEND',
      'SENTENCESTART',
      'Inclusive',
      'and',
      'optimistic',
      'SENTENCEEND',
      'SENTENCESTART',
      'And',
      'I',
      'hope',
      'they’ll',
      'walk',
      'out',
      'more',
      'empowered',
      'to',
      'go',
      'and',
      'change',
      'their',
      'worlds',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'This',
      'month',
      'we',
      'tell',
      'the',
      'stories',
      'of',
      'a',
      'special',
      'set',
      'of',
      'heroes',
      'The',
      'African',
      'American',
      'community',
      'organizers',
      'and',
      'active',
      'citizens',
      'who',
      'have',
      'throughout',
      'our',
      'history',
      'brought',
      'people',
      'together',
      'in',
      'pursuit',
      'of',
      'a',
      'more',
      'just',
      'inclusive',
      'and',
      'generous',
      'America',
      'SENTENCEEND',
      'SENTENCESTART',
      'The',
      'pioneers',
      'who',
      'challenged',
      'us',
      'to',
      'dream',
      'bigger',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'C',
      'SENTENCEEND',
      'SENTENCESTART',
      'T',
      'SENTENCEEND',
      'SENTENCESTART',
      'Vivian',
      'is',
      'one',
      'of',
      'those',
      'heroes',
      'SENTENCEEND',
      'SENTENCESTART',
      'A',
      'Baptist',
      'minister',
      'C',
      'SENTENCEEND',
      'SENTENCESTART',
      'T',
      'SENTENCEEND',
      'SENTENCESTART',
      'Vivian',
      'was',
      'one',
      'of',
      'Dr',
      'SENTENCEEND',
      'SENTENCESTART',
      'King’s',
      'closest',
      'advisors',
      'SENTENCEEND',
      'SENTENCESTART',
      '“Martin',
      'taught',
      'us',
      '”',
      'he',
      'says',
      '“that',
      'it’s',
      'in',
      'the',
      'action',
      'that',
      'we',
      'find',
      'out',
      'who',
      'we',
      'really',
      'are',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'And',
      'time',
      'and',
      'again',
      'Reverend',
      'Vivian',
      'was',
      'among',
      'the',
      'first',
      'to',
      'be',
      'in',
      'the',
      'action',
      'SENTENCEEND',
      'SENTENCESTART',
      'In',
      '1947',
      'he',
      'joined',
      'a',
      'sit',
      'in',
      'to',
      'integrate',
      'an',
      'Illinois',
      'restaurant',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'was',
      'one',
      'of',
      'the',
      'first',
      'Freedom',
      'Riders',
      'SENTENCEEND',
      'SENTENCESTART',
      'In',
      'Selma',
      'he',
      'stood',
      'on',
      'the',
      'courthouse',
      'steps',
      'to',
      'register',
      'blacks',
      'to',
      'vote',
      'SEPARATOR',
      'and',
      'was',
      'beaten',
      'bloodied',
      'and',
      'jailed',
      'for',
      'it',
      'SENTENCEEND',
      'SENTENCESTART',
      'In',
      'standing',
      'up',
      'he',
      'helped',
      'inspire',
      'a',
      'young',
      'woman',
      'to',
      'sit',
      'down',
      'and',
      'hold',
      'her',
      'ground',
      'SENTENCEEND',
      'SENTENCESTART',
      'Her',
      'name',
      'was',
      'Rosa',
      'Parks',
      'SENTENCEEND',
      'SENTENCESTART',
      'She',
      'said',
      'of',
      'him',
      '“Even',
      'after',
      'things',
      'had',
      'supposedly',
      'been',
      'taken',
      'care',
      'of',
      'and',
      'we',
      'had',
      'our',
      'rights',
      'he',
      'was',
      'still',
      'out',
      'there',
      'inspiring',
      'the',
      'next',
      'generation',
      'including',
      'me',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'NEWLINE',
      'NEWLINE',
      'And',
      'me',
      'SENTENCEEND',
      'SENTENCESTART',
      'Without',
      'the',
      'sacrifices',
      'and',
      'hard',
      'won',
      'battles',
      'of',
      'folks',
      'like',
      'C',
      'SENTENCEEND',
      'SENTENCESTART',
      'T',
      'SENTENCEEND',
      'SENTENCESTART',
      'Vivian',
      'I',
      'may',
      'have',
      'never',
      'had',
      'the',
      'honor',
      'to',
      'serve',
      'as',
      'your',
      'president',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'And',
      'that’s',
      'the',
      'beauty',
      'of',
      'this',
      'nation',
      'Each',
      'new',
      'generation',
      'stands',
      'on',
      'the',
      'successes',
      'of',
      'the',
      'last',
      'SEPARATOR',
      'and',
      'reaches',
      'up',
      'to',
      'bend',
      'the',
      'arc',
      'of',
      'history',
      'in',
      'the',
      'direction',
      'of',
      'more',
      'freedom',
      'more',
      'opportunity',
      'more',
      'justice',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'As',
      'we',
      'continue',
      'to',
      'reflect',
      'on',
      'the',
      'sacrifices',
      'and',
      'contributions',
      'made',
      'by',
      'generations',
      'of',
      'African',
      'Americans',
      'this',
      'month',
      'let',
      'us',
      'resolve',
      'to',
      'continue',
      'our',
      'march',
      'toward',
      'a',
      'day',
      'when',
      'every',
      'person',
      'knows',
      'the',
      'unalienable',
      'rights',
      'to',
      'life',
      'liberty',
      'and',
      'the',
      'pursuit',
      'of',
      'happiness',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Over',
      'the',
      'course',
      'of',
      'the',
      'past',
      'year',
      'Michelle',
      'and',
      'I',
      'have',
      'been',
      'working',
      'with',
      'an',
      'extraordinary',
      'team',
      'to',
      'dream',
      'up',
      'a',
      'campus',
      'for',
      'active',
      'citizenship',
      'on',
      'Chicago’s',
      'South',
      'Side',
      'The',
      'Obama',
      'Presidential',
      'Center',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'Today',
      'we’re',
      'taking',
      'a',
      'significant',
      'step',
      'forward',
      'and',
      'I',
      'wanted',
      'to',
      'share',
      'the',
      'very',
      'latest',
      'Obama',
      'SENTENCEEND',
      'SENTENCESTART',
      'org',
      'The',
      'Center',
      'NEXTPOST',
      'SENTENCESTART',
      'During',
      'my',
      'presidency',
      'I',
      'started',
      'a',
      'tradition',
      'of',
      'sharing',
      'my',
      'reading',
      'lists',
      'and',
      'playlists',
      'SENTENCEEND',
      'SENTENCESTART',
      'It',
      'was',
      'a',
      'nice',
      'way',
      'to',
      'reflect',
      'on',
      'the',
      'works',
      'that',
      'resonated',
      'with',
      'me',
      'and',
      'lift',
      'up',
      'authors',
      'and',
      'artists',
      'from',
      'around',
      'the',
      'world',
      'SENTENCEEND',
      'SENTENCESTART',
      'With',
      'some',
      'extra',
      'time',
      'on',
      'my',
      'hands',
      'this',
      'year',
      'to',
      'catch',
      'up',
      'I',
      'wanted',
      'to',
      'share',
      'the',
      'books',
      'and',
      'music',
      'that',
      'I',
      'enjoyed',
      'most',
      'SENTENCEEND',
      'SENTENCESTART',
      'From',
      'songs',
      'that',
      'got',
      'me',
      'moving',
      'to',
      'stories',
      'that',
      'inspired',
      'me',
      "here's",
      'my',
      '2017',
      'list',
      '—',
      'I',
      'hope',
      'you',
      'enjoy',
      'it',
      'and',
      'have',
      'a',
      'happy',
      'and',
      'healthy',
      'New',
      'Year',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'The',
      'best',
      'books',
      'I',
      'read',
      'in',
      '2017',
      'NEWLINE',
      'The',
      'Power',
      'by',
      'Naomi',
      'Alderman',
      'NEWLINE',
      'Grant',
      'by',
      'Ron',
      'Chernow',
      'NEWLINE',
      'Evicted',
      'Poverty',
      'and',
      'Profit',
      'in',
      'the',
      'American',
      'City',
      'by',
      'Matthew',
      'Desmond',
      'NEWLINE',
      'Janesville',
      'An',
      'American',
      'Story',
      'by',
      'Amy',
      'Goldstein',
      'NEWLINE',
      'Exit',
      'West',
      'by',
      'Mohsin',
      'Hamid',
      'NEWLINE',
      'Five',
      'Carat',
      'Soul',
      'by',
      'James',
      'McBride',
      'NEWLINE',
      'Anything',
      'Is',
      'Possible',
      'by',
      'Elizabeth',
      'Strout',
      'NEWLINE',
      'Dying',
      'A',
      'Memoir',
      'by',
      'Cory',
      'Taylor',
      'NEWLINE',
      'A',
      'Gentleman',
      'in',
      'Moscow',
      'by',
      'Amor',
      'Towles',
      'NEWLINE',
      'Sing',
      'Unburied',
      'Sing',
      'by',
      'Jesmyn',
      'Ward',
      'NEWLINE',
      'Bonus',
      'for',
      'hoops',
      'fans',
      'Coach',
      'Wooden',
      'and',
      'Me',
      'by',
      'Kareem',
      'Abdul',
      'Jabbar',
      'and',
      'Basketball',
      'and',
      'Other',
      'Things',
      'by',
      'Shea',
      'Serrano',
      'NEWLINE',
      'NEWLINE',
      'My',
      'favorite',
      'songs',
      'of',
      '2017',
      'NEWLINE',
      'Mi',
      'Gente',
      'by',
      'J',
      'Balvin',
      'Willy',
      'William',
      'NEWLINE',
      'Havana',
      'by',
      'Camila',
      'Cabello',
      'feat',
      'SENTENCEEND',
      'SENTENCESTART',
      'Young',
      'Thug',
      'NEWLINE',
      'Blessed',
      'by',
      'Daniel',
      'Caesar',
      'NEWLINE',
      'The',
      'Joke',
      'by',
      'Brandi',
      'Carlile',
      'NEWLINE',
      'First',
      'World',
      'Problems',
      'by',
      'Chance',
      'The',
      'Rapper',
      'feat',
      'SENTENCEEND',
      'SENTENCESTART',
      'Daniel',
      'Caesar',
      'NEWLINE',
      'Rise',
      'Up',
      'by',
      'Andra',
      'Day',
      'NEWLINE',
      'Wild',
      'Thoughts',
      'by',
      'DJ',
      'Khaled',
      'feat',
      'SENTENCEEND',
      'SENTENCESTART',
      'Rihanna',
      'and',
      'Bryson',
      'Tiller',
      'NEWLINE',
      'Family',
      'Feud',
      'by',
      'Jay',
      'Z',
      'feat',
      'SENTENCEEND',
      'SENTENCESTART',
      'Beyoncé',
      'NEWLINE',
      'Humble',
      'by',
      'Kendrick',
      'Lamar',
      'NEWLINE',
      'La',
      'Dame',
      'et',
      'Ses',
      'Valises',
      'by',
      'Les',
      'Amazones',
      'd’Afrique',
      'feat',
      'SENTENCEEND',
      'SENTENCESTART',
      'Nneka',
      'NEWLINE',
      'Unforgettable',
      'by',
      'French',
      'Montana',
      'feat',
      'SENTENCEEND',
      'SENTENCESTART',
      'Swae',
      'Lee',
      'NEWLINE',
      'The',
      'System',
      'Only',
      'Dreams',
      'in',
      'Total',
      'Darkness',
      'by',
      'The',
      'National',
      'NEWLINE',
      'Chanel',
      'by',
      'Frank',
      'Ocean',
      'NEWLINE',
      'Feel',
      'It',
      'Still',
      'by',
      'Portugal',
      'SENTENCEEND',
      'SENTENCESTART',
      'The',
      'Man',
      'NEWLINE',
      'Butterfly',
      'Effect',
      'by',
      'Travis',
      'Scott',
      'NEWLINE',
      'Matter',
      'of',
      'Time',
      'by',
      'Sharon',
      'Jones',
      'the',
      'Dap',
      'Kings',
      'NEWLINE',
      'Little',
      'Bit',
      'by',
      'Mavis',
      'Staples',
      'NEWLINE',
      'Millionaire',
      'by',
      'Chris',
      'Stapleton',
      'NEWLINE',
      'Sign',
      'of',
      'the',
      'Times',
      'by',
      'Harry',
      'Styles',
      'NEWLINE',
      'Broken',
      'Clocks',
      'by',
      'SZA',
      'NEWLINE',
      'Ordinary',
      'Love',
      'Extraordinary',
      'Mix',
      'by',
      'U2',
      'NEWLINE',
      'Bonus',
      'Born',
      'in',
      'the',
      'U',
      'SENTENCEEND',
      'SENTENCESTART',
      'S',
      'SENTENCEEND',
      'SENTENCESTART',
      'A',
      'SENTENCEEND',
      'SENTENCESTART',
      'by',
      'Bruce',
      'Springsteen',
      'not',
      'out',
      'yet',
      'but',
      'the',
      'blues',
      'version',
      'in',
      'his',
      'Broadway',
      'show',
      'is',
      'the',
      'best',
      'NEXTPOST',
      'SENTENCESTART',
      'On',
      'behalf',
      'of',
      'the',
      'Obama',
      'family',
      'Merry',
      'Christmas',
      'We',
      'wish',
      'you',
      'joy',
      'and',
      'peace',
      'this',
      'holiday',
      'season',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Wearing',
      'a',
      'Santa',
      'hat',
      'helps',
      'too',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'I',
      'just',
      'got',
      'off',
      'a',
      'call',
      'to',
      'say',
      'thanks',
      'to',
      'folks',
      'who',
      'are',
      'working',
      'hard',
      'to',
      'help',
      ...],
     ['NEXTPOST',
      'SENTENCESTART',
      '“I',
      'feel',
      'like',
      'God',
      'has',
      'given',
      'me',
      'a',
      'talent',
      'for',
      'art',
      'and',
      'using',
      'that',
      'talent',
      'is',
      'a',
      'form',
      'of',
      'gratitude',
      'SENTENCEEND',
      'SENTENCESTART',
      'For',
      'years',
      'I',
      'worked',
      'in',
      'an',
      'automotive',
      'workshop',
      'SENTENCEEND',
      'SENTENCESTART',
      'I’d',
      'try',
      'to',
      'paint',
      'when',
      'I',
      'came',
      'home',
      'but',
      'I',
      'never',
      'had',
      'any',
      'energy',
      'left',
      'SENTENCEEND',
      'SENTENCESTART',
      'So',
      'I',
      'finally',
      'quit',
      'SENTENCEEND',
      'SENTENCESTART',
      'Now',
      'I',
      'have',
      'more',
      'freedom',
      'to',
      'develop',
      'my',
      'talents',
      'but',
      'it',
      'hasn’t',
      'been',
      'easy',
      'SENTENCEEND',
      'SENTENCESTART',
      'Art',
      'is',
      'more',
      'difficult',
      'in',
      'this',
      'country',
      'SENTENCEEND',
      'SENTENCESTART',
      'It’s',
      'not',
      'really',
      'seen',
      'as',
      'a',
      'profession',
      'SENTENCEEND',
      'SENTENCESTART',
      'It',
      'would',
      'be',
      'easier',
      'if',
      'I',
      'was',
      'alone',
      'but',
      'I',
      'have',
      'a',
      'family',
      'to',
      'support',
      'so',
      'there’s',
      'a',
      'lot',
      'of',
      'pressure',
      'to',
      'earn',
      'SENTENCEEND',
      'SENTENCESTART',
      'I’m',
      'trying',
      'to',
      'be',
      'tough',
      'in',
      'poverty',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'read',
      'a',
      'lot',
      'of',
      'poetry',
      'to',
      'give',
      'me',
      'strength',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'would',
      'never',
      'beg',
      'so',
      'right',
      'now',
      'I’m',
      'drawing',
      'portraits',
      'and',
      'caricatures',
      'to',
      'support',
      'myself',
      'SENTENCEEND',
      'SENTENCESTART',
      'I’d',
      'rather',
      'be',
      'painting',
      'what',
      'I',
      'think',
      'and',
      'feel',
      'but',
      'I',
      'have',
      'to',
      'fulfill',
      'what',
      'the',
      'consumer',
      'wants',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'NEWLINE',
      'NEWLINE',
      'Jakarta',
      'Indonesia',
      'NEXTPOST',
      'SENTENCESTART',
      '“Mom',
      'raised',
      'me',
      'on',
      'her',
      'own',
      'since',
      'I',
      'was',
      'young',
      'SENTENCEEND',
      'SENTENCESTART',
      'My',
      'dad',
      'didn’t',
      'care',
      'about',
      'our',
      'family',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'was',
      'there',
      'physically',
      'but',
      'he',
      'wasn’t',
      'there',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'kept',
      'all',
      'his',
      'income',
      'for',
      'himself',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'gambled',
      'away',
      'our',
      'possessions',
      'SENTENCEEND',
      'SENTENCESTART',
      'We',
      'couldn’t',
      'buy',
      'anything',
      'for',
      'ourselves',
      'because',
      'he',
      'would',
      'sell',
      'it',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'sold',
      'all',
      'our',
      'electronics',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'even',
      'sold',
      'our',
      'furniture',
      'SENTENCEEND',
      'SENTENCESTART',
      'But',
      'my',
      'mother',
      'still',
      'did',
      'everything',
      'for',
      'him',
      'cooked',
      'his',
      'food',
      'did',
      'his',
      'laundry…',
      'everything',
      'SENTENCEEND',
      'SENTENCESTART',
      'Then',
      'one',
      'day',
      'she',
      'came',
      'home',
      'from',
      'work',
      'and',
      'the',
      'house',
      'was',
      'empty',
      'SENTENCEEND',
      'SENTENCESTART',
      'There',
      'was',
      'nothing',
      'left',
      'but',
      'our',
      'clothes',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'told',
      'her',
      'it',
      'was',
      'time',
      'to',
      'go',
      'SENTENCEEND',
      'SENTENCESTART',
      'For',
      'the',
      'last',
      'two',
      'years',
      'we’ve',
      'been',
      'living',
      'in',
      'a',
      'rented',
      'house',
      'together',
      'SENTENCEEND',
      'SENTENCESTART',
      'Mom',
      'seems',
      'so',
      'much',
      'lighter',
      'now',
      'SENTENCEEND',
      'SENTENCESTART',
      'We',
      'can',
      'own',
      'our',
      'own',
      'things',
      'SENTENCEEND',
      'SENTENCESTART',
      'We',
      'can',
      'actually',
      'make',
      'progress',
      'and',
      'move',
      'forward',
      'in',
      'life',
      'SENTENCEEND',
      'SENTENCESTART',
      "I've",
      'started',
      'working',
      'at',
      'a',
      'pharmacy',
      'and',
      "I'm",
      'paying',
      'my',
      'way',
      'through',
      'college',
      'SENTENCEEND',
      'SENTENCESTART',
      'I’m',
      'still',
      'in',
      'touch',
      'with',
      'my',
      'father',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'even',
      'give',
      'him',
      'money',
      'for',
      'rent',
      'and',
      'food',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'have',
      'no',
      'interest',
      'in',
      'taking',
      'revenge',
      'SENTENCEEND',
      'SENTENCESTART',
      'I’m',
      'showing',
      'him',
      'how',
      'he',
      'should',
      'have',
      'treated',
      'us',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'NEWLINE',
      'NEWLINE',
      'Jakarta',
      'Indonesia',
      'NEXTPOST',
      'SENTENCESTART',
      '“I’m',
      'a',
      'fund',
      'manager',
      'SENTENCEEND',
      'SENTENCESTART',
      'One',
      'of',
      'my',
      'employees',
      'stole',
      'millions',
      'from',
      'my',
      'clients',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'hid',
      'it',
      'from',
      'me',
      'for',
      'almost',
      'two',
      'years',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'was',
      'working',
      'with',
      'a',
      'broker',
      'to',
      'create',
      'fake',
      'transactions',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'disappeared',
      'when',
      'I',
      'discovered',
      'the',
      'scheme',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'could',
      'call',
      'the',
      'police',
      'but',
      'it',
      'would',
      'be',
      'all',
      'over',
      'the',
      'press',
      'and',
      'I’d',
      'never',
      'get',
      'another',
      'client',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'could',
      'declare',
      'bankruptcy',
      'but',
      'I’m',
      'not',
      'a',
      'coward',
      'SENTENCEEND',
      'SENTENCESTART',
      'So',
      'I’m',
      'trying',
      'to',
      'pay',
      'my',
      'clients',
      'back',
      'the',
      'initial',
      'amount',
      'they',
      'invested',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'sold',
      'my',
      'other',
      'business',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'sold',
      'two',
      'houses',
      'and',
      'a',
      'car',
      'SENTENCEEND',
      'SENTENCESTART',
      'That’s',
      'why',
      'I’m',
      'taking',
      'trains',
      'and',
      'online',
      'taxis',
      'SENTENCEEND',
      'SENTENCESTART',
      'Some',
      'of',
      'my',
      'clients',
      'feel',
      'sorry',
      'for',
      'me',
      'SENTENCEEND',
      'SENTENCESTART',
      'Most',
      'of',
      'them',
      'are',
      'angry',
      'SENTENCEEND',
      'SENTENCESTART',
      'A',
      'few',
      'hate',
      'me',
      'SENTENCEEND',
      'SENTENCESTART',
      'All',
      'of',
      'them',
      'keep',
      'calling',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'tell',
      'them',
      'to',
      'give',
      'me',
      'time',
      'but',
      'they',
      'keep',
      'calling',
      'SENTENCEEND',
      'SENTENCESTART',
      'I’m',
      'smoking',
      'four',
      'packs',
      'of',
      'cigarettes',
      'a',
      'day',
      'because',
      'of',
      'the',
      'stress',
      'SENTENCEEND',
      'SENTENCESTART',
      'I’ve',
      'got',
      'to',
      'find',
      'fresh',
      'money',
      'SENTENCEEND',
      'SENTENCESTART',
      'If',
      'I',
      'can',
      'get',
      'new',
      'clients',
      'then',
      'I',
      'can',
      'slowly',
      'repay',
      'the',
      'old',
      'ones',
      'with',
      'the',
      'profits',
      'that',
      'I',
      'make',
      'SENTENCEEND',
      'SENTENCESTART',
      'But',
      'all',
      'of',
      'them',
      'want',
      'their',
      'money',
      'now',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'NEWLINE',
      'NEWLINE',
      'Jakarta',
      'Indonesia',
      'NEXTPOST',
      'SENTENCESTART',
      '“Several',
      'years',
      'ago',
      'my',
      'father',
      'used',
      'our',
      'house',
      'for',
      'collateral',
      'on',
      'a',
      'business',
      'loan',
      'SENTENCEEND',
      'SENTENCESTART',
      'When',
      'things',
      'fell',
      'apart',
      'we',
      'lost',
      'everything',
      'SENTENCEEND',
      'SENTENCESTART',
      'My',
      'dad',
      'got',
      'depressed',
      'and',
      'fell',
      'sick',
      'SENTENCEEND',
      'SENTENCESTART',
      'My',
      'mother',
      'was',
      'backed',
      'into',
      'a',
      'corner',
      'SENTENCEEND',
      'SENTENCESTART',
      'She',
      'had',
      'four',
      'young',
      'children',
      'but',
      'no',
      'income',
      'SENTENCEEND',
      'SENTENCESTART',
      'At',
      'the',
      'time',
      'she',
      'was',
      'a',
      'very',
      'feminine',
      'housewife',
      'SENTENCEEND',
      'SENTENCESTART',
      'But',
      'she',
      'asked',
      'our',
      'neighbors',
      'to',
      'teach',
      'her',
      'how',
      'to',
      'farm',
      'SENTENCEEND',
      'SENTENCESTART',
      'Then',
      'she',
      'convinced',
      'our',
      'landlord',
      'to',
      'let',
      'her',
      'plant',
      'an',
      'empty',
      'field',
      'SENTENCEEND',
      'SENTENCESTART',
      'He',
      'told',
      'her',
      '‘If',
      'you',
      'can',
      'make',
      'it',
      'yield',
      'anything',
      'we’ll',
      'split',
      'the',
      'profits',
      'SENTENCEEND',
      'SENTENCESTART',
      '’',
      'It',
      'didn’t',
      'succeed',
      'right',
      'away',
      'SENTENCEEND',
      'SENTENCESTART',
      'There',
      'were',
      'a',
      'lot',
      'of',
      'pests',
      'SENTENCEEND',
      'SENTENCESTART',
      'Some',
      'of',
      'the',
      'crops',
      'dried',
      'out',
      'SENTENCEEND',
      'SENTENCESTART',
      'But',
      'my',
      'mother',
      'harvested',
      'whatever',
      'was',
      'left',
      'and',
      'it',
      'was',
      'enough',
      'to',
      'support',
      'our',
      'family',
      'SENTENCEEND',
      'SENTENCESTART',
      'She',
      'cut',
      'off',
      'her',
      'hair',
      'during',
      'this',
      'time',
      'SENTENCEEND',
      'SENTENCESTART',
      'She',
      'got',
      'very',
      'muscular',
      'SENTENCEEND',
      'SENTENCESTART',
      'She',
      'worked',
      'all',
      'day',
      'in',
      'the',
      'fields',
      'but',
      'still',
      'found',
      'time',
      'to',
      'make',
      'sure',
      'we',
      'went',
      'to',
      'school',
      'SENTENCEEND',
      'SENTENCESTART',
      'If',
      'my',
      'brothers',
      'and',
      'I',
      'ever',
      'started',
      'crying',
      'she’d',
      'tell',
      'us',
      '‘If',
      'I’m',
      'strong',
      'enough',
      'to',
      'do',
      'this',
      'you’re',
      'strong',
      'enough',
      'to',
      'go',
      'to',
      'school',
      'SENTENCEEND',
      'SENTENCESTART',
      '’”',
      'NEWLINE',
      'NEWLINE',
      'Jakarta',
      'Indonesia',
      'NEXTPOST',
      'SENTENCESTART',
      '“I',
      'tried',
      'to',
      'pick',
      'my',
      'wife',
      'up',
      'from',
      'her',
      'religious',
      'group',
      'one',
      'night',
      'but',
      'her',
      'friends',
      'told',
      'me',
      'that',
      'she’d',
      'left',
      'with',
      'someone',
      'else',
      'SENTENCEEND',
      'SENTENCESTART',
      'She',
      'told',
      'me',
      'the',
      'guy',
      'was',
      'just',
      'a',
      'friend',
      'SENTENCEEND',
      'SENTENCESTART',
      'But',
      'over',
      'the',
      'next',
      'few',
      'months',
      'I',
      'started',
      'to',
      'hear',
      'more',
      'and',
      'more',
      'things',
      'SENTENCEEND',
      'SENTENCESTART',
      'Somebody',
      'saw',
      'her',
      'coming',
      'out',
      'of',
      'a',
      'hotel',
      'with',
      'another',
      'man',
      'SENTENCEEND',
      'SENTENCESTART',
      'Then',
      'a',
      'few',
      'weeks',
      'later',
      'I',
      'discovered',
      'them',
      'on',
      'a',
      'beach',
      'together',
      'SENTENCEEND',
      'SENTENCESTART',
      'Then',
      'finally',
      'I',
      'came',
      'home',
      'early',
      'one',
      'day',
      'and',
      'found',
      'them',
      'in',
      'my',
      'house',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'was',
      'heartbroken',
      'SENTENCEEND',
      'SENTENCESTART',
      'Because',
      'I',
      'really',
      'did',
      'love',
      'her',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'left',
      'town',
      'immediately',
      'after',
      'the',
      'divorce',
      'because',
      'I',
      'didn’t',
      'want',
      'to',
      'be',
      'reminded',
      'of',
      'the',
      'heartbreak',
      'SENTENCEEND',
      'SENTENCESTART',
      'I’ve',
      'lived',
      'in',
      'Jakarta',
      'for',
      'three',
      'years',
      'now',
      'SENTENCEEND',
      'SENTENCESTART',
      'I',
      'have',
      'my',
      'own',
      'shop',
      'SENTENCEEND',
      'SENTENCESTART',
      ...],
     ['NEXTPOST',
      'SENTENCESTART',
      'Firefly',
      'a',
      'new',
      'microscope',
      'from',
      "Harvard's",
      'Cohen',
      'Lab',
      'can',
      'show',
      'how',
      'neurons',
      'pulse',
      'communicate',
      'and',
      'shine',
      'SENTENCEEND',
      'SENTENCESTART',
      'The',
      "microscope's",
      'large',
      'field',
      'of',
      'view',
      'and',
      'fast',
      'imaging',
      'capability',
      'allows',
      'it',
      'to',
      'image',
      'electrical',
      'signals',
      'quickly',
      'traveling',
      'from',
      'neuron',
      'to',
      'neuron',
      'SENTENCEEND',
      'SENTENCESTART',
      '⠀',
      'NEWLINE',
      '⠀',
      'NEWLINE',
      'The',
      'images',
      'could',
      'illuminate',
      'how',
      'disorders',
      'like',
      'epilepsy',
      'and',
      'Alzheimer’s',
      'disease',
      'affect',
      'neuron',
      'communication',
      'and',
      'thus',
      'enable',
      'researchers',
      'to',
      'discover',
      'how',
      'to',
      'prevent',
      'and',
      'treat',
      'a',
      'host',
      'of',
      'neurological',
      'diseases',
      'SENTENCEEND',
      'SENTENCESTART',
      '⠀',
      'NEWLINE',
      '⠀',
      'NEWLINE',
      'Here',
      'each',
      'bright',
      'dot',
      'represents',
      'one',
      'neuron',
      'in',
      'the',
      'brain',
      'of',
      'a',
      'genetically',
      'modified',
      'mouse',
      'SENTENCEEND',
      'SENTENCESTART',
      '⠀',
      'NEWLINE',
      '⠀',
      'NEWLINE',
      'Credit',
      'Vaibhav',
      'Joshi',
      'NEXTPOST',
      'SENTENCESTART',
      'At',
      'Harvard',
      'Business',
      "School's",
      'Crossover',
      'into',
      'Business',
      'program',
      'athletes',
      'from',
      'the',
      'WNBA',
      'NBA',
      'and',
      'NFL',
      'worked',
      'with',
      'their',
      'M',
      'SENTENCEEND',
      'SENTENCESTART',
      'B',
      'SENTENCEEND',
      'SENTENCESTART',
      'A',
      'SENTENCEEND',
      'SENTENCESTART',
      'student',
      'mentors',
      'discussing',
      'the',
      'LeBron',
      'James',
      'case',
      'and',
      'endorsement',
      'opportunities',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      '73°F',
      'in',
      '360°',
      'NEXTPOST',
      'SENTENCESTART',
      'Inside',
      'the',
      'Harvard',
      'Semitic',
      'Museum',
      'a',
      'new',
      'exhibit',
      'blends',
      'important',
      'artifacts',
      'with',
      'immersive',
      'technology',
      'SENTENCEEND',
      'SENTENCESTART',
      'Take',
      'a',
      'look',
      'NEXTPOST',
      'SENTENCESTART',
      'Beautiful',
      'weather',
      'has',
      'arrived',
      'David',
      'Chang',
      "'17",
      'rescues',
      'Waverly',
      'He',
      "'18",
      'from',
      'being',
      'holed',
      'up',
      'inside',
      'the',
      'Northwest',
      'Building',
      'writing',
      'her',
      'thesis',
      'on',
      'an',
      'unseasonably',
      'warm',
      'February',
      'day',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'Oh',
      'and',
      "that's",
      'a',
      '21',
      'foot',
      'killer',
      'whale',
      'skeleton',
      'in',
      'the',
      'background',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'Photo',
      'Rose',
      'Lincoln',
      'Harvard',
      'Staff',
      'Photographer',
      'NEXTPOST',
      'SENTENCESTART',
      'If',
      'you',
      'see',
      'this',
      'little',
      'robot',
      'scurrying',
      'down',
      'the',
      'hall',
      'at',
      'Harvard',
      'try',
      'not',
      'to',
      'squish',
      'it',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Happy',
      "Presidents'",
      'Day',
      'Did',
      'you',
      'know',
      'there',
      'are',
      'eight',
      'U',
      'SENTENCEEND',
      'SENTENCESTART',
      'S',
      'SENTENCEEND',
      'SENTENCESTART',
      'presidents',
      'with',
      'Harvard',
      'degrees',
      'NEXTPOST',
      'SENTENCESTART',
      'Thanks',
      'to',
      'a',
      'breakthrough',
      'gift',
      'from',
      'Susan',
      'Shallcross',
      'Swartz',
      'and',
      'her',
      'husband',
      'James',
      'R',
      'SENTENCEEND',
      'SENTENCESTART',
      'Swartz',
      '’64',
      'Andover',
      'Hall',
      'will',
      'undergo',
      'a',
      'renewal',
      'its',
      'first',
      'since',
      'construction',
      'more',
      'than',
      '100',
      'years',
      'ago',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Department',
      'chair',
      'Daniel',
      'Lord',
      'Smail',
      'lauded',
      'the',
      'Dakota',
      'descendant',
      'as',
      '“hands',
      'down',
      'the',
      'leading',
      'authority',
      'in',
      'Native',
      'American',
      'history',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Your',
      'physician',
      'may',
      'have',
      'less',
      'of',
      'a',
      'role',
      'in',
      'day',
      'to',
      'day',
      'well',
      'being',
      'than',
      'the',
      'facilities',
      'manager',
      'where',
      'you',
      'work',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Solange',
      'Knowles',
      'will',
      'be',
      'recognized',
      'at',
      "Harvard's",
      'annual',
      'award',
      'ceremony',
      'on',
      'March',
      '3',
      'SENTENCEEND',
      'SENTENCESTART',
      'Congratulations',
      'NEXTPOST',
      'SENTENCESTART',
      'Master',
      'potter',
      'Ben',
      'Owen',
      'III',
      'demonstrated',
      'his',
      'throwing',
      'process',
      'and',
      'discussed',
      'his',
      'work',
      'at',
      'a',
      'recent',
      'workshop',
      'at',
      'the',
      'Harvard',
      'Ceramics',
      'Building',
      'in',
      'Allston',
      'SENTENCEEND',
      'SENTENCESTART',
      'http',
      'hrvd',
      'SENTENCEEND',
      'SENTENCESTART',
      'me',
      'maste6cc8',
      'NEXTPOST',
      'SENTENCESTART',
      'Has',
      'this',
      'flu',
      'season',
      'peaked',
      'A',
      'panel',
      'of',
      'experts',
      'discussed',
      'the',
      'flu',
      'vaccination',
      'and',
      'future',
      'research',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'The',
      'papers',
      'of',
      'famed',
      'activist',
      'Angela',
      'Davis',
      'are',
      'now',
      'part',
      'of',
      'Radcliffe’s',
      'Schlesinger',
      'Library',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'When',
      'Harvard',
      'student',
      'Kaitlyn',
      'was',
      'in',
      'middle',
      'school',
      'her',
      'mother',
      'made',
      'this',
      "Valentine's",
      'Day',
      'sweater',
      'out',
      'of',
      'a',
      'dish',
      'towel',
      "She's",
      'worn',
      'it',
      'every',
      "Valentine's",
      'Day',
      'since',
      'SENTENCEEND',
      'SENTENCESTART',
      '❤️',
      'NEWLINE',
      'NEWLINE',
      'Photo',
      'Rose',
      'Lincoln',
      'Harvard',
      'Staff',
      'Photographer',
      'NEXTPOST',
      'SENTENCESTART',
      'These',
      'Harvard',
      'professors',
      'study',
      'the',
      'science',
      'of',
      'love',
      'SENTENCEEND',
      'SENTENCESTART',
      'Happy',
      "Valentine's",
      'Day',
      'NEXTPOST',
      'SENTENCESTART',
      'A',
      'record',
      '42',
      '742',
      'students',
      'applied',
      'for',
      'admission',
      'to',
      'Harvard’s',
      'Class',
      'of',
      '2022',
      'breaking',
      'last',
      'year’s',
      'record',
      'of',
      '39',
      '506',
      'for',
      'the',
      'current',
      'freshman',
      'class',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'What',
      'you',
      'need',
      'to',
      'know',
      'about',
      'the',
      'flu',
      'experts',
      'discuss',
      'the',
      'flu',
      'season',
      'prevention',
      'and',
      'treatment',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      "Here's",
      'what',
      'our',
      'community',
      '—',
      'from',
      'deans',
      'to',
      'faculty',
      'to',
      'students',
      '—',
      'said',
      'about',
      "Harvard's",
      '29th',
      'president',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Ryan',
      'Donato',
      "'19",
      'is',
      'part',
      'of',
      'the',
      'U',
      'SENTENCEEND',
      'SENTENCESTART',
      'S',
      'SENTENCEEND',
      'SENTENCESTART',
      'Olympic',
      'Men’s',
      'Ice',
      'Hockey',
      'Team',
      'competing',
      'in',
      'South',
      'Korea',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      '“I',
      'really',
      'see',
      'this',
      'as',
      'an',
      'opportunity',
      'to',
      'not',
      'just',
      'serve',
      'Harvard',
      'but',
      'at',
      'this',
      'particular',
      'moment',
      'in',
      'time',
      'to',
      'serve',
      'higher',
      'education',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'NEXTPOST',
      'SENTENCESTART',
      'Today',
      'we',
      'announce',
      "Harvard's",
      '29th',
      'president',
      'NEXTPOST',
      'SENTENCESTART',
      'On',
      'their',
      'quest',
      'to',
      'restore',
      'hearing',
      'through',
      'gene',
      'therapy',
      'scientists',
      'have',
      'long',
      'sought',
      'ways',
      'to',
      'improve',
      'gene',
      'delivery',
      'into',
      'hair',
      'cells',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'The',
      'free',
      'online',
      'HarvardX',
      'course',
      '“Super',
      'Earths',
      'and',
      'Life”',
      'uses',
      'adaptive',
      'learning',
      'to',
      'explore',
      'how',
      'the',
      'technology',
      'can',
      'improve',
      'online',
      'learning',
      'outcomes',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'See',
      'Harvard',
      'through',
      'this',
      'collection',
      'of',
      'double',
      'exposure',
      'images',
      'where',
      'iconic',
      'elements',
      'of',
      'campus',
      'overlap',
      'and',
      'converge',
      'in',
      'surprising',
      'ways',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'He',
      'is',
      'already',
      'known',
      'as',
      'one',
      'of',
      'Hollywood’s',
      'most',
      'versatile',
      'screen',
      'stars',
      'but',
      'Ryan',
      'Reynolds',
      'earned',
      'an',
      'offbeat',
      'claim',
      'to',
      'fame',
      'with',
      'his',
      'visit',
      'to',
      'Harvard',
      'to',
      'collect',
      'the',
      'Hasty',
      'Pudding',
      'Theatricals’',
      '2017',
      'Man',
      'of',
      'the',
      'Year',
      'award',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Learn',
      'how',
      'to',
      'spot',
      'fake',
      'news',
      'and',
      'connect',
      'with',
      'reputable',
      'resources',
      'with',
      'this',
      'guide',
      'from',
      'the',
      'Harvard',
      'Library',
      'http',
      'hvrd',
      'SENTENCEEND',
      'SENTENCESTART',
      'me',
      'qHoO308EtQ7',
      'NEXTPOST',
      'SENTENCESTART',
      'To',
      'commemorate',
      'one',
      'of',
      'America’s',
      'oldest',
      'continued',
      'partnerships',
      'the',
      'Harvard',
      'Library',
      'curated',
      '“To',
      'Serve',
      'Better',
      'Thy',
      'Country',
      '”',
      'an',
      'exhibit',
      'of',
      'the',
      'interwoven',
      'histories',
      'of',
      'the',
      'two',
      'storied',
      'institutions',
      'assembling',
      'letters',
      'photographs',
      'and',
      'objects',
      'that',
      'show',
      'Harvard',
      'affiliates’',
      'tradition',
      'of',
      'service',
      'from',
      'the',
      'earliest',
      'years',
      'of',
      'our',
      'country',
      'to',
      'today',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Top',
      'reporters',
      'and',
      'editors',
      'discuss',
      'the',
      'future',
      'of',
      'news',
      'as',
      'well',
      'as',
      'the',
      'opportunities',
      'and',
      'the',
      'challenges',
      'the',
      'industry',
      'faces',
      'in',
      'what',
      'many',
      'observers',
      'call',
      'the',
      '“post',
      'truth”',
      'era',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Paola',
      'Villarreal',
      'a',
      'fellow',
      'at',
      'the',
      'Berkman',
      'Klein',
      'Center',
      'for',
      'Internet',
      'Society',
      'at',
      'Harvard',
      'University',
      'is',
      'using',
      'data',
      'visualization',
      'to',
      'shed',
      'light',
      'on',
      'inequality',
      'in',
      'health',
      'housing',
      'and',
      'more',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'In',
      'response',
      'to',
      'travel',
      'restrictions',
      'to',
      'the',
      'U',
      'SENTENCEEND',
      'SENTENCESTART',
      'S',
      'SENTENCEEND',
      'SENTENCESTART',
      'from',
      'seven',
      'Muslim',
      'majority',
      'nations',
      'Harvard',
      'leaders',
      'issued',
      'strong',
      'statements',
      'supporting',
      'the',
      'University’s',
      'international',
      'community',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'On',
      'Franklin',
      'D',
      'SENTENCEEND',
      'SENTENCESTART',
      "Roosevelt's",
      'birthday',
      'look',
      'around',
      'the',
      'FDR',
      'Suite',
      'in',
      'Adams',
      'House',
      'where',
      'the',
      '32nd',
      'president',
      'lived',
      'while',
      'at',
      'Harvard',
      'from',
      '1900',
      '1904',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      '“Benefiting',
      'from',
      'the',
      'talents',
      'and',
      'energy',
      'the',
      'knowledge',
      'and',
      'ideas',
      'of',
      'people',
      'from',
      'nations',
      'around',
      'the',
      'globe',
      'is',
      'not',
      'just',
      'a',
      'vital',
      'interest',
      'of',
      'the',
      'University',
      'it',
      'long',
      'has',
      'been',
      'and',
      'it',
      'fully',
      'remains',
      'a',
      'vital',
      'interest',
      'of',
      ...],
     ['NEXTPOST',
      'SENTENCESTART',
      'Eric',
      'Trump',
      'and',
      'Charlie',
      'Kirk',
      'speak',
      'at',
      'CPAC',
      '2018',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'During',
      'a',
      'fight',
      'late',
      'last',
      'year',
      'a',
      'family',
      'friend',
      'told',
      'a',
      'dispatcher',
      'that',
      'Nikolas',
      'Cruz',
      '“bought',
      'a',
      'gun',
      'from',
      'Dick’s',
      'last',
      'week',
      'and',
      'is',
      'now',
      'going',
      'to',
      'pick',
      'it',
      'up',
      '”',
      'according',
      'to',
      'the',
      '911',
      'call',
      'log',
      'SENTENCEEND',
      'SENTENCESTART',
      'She',
      'added',
      'Cruz',
      'had',
      '“bought',
      'tons',
      'of',
      'ammo',
      '”',
      '“used',
      'a',
      'gun',
      'against',
      'people',
      'before”',
      'and',
      'has',
      'put',
      'the',
      'gun',
      'to',
      'others',
      'sic',
      'heads',
      'in',
      'the',
      'past',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'NEXTPOST',
      'SENTENCESTART',
      'President',
      'Donald',
      'J',
      'SENTENCEEND',
      'SENTENCESTART',
      'Trump',
      'on',
      'Thursday',
      'floated',
      'the',
      'possibility',
      'of',
      'paying',
      'teachers',
      'a',
      '“bonus”',
      'to',
      'carry',
      'concealed',
      'firearms',
      'stressing',
      'the',
      'importance',
      'of',
      'school',
      'security',
      'even',
      'as',
      'he',
      'calls',
      'for',
      'strengthening',
      'gun',
      'laws',
      'in',
      'the',
      'wake',
      'of',
      'the',
      'Parkland',
      'mass',
      'shooting',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'BREAKING',
      'NEWS',
      'A',
      'federal',
      'grand',
      'jury',
      'filed',
      'a',
      '32',
      'count',
      'indictment',
      'Thursday',
      'against',
      'former',
      'Trump',
      'campaign',
      'chairman',
      'Paul',
      'Manafort',
      'and',
      'aide',
      'Rick',
      'Gates',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'Both',
      'men',
      'are',
      'charged',
      'with',
      'multiple',
      'counts',
      'of',
      'preparing',
      'false',
      'tax',
      'returns',
      'bank',
      'fraud',
      'conspiracy',
      'to',
      'commit',
      'bank',
      'fraud',
      'and',
      'failure',
      'to',
      'disclose',
      'income',
      'from',
      'foreign',
      'sources',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Coral',
      'Springs',
      'Police',
      'hold',
      'a',
      'press',
      'conference',
      'on',
      'last',
      "week's",
      'school',
      'shooting',
      'in',
      'Parkland',
      'Florida',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'Credit',
      'WFOR',
      'NEXTPOST',
      'SENTENCESTART',
      'A',
      'man',
      'eating',
      'at',
      "Arby's",
      'rushed',
      'to',
      'help',
      'a',
      'Georgia',
      'state',
      'trooper',
      'who',
      'was',
      'struggling',
      'with',
      'a',
      'suspect',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      '“',
      'Video',
      'images',
      'were',
      'delayed',
      '20',
      'minutes',
      'and',
      'nobody',
      'told',
      'us',
      'that',
      'SENTENCEEND',
      'SENTENCESTART',
      '”',
      'NEWLINE',
      'NEWLINE',
      'Police',
      'officers',
      'responding',
      'to',
      'last',
      'week’s',
      'school',
      'shooting',
      'in',
      'Parkland',
      'Florida',
      'reportedly',
      'thought',
      'they',
      'were',
      'tracking',
      'Nikolas',
      'Cruz',
      'live',
      'on',
      'surveillance',
      'video',
      '—',
      'but',
      'then',
      'realized',
      'the',
      'footage',
      'was',
      'delayed',
      'tossing',
      'roadblocks',
      'into',
      'the',
      'frantic',
      'efforts',
      'to',
      'capture',
      'the',
      '19',
      'year',
      'old',
      'shooting',
      'suspect',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'EXCLUSIVE',
      'The',
      'centerpiece',
      'of',
      'a',
      'The',
      'New',
      'Yorker',
      'story',
      'on',
      'Karen',
      'McDougal',
      'who',
      'says',
      'she',
      'had',
      'an',
      'affair',
      'with',
      'Donald',
      'J',
      'SENTENCEEND',
      'SENTENCESTART',
      'Trump',
      'is',
      'scribbled',
      'notes',
      'kept',
      'by',
      'the',
      'former',
      'Playboy',
      'playmate',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'A',
      'telltale',
      'marking',
      'on',
      'the',
      'entries',
      'reproduced',
      'by',
      'the',
      'magazine',
      'shows',
      'that',
      'McDougal',
      'wrote',
      'these',
      'pages',
      'either',
      'during',
      'or',
      'since',
      'the',
      '2016',
      'campaign—relying',
      'on',
      'memories',
      'that',
      'were',
      'at',
      'least',
      'a',
      'decade',
      'old',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Senator',
      'Ted',
      'Cruz',
      'speaks',
      'at',
      'CPAC',
      '2018',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'The',
      'White',
      'House',
      'Principal',
      'Deputy',
      'Press',
      'Secretary',
      'Raj',
      'Shah',
      'holds',
      'a',
      'briefing',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'HEARTWARMING',
      'Students',
      'arriving',
      'at',
      'Westlake',
      'High',
      'School',
      'were',
      'recently',
      'greeted',
      'by',
      'police',
      'officers',
      'ready',
      'to',
      'give',
      'them',
      'a',
      'hug',
      'or',
      'high',
      'five',
      'SENTENCEEND',
      'SENTENCESTART',
      'The',
      'officers',
      'wanted',
      'to',
      'show',
      'their',
      'support',
      'after',
      'the',
      'recent',
      'school',
      'shooting',
      'in',
      'Parkland',
      'Florida',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Fear',
      'is',
      'not',
      'in',
      'my',
      'vocabulary',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'An',
      '88',
      'year',
      'old',
      'British',
      'military',
      'veteran',
      'said',
      'all',
      'he',
      'wanted',
      'to',
      'do',
      'was',
      'help',
      'the',
      'woman',
      'being',
      'attacked',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'JUST',
      'IN',
      'A',
      'driver',
      'intentionally',
      'crashed',
      'his',
      'car',
      'into',
      'the',
      'emergency',
      'room',
      'entrance',
      'at',
      'Connecticut’s',
      'Middlesex',
      'Hospital',
      'on',
      'Thursday',
      'morning',
      'and',
      'set',
      'himself',
      'on',
      'fire',
      'officials',
      'said',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Marion',
      'Maréchal',
      'Le',
      'Pen',
      'addresses',
      'CPAC',
      '2018',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'A',
      'funeral',
      'is',
      'held',
      'for',
      'Aaron',
      'Feis',
      'the',
      'football',
      'coach',
      'who',
      'died',
      'after',
      'placing',
      'himself',
      'in',
      'front',
      'of',
      'bullets',
      'to',
      'protect',
      'students',
      'from',
      'a',
      'gunman',
      'inside',
      'a',
      'Florida',
      'high',
      'school',
      'last',
      'week',
      'SENTENCEEND',
      'SENTENCESTART',
      'http',
      'fxn',
      'SENTENCEEND',
      'SENTENCESTART',
      'ws',
      '2EFuAeN',
      'NEWLINE',
      'NEWLINE',
      'Courtesy',
      'http',
      'live',
      'SENTENCEEND',
      'SENTENCESTART',
      'cbglades',
      'SENTENCEEND',
      'SENTENCESTART',
      'com',
      'NEXTPOST',
      'SENTENCESTART',
      'Moments',
      'ago',
      'at',
      'CPAC',
      '2018',
      'Vice',
      'President',
      'Mike',
      'Pence',
      'slammed',
      'the',
      'media',
      'for',
      'its',
      'criticisms',
      'of',
      'his',
      'actions',
      'at',
      'the',
      'Olympic',
      'Games',
      'SENTENCEEND',
      'SENTENCESTART',
      'http',
      'fxn',
      'SENTENCEEND',
      'SENTENCESTART',
      'ws',
      '2FlLgWD',
      'NEXTPOST',
      'SENTENCESTART',
      'Vice',
      'President',
      'Mike',
      'Pence',
      'addresses',
      'CPAC',
      '2018',
      'SENTENCEEND',
      'SENTENCESTART',
      'http',
      'fxn',
      'SENTENCEEND',
      'SENTENCESTART',
      'ws',
      '2FlLgWD',
      'NEXTPOST',
      'SENTENCESTART',
      'Attorney',
      'General',
      'Jeff',
      'Sessions',
      'and',
      'other',
      'law',
      'enforcement',
      'officials',
      'hold',
      'a',
      'news',
      'conference',
      'to',
      'announce',
      'a',
      'significant',
      'law',
      'enforcement',
      'action',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Moments',
      'ago',
      'President',
      'Donald',
      'J',
      'SENTENCEEND',
      'SENTENCESTART',
      'Trump',
      'tweeted',
      'his',
      'support',
      'for',
      'the',
      'NRA',
      'National',
      'Rifle',
      'Association',
      'of',
      'America',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'NRA',
      'National',
      'Rifle',
      'Association',
      'of',
      'America',
      'CEO',
      'Wayne',
      'LaPierre',
      'addresses',
      'the',
      'Florida',
      'school',
      'shooting',
      'at',
      'CPAC',
      '2018',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'WATCH',
      'LIVE',
      'The',
      'Conservative',
      'Political',
      'Action',
      'Conference',
      'CPAC',
      '2018',
      'is',
      'held',
      'in',
      'Maryland',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'SpaceX',
      'launches',
      'a',
      'Falcon',
      '9',
      'rocket',
      'with',
      'PAZ',
      'satellite',
      'to',
      'low',
      'Earth',
      'orbit',
      'from',
      'Space',
      'Launch',
      'Complex',
      '4',
      'East',
      'SLC',
      '4E',
      'at',
      'Vandenberg',
      'Air',
      'Force',
      'Base',
      'California',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'The',
      'second',
      'day',
      'of',
      'CPAC',
      'begins',
      'with',
      'the',
      'Presentation',
      'of',
      'Colors',
      'Pledge',
      'of',
      'Allegiance',
      'and',
      'prayer',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'Moments',
      'ago',
      'President',
      'Donald',
      'J',
      'SENTENCEEND',
      'SENTENCESTART',
      'Trump',
      'tweeted',
      'about',
      'new',
      'changes',
      'to',
      'gun',
      'policies',
      'in',
      'the',
      'wake',
      'of',
      'the',
      'Florida',
      'school',
      'shooting',
      'SENTENCEEND',
      'SENTENCESTART',
      'http',
      'fxn',
      'SENTENCEEND',
      'SENTENCESTART',
      'ws',
      '2FnSKYU',
      'NEXTPOST',
      'SENTENCESTART',
      'He',
      'and',
      'my',
      'uncle',
      'both',
      'believed',
      'Acts',
      '17',
      '26',
      "'Of",
      'one',
      'blood',
      'God',
      'made',
      'all',
      'people',
      'SENTENCEEND',
      'SENTENCESTART',
      "'",
      'NEWLINE',
      'NEWLINE',
      'On',
      'Fox',
      'Friends',
      'Dr',
      'SENTENCEEND',
      'SENTENCESTART',
      'Alveda',
      'King',
      'talked',
      'about',
      'the',
      'relationship',
      'between',
      'Rev',
      'SENTENCEEND',
      'SENTENCESTART',
      'Billy',
      'Graham',
      'and',
      'her',
      'uncle',
      'the',
      'Rev',
      'SENTENCEEND',
      'SENTENCESTART',
      'Dr',
      'SENTENCEEND',
      'SENTENCESTART',
      'Martin',
      'Luther',
      'King',
      'Jr',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEXTPOST',
      'SENTENCESTART',
      'How',
      'many',
      'other',
      'FISA',
      'warrants',
      'and',
      'applications',
      'were',
      'based',
      'on',
      'other',
      'junk',
      'science',
      "There's",
      'plenty',
      'of',
      'other',
      'Fusion',
      "GPS's",
      'out',
      'there',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'On',
      'Fox',
      'Friends',
      'Michelle',
      'Malkin',
      'warned',
      'that',
      'the',
      'government',
      'surveillance',
      'abuses',
      'alleged',
      'in',
      'the',
      'controversial',
      'FISA',
      'memo',
      'could',
      'be',
      'just',
      'the',
      'tip',
      'of',
      'the',
      'iceberg',
      'SENTENCEEND',
      'SENTENCESTART',
      'http',
      'bit',
      'SENTENCEEND',
      'SENTENCESTART',
      'ly',
      '2GIC3YA',
      'NEXTPOST',
      'SENTENCESTART',
      'What',
      'I',
      'do',
      'know',
      'is',
      'To',
      'date',
      'not',
      'one',
      'bit',
      'of',
      'evidence',
      'that',
      'shows',
      'any',
      'time',
      'of',
      'coordination',
      'by',
      'the',
      'Trump',
      'campaign',
      'and',
      'Russians',
      'to',
      'influence',
      'the',
      'election',
      'SENTENCEEND',
      'SENTENCESTART',
      'NEWLINE',
      'NEWLINE',
      'On',
      'Fox',
      'Friends',
      'Rep',
      'SENTENCEEND',
      'SENTENCESTART',
      'Jim',
      'Jordan',
      'reflected',
      'on',
      'the',
      'FISA',
      'memo',
      'Jim',
      'Comey',
      'and',
      'the',
      'Russia',
      'investigation',
      'SENTENCEEND',
      'SENTENCESTART',
      'http',
      'bit',
      'SENTENCEEND',
      'SENTENCESTART',
      'ly',
      '2EBgQzd',
      'NEXTPOST',
      'SENTENCESTART',
      'Bryan',
      'Llenas',
      'takes',
      'us',
      'inside',
      'an',
      'active',
      'shooter',
      'situation',
      'drill',
      'filled',
      'with',
      'gunfire',
      'and',
      'smoke',
      'bombs',
      'SENTENCEEND',
      'SENTENCESTART',
      'See',
      'how',
      'one',
      'small',
      'business',
      'is',
      'taking',
      'steps',
      'to',
      'prepare',
      'its',
      'employees',
      'for',
      'worst',
      'case',
      ...]]



We than create the tokens and matrixes for each post:


    from keras.preprocessing.text import Tokenizer
    tokens = []
    text_mtxs = []
    for seq in allpostsAsSequence:
        token = Tokenizer(nb_words=vocabulary_size,char_level=False)
        token.fit_on_texts(seq)
        text_mtx = token.texts_to_matrix(seq, mode='binary')
        tokens.append(token)
        text_mtxs.append(text_mtx)

And creating the vocabulary of each page:


    import pandas as pd
    import numpy as np
    index = 0
    vocabs = []
    for seq in allpostsAsSequence:
        vocab = pd.DataFrame({'word':seq,'code':np.argmax(text_mtxs[index],axis=1)})
        vocab=vocab.drop_duplicates()
        vocabs.append(vocab)
        index += 1


    vocabs




    [      code             word
     0        4         NEXTPOST
     1        1    SENTENCESTART
     2       46            Today
     3       26               it
     4       43              was
     5       34               my
     6       13            great
     7       91            honor
     8        6               to
     9      463             host
     10       9                a
     11     161           School
     12     237           Safety
     13       0       Roundtable
     14      29               at
     15       3              the
     16     124            White
     17     172            House
     18      16             with
     19     151            State
     20       5              and
     21     186            local
     22     219          leaders
     23     104              law
     24     216      enforcement
     25     321         officers
     27       0        education
     28     408        officials
     29       2      SENTENCEEND
     31       8          NEWLINE
     ...    ...              ...
     3979     0            fired
     3981     0             give
     3982     0              any
     3983     0       indication
     3984   225            there
     3986     0            until
     3987     0            after
     3989     0             airs
     3995   583             Paul
     3996   499           Teutul
     4011     0        impressed
     4013   409     THEGaryBusey
     4024   579          cashier
     4027     0          mistake
     4029   548  BrandenRoderick
     4037     0  KellyandMichael
     4039     0             both
     4044    33            Their
     4047   381         terrific
     4055     0     DennisRodman
     4061     0            Korea
     4071     0       definitely
     4072     0        different
     4100     0         advisors
     4107     0            can’t
     4108     0          believe
     4111     0           choose
     4116     0           choice
     4117   418              She
     4121   586           handle
     
     [1348 rows x 2 columns],       code            word
     0        9        NEXTPOST
     1        1   SENTENCESTART
     2       65           Today
     3        0         Kehinde
     4        0           Wiley
     5        6             and
     6      362             Amy
     7        0         Sherald
     8        0          became
     9        3             the
     10      79           first
     11       0           black
     12     401         artists
     13       4              to
     14     436          create
     15      77        official
     16     163    presidential
     17       0       portraits
     18      12             for
     20       0     Smithsonian
     21       2     SENTENCEEND
     23       4              To
     24     172            call
     25      24            this
     26     572      experience
     27       0        humbling
     28      82           would
     29      37              be
     30      96              an
     31       0  understatement
     ...    ...             ...
     6607     0         arrival
     6608     0        ceremony
     6644     0       receiving
     6645     0            line
     6648     0          guests
     6686     0          visits
     6688   275           Great
     6689     0          Buddha
     6691   384        Kamakura
     6693     0         Michiko
     6694   376            Sato
     6695     0        director
     6700     0           Takao
     6702     0           Chief
     6703     0            Monk
     6706     0          Kotoku
     6708     0          Temple
     6735     0           holds
     6737     0       bilateral
     6738     0         meeting
     6741     0              Hu
     6742     0          Jintao
     6744     0           China
     6747     0           Grand
     6748     0           Hyatt
     6751     0           Seoul
     6753     0           Korea
     6757     0              11
     6770     0        Samantha
     6771     0        Appleton
     
     [1848 rows x 2 columns],        code           word
     0        12       NEXTPOST
     1         1  SENTENCESTART
     2       114             “I
     3       103           feel
     4        32           like
     5       366            God
     6       117            has
     7         0          given
     8        21             me
     9         6              a
     10      351         talent
     11       22            for
     12      282            art
     13        7            and
     14        0          using
     15       13           that
     17       24             is
     19        0           form
     20        9             of
     21        0      gratitude
     22        2    SENTENCEEND
     24       22            For
     25       67          years
     26        3              I
     27      268         worked
     28       15             in
     29       74             an
     30        0     automotive
     31        0       workshop
     34      176            I’d
     ...     ...            ...
     14443     0            Wes
     14444     0       Anderson
     14452     0            Fur
     14453     0           coat
     14456     0         winter
     14458   207           Look
     14461     0          polar
     14464     0       Stuntin'
     14467     0           snow
     14470     0         haters
     14485     0          flare
     14489     0           trod
     14490     0        fashion
     14491     0      statement
     14496   385        CAPTION
     14497     0        CONTEST
     14503     0        contest
     14505     0         awhile
     14508     0         Submit
     14530     0           site
     14543   530          cause
     14545     0         lawyer
     14572     0      Sometimes
     14581     0         bright
     14584     0         golden
     14593     0        Chinese
     14594     0          candy
     14599   578           Hold
     14626     0          kinda
     14635     0        lassoed
     
     [2452 rows x 2 columns],       code            word
     0        4        NEXTPOST
     1        1   SENTENCESTART
     2      551         Firefly
     3        7               a
     4       26             new
     5        0      microscope
     6       22            from
     7       35       Harvard's
     8        0           Cohen
     9      442             Lab
     10      39             can
     11     202            show
     12      28             how
     13       0         neurons
     14       0           pulse
     15       0     communicate
     16       8             and
     17       0           shine
     18       2     SENTENCEEND
     20       3             The
     21       0    microscope's
     22     544           large
     23     444           field
     24       5              of
     25     304            view
     27       0            fast
     28     556         imaging
     29       0      capability
     30       0          allows
     31      21              it
     ...    ...             ...
     4527     0       Institute
     4529   398         African
     4533    71        Research
     4535     0        powerful
     4539     0              96
     4541   572             old
     4543     0       Elizabeth
     4544     0         Catlett
     4545     0          offers
     4546     0         viewers
     4548     0           sense
     4550   500         despair
     4560   565           great
     4567     0        dreaming
     4589     0           comes
     4591     0  responsibility
     4593     0          figure
     4597     0            “use
     4613     0       partnered
     4617     0         schools
     4621     0            half
     4626     0          filled
     4628     0    motivational
     4629     0           talks
     4631     0        provided
     4634     0           taste
     4639     0           lunch
     4641     0       Annenberg
     4646     0           tours
     4652    76           staff
     
     [1778 rows x 2 columns],       code            word
     0        5        NEXTPOST
     1        1   SENTENCESTART
     2        0            Eric
     3       23           Trump
     4       11             and
     5        0         Charlie
     6        0            Kirk
     7        0           speak
     8       15              at
     9       93            CPAC
     10     101            2018
     11       2     SENTENCEEND
     15      66          During
     16       8               a
     17     330           fight
     18     438            late
     19      64            last
     20     121            year
     22     150          family
     23       0          friend
     24     187            told
     26       0      dispatcher
     27      14            that
     28     509         Nikolas
     29     148            Cruz
     30       0         “bought
     32     129             gun
     33      62            from
     34       0          Dick’s
     36     149            week
     ...    ...             ...
     6225     0           “lack
     6227     0        interest
     6229     0            Army
     6231     0            Glen
     6232     0        Phillips
     6235     0        Facebook
     6249     0          person
     6272     0          Thanks
     6276     0         Liberty
     6277     0      Kentucky’s
     6278     0    Independence
     6289     0         1rXl477
     6317   514         1vpauWB
     6345     0        murdered
     6346     0        somebody
     6356     0         presses
     6357     0         Weather
     6358     0     Underground
     6359     0              co
     6360     0         founder
     6362     0           Ayers
     6367     0        bombings
     6370     0       buildings
     6376     0         violent
     6427     0           isn't
     6429     0      individual
     6430     0  responsibility
     6434     0      difference
     6439     0        greatest
     6446     0     opportunity
     
     [1955 rows x 2 columns]]



Creating inputs and outputs for each page's network:


    inputs = []
    outputs = []
    for text_mat in text_mtxs:
        inputs.append(text_mat[:-1])
        outputs.append(text_mat[1:])


    from keras.models import Sequential
    from keras.layers.core import Dense, Activation, Flatten
    from keras.layers.embeddings import Embedding

Assembling a model for each page:


    models = []
    index = 0
    for input_ in inputs:
        model = Sequential()
        model.add(Embedding(input_dim=input_.shape[1],output_dim= 42, input_length=input_.shape[1]))
        model.add(Flatten())
        model.add(Dense(outputs[index].shape[1], activation='sigmoid'))
        index += 1
        models.append(model)

Compiling all the models:


    for model in models:
        model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])

The model training is commented out to save time in future runs. We will load the models' weights from backup files.

Training the models:(we tried many options for the parameters and found these to be optimal)


    index = 0
    for model in models:
        if(index == 2):
            model.fit(inputs[index], y=outputs[index], batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)
        else:
            model.fit(inputs[index], y=outputs[index], batch_size=200, nb_epoch=100, verbose=1, validation_split=0.2)
        index +=1

    Train on 3299 samples, validate on 825 samples
    Epoch 1/100
    3299/3299 [==============================] - 7s - loss: 4.9926 - acc: 0.0209 - val_loss: 4.8163 - val_acc: 0.1285
    Epoch 2/100
    3299/3299 [==============================] - 7s - loss: 4.6610 - acc: 0.0740 - val_loss: 4.8188 - val_acc: 0.1164
    Epoch 3/100
    3299/3299 [==============================] - 7s - loss: 4.6067 - acc: 0.0700 - val_loss: 4.8832 - val_acc: 0.1164
    Epoch 4/100
    3299/3299 [==============================] - 7s - loss: 4.5678 - acc: 0.0700 - val_loss: 4.9940 - val_acc: 0.1164
    Epoch 5/100
    3299/3299 [==============================] - 7s - loss: 4.5333 - acc: 0.0700 - val_loss: 5.0360 - val_acc: 0.1164
    Epoch 6/100
    3299/3299 [==============================] - 7s - loss: 4.4906 - acc: 0.0700 - val_loss: 5.0286 - val_acc: 0.1200
    Epoch 7/100
    3299/3299 [==============================] - 7s - loss: 4.4436 - acc: 0.0703 - val_loss: 5.0721 - val_acc: 0.1164
    Epoch 8/100
    3299/3299 [==============================] - 7s - loss: 4.3902 - acc: 0.0797 - val_loss: 5.1130 - val_acc: 0.1830
    Epoch 9/100
    3299/3299 [==============================] - 7s - loss: 4.3326 - acc: 0.1070 - val_loss: 5.3055 - val_acc: 0.1782
    Epoch 10/100
    3299/3299 [==============================] - 7s - loss: 4.2321 - acc: 0.1373 - val_loss: 5.2103 - val_acc: 0.1915
    Epoch 11/100
    3299/3299 [==============================] - 7s - loss: 4.1256 - acc: 0.1488 - val_loss: 5.2006 - val_acc: 0.1491
    Epoch 12/100
    3299/3299 [==============================] - 7s - loss: 4.0349 - acc: 0.1570 - val_loss: 5.1365 - val_acc: 0.1915
    Epoch 13/100
    3299/3299 [==============================] - 7s - loss: 3.9347 - acc: 0.1658 - val_loss: 5.1415 - val_acc: 0.1552
    Epoch 14/100
    3299/3299 [==============================] - 7s - loss: 3.8385 - acc: 0.1828 - val_loss: 5.0603 - val_acc: 0.1903
    Epoch 15/100
    3299/3299 [==============================] - 7s - loss: 3.7458 - acc: 0.1928 - val_loss: 5.0323 - val_acc: 0.1939
    Epoch 16/100
    3299/3299 [==============================] - 7s - loss: 3.6494 - acc: 0.1976 - val_loss: 4.9812 - val_acc: 0.1964
    Epoch 17/100
    3299/3299 [==============================] - 7s - loss: 3.5612 - acc: 0.1998 - val_loss: 4.9509 - val_acc: 0.1915
    Epoch 18/100
    3299/3299 [==============================] - 7s - loss: 3.4681 - acc: 0.2110 - val_loss: 4.9518 - val_acc: 0.1891
    Epoch 19/100
    3299/3299 [==============================] - 7s - loss: 3.3781 - acc: 0.2158 - val_loss: 4.9879 - val_acc: 0.1988
    Epoch 20/100
    3299/3299 [==============================] - 7s - loss: 3.2968 - acc: 0.2283 - val_loss: 5.0076 - val_acc: 0.1697
    Epoch 21/100
    3299/3299 [==============================] - 7s - loss: 3.2118 - acc: 0.2380 - val_loss: 4.9573 - val_acc: 0.2036
    Epoch 22/100
    3299/3299 [==============================] - 7s - loss: 3.1232 - acc: 0.2413 - val_loss: 4.9530 - val_acc: 0.2085
    Epoch 23/100
    3299/3299 [==============================] - 7s - loss: 3.0458 - acc: 0.2510 - val_loss: 4.7987 - val_acc: 0.1770
    Epoch 24/100
    3299/3299 [==============================] - 7s - loss: 2.9738 - acc: 0.2625 - val_loss: 4.8748 - val_acc: 0.2085
    Epoch 25/100
    3299/3299 [==============================] - 7s - loss: 2.8899 - acc: 0.2637 - val_loss: 4.9179 - val_acc: 0.2097
    Epoch 26/100
    3299/3299 [==============================] - 7s - loss: 2.8200 - acc: 0.2698 - val_loss: 4.9429 - val_acc: 0.2024
    Epoch 27/100
    3299/3299 [==============================] - 7s - loss: 2.7458 - acc: 0.2752 - val_loss: 4.9303 - val_acc: 0.2097
    Epoch 28/100
    3299/3299 [==============================] - 7s - loss: 2.6873 - acc: 0.2822 - val_loss: 4.9006 - val_acc: 0.2024
    Epoch 29/100
    3299/3299 [==============================] - 7s - loss: 2.6221 - acc: 0.2898 - val_loss: 4.9113 - val_acc: 0.2036
    Epoch 30/100
    3299/3299 [==============================] - 7s - loss: 2.5540 - acc: 0.2965 - val_loss: 5.0262 - val_acc: 0.2109
    Epoch 31/100
    3299/3299 [==============================] - 7s - loss: 2.4973 - acc: 0.3037 - val_loss: 5.0235 - val_acc: 0.2048
    Epoch 32/100
    3299/3299 [==============================] - 7s - loss: 2.4351 - acc: 0.3092 - val_loss: 5.0702 - val_acc: 0.2097
    Epoch 33/100
    3299/3299 [==============================] - 7s - loss: 2.3940 - acc: 0.3156 - val_loss: 5.0280 - val_acc: 0.2133
    Epoch 34/100
    3299/3299 [==============================] - 7s - loss: 2.3353 - acc: 0.3171 - val_loss: 4.9384 - val_acc: 0.2048
    Epoch 35/100
    3299/3299 [==============================] - 7s - loss: 2.2922 - acc: 0.3219 - val_loss: 5.0836 - val_acc: 0.2109
    Epoch 36/100
    3299/3299 [==============================] - 7s - loss: 2.2416 - acc: 0.3268 - val_loss: 4.9685 - val_acc: 0.2061
    Epoch 37/100
    3299/3299 [==============================] - 7s - loss: 2.2043 - acc: 0.3256 - val_loss: 5.1223 - val_acc: 0.2024
    Epoch 38/100
    3299/3299 [==============================] - 7s - loss: 2.1622 - acc: 0.3307 - val_loss: 5.0787 - val_acc: 0.2024
    Epoch 39/100
    3299/3299 [==============================] - 7s - loss: 2.1288 - acc: 0.3322 - val_loss: 5.0926 - val_acc: 0.2036
    Epoch 40/100
    3299/3299 [==============================] - 7s - loss: 2.0920 - acc: 0.3328 - val_loss: 5.2005 - val_acc: 0.2109
    Epoch 41/100
    3299/3299 [==============================] - 7s - loss: 2.0611 - acc: 0.3419 - val_loss: 5.1808 - val_acc: 0.2061
    Epoch 42/100
    3299/3299 [==============================] - 7s - loss: 2.0334 - acc: 0.3425 - val_loss: 5.1994 - val_acc: 0.2036
    Epoch 43/100
    3299/3299 [==============================] - 7s - loss: 2.0019 - acc: 0.3407 - val_loss: 5.3119 - val_acc: 0.2048
    Epoch 44/100
    3299/3299 [==============================] - 7s - loss: 1.9897 - acc: 0.3407 - val_loss: 5.3208 - val_acc: 0.2109
    Epoch 45/100
    3299/3299 [==============================] - 7s - loss: 1.9647 - acc: 0.3392 - val_loss: 5.2665 - val_acc: 0.2133
    Epoch 46/100
    3299/3299 [==============================] - 7s - loss: 1.9385 - acc: 0.3413 - val_loss: 5.3806 - val_acc: 0.2073
    Epoch 47/100
    3299/3299 [==============================] - 7s - loss: 1.9255 - acc: 0.3440 - val_loss: 5.2982 - val_acc: 0.2012
    Epoch 48/100
    3299/3299 [==============================] - 7s - loss: 1.9035 - acc: 0.3365 - val_loss: 5.4399 - val_acc: 0.2145
    Epoch 49/100
    3299/3299 [==============================] - 7s - loss: 1.8967 - acc: 0.3377 - val_loss: 5.3606 - val_acc: 0.1988
    Epoch 50/100
    3299/3299 [==============================] - 7s - loss: 1.8839 - acc: 0.3365 - val_loss: 5.5037 - val_acc: 0.2121
    Epoch 51/100
    3299/3299 [==============================] - 7s - loss: 1.8697 - acc: 0.3362 - val_loss: 5.5007 - val_acc: 0.2109
    Epoch 52/100
    3299/3299 [==============================] - 7s - loss: 1.8597 - acc: 0.3334 - val_loss: 5.5526 - val_acc: 0.2097
    Epoch 53/100
    3299/3299 [==============================] - 7s - loss: 1.8551 - acc: 0.3346 - val_loss: 5.5519 - val_acc: 0.2121
    Epoch 54/100
    3299/3299 [==============================] - 7s - loss: 1.8506 - acc: 0.3316 - val_loss: 5.5699 - val_acc: 0.2133
    Epoch 55/100
    3299/3299 [==============================] - 7s - loss: 1.8437 - acc: 0.3307 - val_loss: 5.5883 - val_acc: 0.2097
    Epoch 56/100
    3299/3299 [==============================] - 7s - loss: 1.8329 - acc: 0.3298 - val_loss: 5.4939 - val_acc: 0.2036
    Epoch 57/100
    3299/3299 [==============================] - 7s - loss: 1.8313 - acc: 0.3349 - val_loss: 5.6499 - val_acc: 0.2036
    Epoch 58/100
    3299/3299 [==============================] - 7s - loss: 1.8268 - acc: 0.3295 - val_loss: 5.4915 - val_acc: 0.2012
    Epoch 59/100
    3299/3299 [==============================] - 7s - loss: 1.8234 - acc: 0.3295 - val_loss: 5.7636 - val_acc: 0.2085
    Epoch 60/100
    3299/3299 [==============================] - 7s - loss: 1.8187 - acc: 0.3298 - val_loss: 5.7561 - val_acc: 0.2109
    Epoch 61/100
    3299/3299 [==============================] - 7s - loss: 1.8175 - acc: 0.3319 - val_loss: 5.7180 - val_acc: 0.2097
    Epoch 62/100
    3299/3299 [==============================] - 7s - loss: 1.8067 - acc: 0.3313 - val_loss: 5.9141 - val_acc: 0.2145
    Epoch 63/100
    3299/3299 [==============================] - 8s - loss: 1.8137 - acc: 0.3316 - val_loss: 5.8558 - val_acc: 0.2121
    Epoch 64/100
    3299/3299 [==============================] - 7s - loss: 1.8100 - acc: 0.3319 - val_loss: 5.4978 - val_acc: 0.1988
    Epoch 65/100
    3299/3299 [==============================] - 7s - loss: 1.8111 - acc: 0.3295 - val_loss: 5.8187 - val_acc: 0.2121
    Epoch 66/100
    3299/3299 [==============================] - 7s - loss: 1.8095 - acc: 0.3298 - val_loss: 5.8387 - val_acc: 0.2097
    Epoch 67/100
    3299/3299 [==============================] - 7s - loss: 1.8016 - acc: 0.3298 - val_loss: 5.8201 - val_acc: 0.2097
    Epoch 68/100
    3299/3299 [==============================] - 7s - loss: 1.8003 - acc: 0.3325 - val_loss: 5.8265 - val_acc: 0.2133
    Epoch 69/100
    3299/3299 [==============================] - 7s - loss: 1.8023 - acc: 0.3295 - val_loss: 5.7311 - val_acc: 0.2000
    Epoch 70/100
    3299/3299 [==============================] - 7s - loss: 1.7972 - acc: 0.3340 - val_loss: 5.4863 - val_acc: 0.2085
    Epoch 71/100
    3299/3299 [==============================] - 7s - loss: 1.8014 - acc: 0.3334 - val_loss: 5.6955 - val_acc: 0.2000
    Epoch 72/100
    3299/3299 [==============================] - 7s - loss: 1.7908 - acc: 0.3301 - val_loss: 5.8889 - val_acc: 0.2158
    Epoch 73/100
    3299/3299 [==============================] - 7s - loss: 1.7916 - acc: 0.3328 - val_loss: 5.8768 - val_acc: 0.2133
    Epoch 74/100
    3299/3299 [==============================] - 7s - loss: 1.8008 - acc: 0.3301 - val_loss: 6.0044 - val_acc: 0.2109
    Epoch 75/100
    3299/3299 [==============================] - 7s - loss: 1.7927 - acc: 0.3310 - val_loss: 5.7313 - val_acc: 0.2073
    Epoch 76/100
    3299/3299 [==============================] - 7s - loss: 1.7837 - acc: 0.3374 - val_loss: 5.7795 - val_acc: 0.1976
    Epoch 77/100
    3299/3299 [==============================] - 7s - loss: 1.7953 - acc: 0.3334 - val_loss: 5.7565 - val_acc: 0.2073
    Epoch 78/100
    3299/3299 [==============================] - 7s - loss: 1.7889 - acc: 0.3325 - val_loss: 5.6904 - val_acc: 0.2085
    Epoch 79/100
    3299/3299 [==============================] - 7s - loss: 1.7846 - acc: 0.3371 - val_loss: 5.9425 - val_acc: 0.2145
    Epoch 80/100
    3299/3299 [==============================] - 7s - loss: 1.7903 - acc: 0.3310 - val_loss: 5.7401 - val_acc: 0.1952
    Epoch 81/100
    3299/3299 [==============================] - 7s - loss: 1.7894 - acc: 0.3319 - val_loss: 5.8597 - val_acc: 0.2012
    Epoch 82/100
    3299/3299 [==============================] - 7s - loss: 1.7875 - acc: 0.3328 - val_loss: 6.0017 - val_acc: 0.2024
    Epoch 83/100
    3299/3299 [==============================] - 7s - loss: 1.7812 - acc: 0.3310 - val_loss: 5.8492 - val_acc: 0.1988
    Epoch 84/100
    3299/3299 [==============================] - 7s - loss: 1.7897 - acc: 0.3313 - val_loss: 5.6943 - val_acc: 0.1988
    Epoch 85/100
    3299/3299 [==============================] - 7s - loss: 1.7878 - acc: 0.3295 - val_loss: 6.0683 - val_acc: 0.2097
    Epoch 86/100
    3299/3299 [==============================] - 7s - loss: 1.7817 - acc: 0.3289 - val_loss: 5.9033 - val_acc: 0.2024
    Epoch 87/100
    3299/3299 [==============================] - 7s - loss: 1.7872 - acc: 0.3346 - val_loss: 6.0656 - val_acc: 0.2073
    Epoch 88/100
    3299/3299 [==============================] - 7s - loss: 1.7815 - acc: 0.3331 - val_loss: 6.1051 - val_acc: 0.2085
    Epoch 89/100
    3299/3299 [==============================] - 7s - loss: 1.7934 - acc: 0.3331 - val_loss: 5.8579 - val_acc: 0.2073
    Epoch 90/100
    3299/3299 [==============================] - 7s - loss: 1.7798 - acc: 0.3337 - val_loss: 6.0135 - val_acc: 0.2109
    Epoch 91/100
    3299/3299 [==============================] - 7s - loss: 1.7802 - acc: 0.3319 - val_loss: 5.8505 - val_acc: 0.1976
    Epoch 92/100
    3299/3299 [==============================] - 7s - loss: 1.7870 - acc: 0.3301 - val_loss: 5.6533 - val_acc: 0.2109
    Epoch 93/100
    3299/3299 [==============================] - 7s - loss: 1.7854 - acc: 0.3289 - val_loss: 6.0483 - val_acc: 0.2109
    Epoch 94/100
    3299/3299 [==============================] - 7s - loss: 1.7775 - acc: 0.3298 - val_loss: 6.1745 - val_acc: 0.2073
    Epoch 95/100
    3299/3299 [==============================] - 7s - loss: 1.7773 - acc: 0.3283 - val_loss: 6.1824 - val_acc: 0.2000
    Epoch 96/100
    3299/3299 [==============================] - 7s - loss: 1.7732 - acc: 0.3343 - val_loss: 5.7044 - val_acc: 0.2000
    Epoch 97/100
    3299/3299 [==============================] - 7s - loss: 1.7812 - acc: 0.3319 - val_loss: 6.2425 - val_acc: 0.2061
    Epoch 98/100
    3299/3299 [==============================] - 7s - loss: 1.7820 - acc: 0.3322 - val_loss: 5.8958 - val_acc: 0.2109
    Epoch 99/100
    3299/3299 [==============================] - 7s - loss: 1.7846 - acc: 0.3307 - val_loss: 6.2506 - val_acc: 0.2121
    Epoch 100/100
    3299/3299 [==============================] - 7s - loss: 1.7795 - acc: 0.3359 - val_loss: 6.3412 - val_acc: 0.2073
    Train on 5416 samples, validate on 1355 samples
    Epoch 1/100
    5416/5416 [==============================] - 12s - loss: 4.6667 - acc: 0.0279 - val_loss: 4.8574 - val_acc: 0.0148
    Epoch 2/100
    5416/5416 [==============================] - 12s - loss: 4.4217 - acc: 0.0541 - val_loss: 5.0105 - val_acc: 0.0871
    Epoch 3/100
    5416/5416 [==============================] - 12s - loss: 4.3678 - acc: 0.0668 - val_loss: 5.0507 - val_acc: 0.0871
    Epoch 4/100
    5416/5416 [==============================] - 12s - loss: 4.3106 - acc: 0.0702 - val_loss: 5.1244 - val_acc: 0.1166
    Epoch 5/100
    5416/5416 [==============================] - 12s - loss: 4.2476 - acc: 0.0587 - val_loss: 5.1103 - val_acc: 0.0583
    Epoch 6/100
    5416/5416 [==============================] - 13s - loss: 4.1578 - acc: 0.0654 - val_loss: 5.4086 - val_acc: 0.0583
    Epoch 7/100
    5416/5416 [==============================] - 13s - loss: 4.0210 - acc: 0.1422 - val_loss: 5.0993 - val_acc: 0.1491
    Epoch 8/100
    5416/5416 [==============================] - 12s - loss: 3.8804 - acc: 0.1595 - val_loss: 5.0157 - val_acc: 0.1594
    Epoch 9/100
    5416/5416 [==============================] - 12s - loss: 3.7551 - acc: 0.1632 - val_loss: 5.1648 - val_acc: 0.1624
    Epoch 10/100
    5416/5416 [==============================] - 13s - loss: 3.6264 - acc: 0.1712 - val_loss: 4.9255 - val_acc: 0.1823
    Epoch 11/100
    5416/5416 [==============================] - 12s - loss: 3.5117 - acc: 0.1874 - val_loss: 4.9749 - val_acc: 0.1720
    Epoch 12/100
    5416/5416 [==============================] - 12s - loss: 3.3981 - acc: 0.1946 - val_loss: 4.9022 - val_acc: 0.1579
    Epoch 13/100
    5416/5416 [==============================] - 12s - loss: 3.2891 - acc: 0.2049 - val_loss: 4.8402 - val_acc: 0.1764
    Epoch 14/100
    5416/5416 [==============================] - 12s - loss: 3.1873 - acc: 0.2110 - val_loss: 4.5853 - val_acc: 0.1845
    Epoch 15/100
    5416/5416 [==============================] - 12s - loss: 3.0907 - acc: 0.2219 - val_loss: 5.1283 - val_acc: 0.1756
    Epoch 16/100
    5416/5416 [==============================] - 12s - loss: 3.0043 - acc: 0.2284 - val_loss: 5.0700 - val_acc: 0.1756
    Epoch 17/100
    5416/5416 [==============================] - 12s - loss: 2.9160 - acc: 0.2358 - val_loss: 4.7551 - val_acc: 0.1889
    Epoch 18/100
    5416/5416 [==============================] - 12s - loss: 2.8268 - acc: 0.2415 - val_loss: 4.6989 - val_acc: 0.1934
    Epoch 19/100
    5416/5416 [==============================] - 12s - loss: 2.7509 - acc: 0.2491 - val_loss: 4.9063 - val_acc: 0.1793
    Epoch 20/100
    5416/5416 [==============================] - 12s - loss: 2.6763 - acc: 0.2552 - val_loss: 4.9980 - val_acc: 0.1963
    Epoch 21/100
    5416/5416 [==============================] - 12s - loss: 2.6095 - acc: 0.2598 - val_loss: 5.1065 - val_acc: 0.1786
    Epoch 22/100
    5416/5416 [==============================] - 12s - loss: 2.5481 - acc: 0.2618 - val_loss: 5.0910 - val_acc: 0.1860
    Epoch 23/100
    5416/5416 [==============================] - 12s - loss: 2.4915 - acc: 0.2674 - val_loss: 4.8941 - val_acc: 0.1823
    Epoch 24/100
    5416/5416 [==============================] - 12s - loss: 2.4329 - acc: 0.2718 - val_loss: 5.0100 - val_acc: 0.1956
    Epoch 25/100
    5416/5416 [==============================] - 12s - loss: 2.3882 - acc: 0.2738 - val_loss: 4.8169 - val_acc: 0.1904
    Epoch 26/100
    5416/5416 [==============================] - 12s - loss: 2.3450 - acc: 0.2775 - val_loss: 4.8757 - val_acc: 0.1897
    Epoch 27/100
    5416/5416 [==============================] - 12s - loss: 2.3100 - acc: 0.2768 - val_loss: 4.9787 - val_acc: 0.1904
    Epoch 28/100
    5416/5416 [==============================] - 12s - loss: 2.2664 - acc: 0.2797 - val_loss: 5.2358 - val_acc: 0.1860
    Epoch 29/100
    5416/5416 [==============================] - 12s - loss: 2.2370 - acc: 0.2779 - val_loss: 4.9575 - val_acc: 0.1889
    Epoch 30/100
    5416/5416 [==============================] - 12s - loss: 2.2106 - acc: 0.2810 - val_loss: 4.9762 - val_acc: 0.1867
    Epoch 31/100
    5416/5416 [==============================] - 12s - loss: 2.1863 - acc: 0.2831 - val_loss: 5.1884 - val_acc: 0.1919
    Epoch 32/100
    5416/5416 [==============================] - 12s - loss: 2.1611 - acc: 0.2794 - val_loss: 5.1367 - val_acc: 0.1911
    Epoch 33/100
    5416/5416 [==============================] - 12s - loss: 2.1454 - acc: 0.2790 - val_loss: 4.9238 - val_acc: 0.1919
    Epoch 34/100
    5416/5416 [==============================] - 12s - loss: 2.1266 - acc: 0.2760 - val_loss: 5.4054 - val_acc: 0.1830
    Epoch 35/100
    5416/5416 [==============================] - 13s - loss: 2.1177 - acc: 0.2779 - val_loss: 5.3747 - val_acc: 0.1830
    Epoch 36/100
    5416/5416 [==============================] - 13s - loss: 2.0977 - acc: 0.2816 - val_loss: 5.1336 - val_acc: 0.1926
    Epoch 37/100
    5416/5416 [==============================] - 13s - loss: 2.0938 - acc: 0.2784 - val_loss: 5.5416 - val_acc: 0.1852
    Epoch 38/100
    5416/5416 [==============================] - 13s - loss: 2.0861 - acc: 0.2753 - val_loss: 5.5524 - val_acc: 0.1963
    Epoch 39/100
    5416/5416 [==============================] - 13s - loss: 2.0787 - acc: 0.2751 - val_loss: 5.5944 - val_acc: 0.1860
    Epoch 40/100
    5416/5416 [==============================] - 13s - loss: 2.0731 - acc: 0.2762 - val_loss: 5.5925 - val_acc: 0.1830
    Epoch 41/100
    5416/5416 [==============================] - 12s - loss: 2.0718 - acc: 0.2736 - val_loss: 5.5799 - val_acc: 0.1845
    Epoch 42/100
    5416/5416 [==============================] - 12s - loss: 2.0607 - acc: 0.2758 - val_loss: 5.6479 - val_acc: 0.1823
    Epoch 43/100
    5416/5416 [==============================] - 12s - loss: 2.0599 - acc: 0.2731 - val_loss: 5.4292 - val_acc: 0.1852
    Epoch 44/100
    5416/5416 [==============================] - 12s - loss: 2.0625 - acc: 0.2716 - val_loss: 5.0826 - val_acc: 0.1690
    Epoch 45/100
    5416/5416 [==============================] - 12s - loss: 2.0587 - acc: 0.2720 - val_loss: 5.4436 - val_acc: 0.1882
    Epoch 46/100
    5416/5416 [==============================] - 13s - loss: 2.0603 - acc: 0.2749 - val_loss: 5.6231 - val_acc: 0.1830
    Epoch 47/100
    5416/5416 [==============================] - 15s - loss: 2.0532 - acc: 0.2734 - val_loss: 5.6191 - val_acc: 0.1838
    Epoch 48/100
    5416/5416 [==============================] - 14s - loss: 2.0436 - acc: 0.2718 - val_loss: 5.7750 - val_acc: 0.1875
    Epoch 49/100
    5416/5416 [==============================] - 14s - loss: 2.0496 - acc: 0.2755 - val_loss: 5.2422 - val_acc: 0.1734
    Epoch 50/100
    5416/5416 [==============================] - 14s - loss: 2.0445 - acc: 0.2718 - val_loss: 5.5985 - val_acc: 0.1801
    Epoch 51/100
    5416/5416 [==============================] - 15s - loss: 2.0456 - acc: 0.2720 - val_loss: 5.6386 - val_acc: 0.1749
    Epoch 52/100
    5416/5416 [==============================] - 13s - loss: 2.0412 - acc: 0.2698 - val_loss: 5.7255 - val_acc: 0.1756
    Epoch 53/100
    5416/5416 [==============================] - 12s - loss: 2.0424 - acc: 0.2727 - val_loss: 5.7396 - val_acc: 0.1860
    Epoch 54/100
    5416/5416 [==============================] - 13s - loss: 2.0377 - acc: 0.2659 - val_loss: 5.6861 - val_acc: 0.1830
    Epoch 55/100
    5416/5416 [==============================] - 13s - loss: 2.0390 - acc: 0.2710 - val_loss: 5.5182 - val_acc: 0.1734
    Epoch 56/100
    5416/5416 [==============================] - 13s - loss: 2.0385 - acc: 0.2746 - val_loss: 5.2374 - val_acc: 0.1653
    Epoch 57/100
    5416/5416 [==============================] - 12s - loss: 2.0451 - acc: 0.2709 - val_loss: 5.5901 - val_acc: 0.1786
    Epoch 58/100
    5416/5416 [==============================] - 12s - loss: 2.0429 - acc: 0.2703 - val_loss: 5.6682 - val_acc: 0.1815
    Epoch 59/100
    5416/5416 [==============================] - 12s - loss: 2.0348 - acc: 0.2740 - val_loss: 5.3139 - val_acc: 0.1727
    Epoch 60/100
    5416/5416 [==============================] - 12s - loss: 2.0412 - acc: 0.2707 - val_loss: 5.8074 - val_acc: 0.1675
    Epoch 61/100
    5416/5416 [==============================] - 12s - loss: 2.0385 - acc: 0.2705 - val_loss: 5.5649 - val_acc: 0.1845
    Epoch 62/100
    5416/5416 [==============================] - 12s - loss: 2.0415 - acc: 0.2734 - val_loss: 5.7590 - val_acc: 0.1860
    Epoch 63/100
    5416/5416 [==============================] - 12s - loss: 2.0383 - acc: 0.2710 - val_loss: 5.6200 - val_acc: 0.1801
    Epoch 64/100
    5416/5416 [==============================] - 13s - loss: 2.0259 - acc: 0.2760 - val_loss: 6.0232 - val_acc: 0.1845
    Epoch 65/100
    5416/5416 [==============================] - 12s - loss: 2.0410 - acc: 0.2714 - val_loss: 5.5053 - val_acc: 0.1808
    Epoch 66/100
    5416/5416 [==============================] - 13s - loss: 2.0340 - acc: 0.2729 - val_loss: 5.6386 - val_acc: 0.1801
    Epoch 67/100
    5416/5416 [==============================] - 13s - loss: 2.0339 - acc: 0.2768 - val_loss: 5.8394 - val_acc: 0.1882
    Epoch 68/100
    5416/5416 [==============================] - 13s - loss: 2.0364 - acc: 0.2749 - val_loss: 5.8444 - val_acc: 0.1764
    Epoch 69/100
    5416/5416 [==============================] - 14s - loss: 2.0339 - acc: 0.2727 - val_loss: 5.8542 - val_acc: 0.1823
    Epoch 70/100
    5416/5416 [==============================] - 14s - loss: 2.0392 - acc: 0.2718 - val_loss: 5.4362 - val_acc: 0.1786
    Epoch 71/100
    5416/5416 [==============================] - 14s - loss: 2.0296 - acc: 0.2747 - val_loss: 5.3432 - val_acc: 0.1712
    Epoch 72/100
    5416/5416 [==============================] - 12s - loss: 2.0271 - acc: 0.2716 - val_loss: 5.9688 - val_acc: 0.1808
    Epoch 73/100
    5416/5416 [==============================] - 13s - loss: 2.0399 - acc: 0.2722 - val_loss: 5.5102 - val_acc: 0.1845
    Epoch 74/100
    5416/5416 [==============================] - 13s - loss: 2.0358 - acc: 0.2733 - val_loss: 5.7907 - val_acc: 0.1779
    Epoch 75/100
    5416/5416 [==============================] - 13s - loss: 2.0276 - acc: 0.2696 - val_loss: 5.7884 - val_acc: 0.1838
    Epoch 76/100
    5416/5416 [==============================] - 13s - loss: 2.0369 - acc: 0.2746 - val_loss: 5.7786 - val_acc: 0.1771
    Epoch 77/100
    5416/5416 [==============================] - 13s - loss: 2.0281 - acc: 0.2753 - val_loss: 5.5400 - val_acc: 0.1727
    Epoch 78/100
    5416/5416 [==============================] - 15s - loss: 2.0299 - acc: 0.2770 - val_loss: 5.7449 - val_acc: 0.1830
    Epoch 79/100
    5416/5416 [==============================] - 15s - loss: 2.0309 - acc: 0.2714 - val_loss: 5.6931 - val_acc: 0.1830
    Epoch 80/100
    5416/5416 [==============================] - 13s - loss: 2.0297 - acc: 0.2725 - val_loss: 5.9119 - val_acc: 0.1749
    Epoch 81/100
    5416/5416 [==============================] - 12s - loss: 2.0295 - acc: 0.2699 - val_loss: 5.7339 - val_acc: 0.1882
    Epoch 82/100
    5416/5416 [==============================] - 12s - loss: 2.0334 - acc: 0.2733 - val_loss: 5.7446 - val_acc: 0.1793
    Epoch 83/100
    5416/5416 [==============================] - 12s - loss: 2.0309 - acc: 0.2722 - val_loss: 5.5654 - val_acc: 0.1793
    Epoch 84/100
    5416/5416 [==============================] - 12s - loss: 2.0288 - acc: 0.2747 - val_loss: 5.5971 - val_acc: 0.1793
    Epoch 85/100
    5416/5416 [==============================] - 13s - loss: 2.0260 - acc: 0.2722 - val_loss: 5.9274 - val_acc: 0.1712
    Epoch 86/100
    5416/5416 [==============================] - 13s - loss: 2.0345 - acc: 0.2747 - val_loss: 5.7473 - val_acc: 0.1875
    Epoch 87/100
    5416/5416 [==============================] - 12s - loss: 2.0306 - acc: 0.2733 - val_loss: 5.6444 - val_acc: 0.1801
    Epoch 88/100
    5416/5416 [==============================] - 13s - loss: 2.0295 - acc: 0.2753 - val_loss: 5.7132 - val_acc: 0.1742
    Epoch 89/100
    5416/5416 [==============================] - 13s - loss: 2.0314 - acc: 0.2723 - val_loss: 5.7616 - val_acc: 0.1815
    Epoch 90/100
    5416/5416 [==============================] - 13s - loss: 2.0344 - acc: 0.2736 - val_loss: 5.4261 - val_acc: 0.1734
    Epoch 91/100
    5416/5416 [==============================] - 13s - loss: 2.0299 - acc: 0.2670 - val_loss: 5.5118 - val_acc: 0.1801
    Epoch 92/100
    5416/5416 [==============================] - 13s - loss: 2.0327 - acc: 0.2749 - val_loss: 5.5915 - val_acc: 0.1779
    Epoch 93/100
    5416/5416 [==============================] - 14s - loss: 2.0240 - acc: 0.2782 - val_loss: 5.6751 - val_acc: 0.1838
    Epoch 94/100
    5416/5416 [==============================] - 13s - loss: 2.0363 - acc: 0.2723 - val_loss: 5.7031 - val_acc: 0.1712
    Epoch 95/100
    5416/5416 [==============================] - 12s - loss: 2.0290 - acc: 0.2712 - val_loss: 6.0503 - val_acc: 0.1742
    Epoch 96/100
    5416/5416 [==============================] - 13s - loss: 2.0261 - acc: 0.2733 - val_loss: 5.4784 - val_acc: 0.1779
    Epoch 97/100
    5416/5416 [==============================] - 12s - loss: 2.0278 - acc: 0.2746 - val_loss: 5.8021 - val_acc: 0.1801
    Epoch 98/100
    5416/5416 [==============================] - 13s - loss: 2.0308 - acc: 0.2722 - val_loss: 5.9849 - val_acc: 0.1749
    Epoch 99/100
    5416/5416 [==============================] - 12s - loss: 2.0305 - acc: 0.2727 - val_loss: 5.9407 - val_acc: 0.1845
    Epoch 100/100
    5416/5416 [==============================] - 12s - loss: 2.0315 - acc: 0.2725 - val_loss: 5.7821 - val_acc: 0.1889
    Train on 11709 samples, validate on 2928 samples
    Epoch 1/50
    11709/11709 [==============================] - 28s - loss: 4.6179 - acc: 0.0724 - val_loss: 4.2989 - val_acc: 0.1373
    Epoch 2/50
    11709/11709 [==============================] - 27s - loss: 4.3838 - acc: 0.0991 - val_loss: 4.2906 - val_acc: 0.0686
    Epoch 3/50
    11709/11709 [==============================] - 27s - loss: 4.2055 - acc: 0.1303 - val_loss: 4.1885 - val_acc: 0.1708
    Epoch 4/50
    11709/11709 [==============================] - 27s - loss: 3.9465 - acc: 0.1888 - val_loss: 4.0530 - val_acc: 0.1820
    Epoch 5/50
    11709/11709 [==============================] - 27s - loss: 3.7127 - acc: 0.2086 - val_loss: 3.9590 - val_acc: 0.1957
    Epoch 6/50
    11709/11709 [==============================] - 28s - loss: 3.5082 - acc: 0.2259 - val_loss: 3.8788 - val_acc: 0.2005
    Epoch 7/50
    11709/11709 [==============================] - 27s - loss: 3.3358 - acc: 0.2327 - val_loss: 3.8042 - val_acc: 0.2015
    Epoch 8/50
    11709/11709 [==============================] - 27s - loss: 3.1769 - acc: 0.2422 - val_loss: 3.8452 - val_acc: 0.2046
    Epoch 9/50
    11709/11709 [==============================] - 27s - loss: 3.0378 - acc: 0.2520 - val_loss: 3.8168 - val_acc: 0.2053
    Epoch 10/50
    11709/11709 [==============================] - 27s - loss: 2.9137 - acc: 0.2631 - val_loss: 3.8857 - val_acc: 0.2056
    Epoch 11/50
    11709/11709 [==============================] - 28s - loss: 2.8026 - acc: 0.2711 - val_loss: 3.8880 - val_acc: 0.2049
    Epoch 12/50
    11709/11709 [==============================] - 27s - loss: 2.7100 - acc: 0.2754 - val_loss: 3.9750 - val_acc: 0.2042
    Epoch 13/50
    11709/11709 [==============================] - 27s - loss: 2.6340 - acc: 0.2804 - val_loss: 4.0935 - val_acc: 0.1950
    Epoch 14/50
    11709/11709 [==============================] - 27s - loss: 2.5720 - acc: 0.2846 - val_loss: 4.0220 - val_acc: 0.2042
    Epoch 15/50
    11709/11709 [==============================] - 27s - loss: 2.5249 - acc: 0.2858 - val_loss: 4.0326 - val_acc: 0.2046
    Epoch 16/50
    11709/11709 [==============================] - 27s - loss: 2.4842 - acc: 0.2892 - val_loss: 4.0543 - val_acc: 0.2042
    Epoch 17/50
    11709/11709 [==============================] - 27s - loss: 2.4498 - acc: 0.2926 - val_loss: 4.1119 - val_acc: 0.2066
    Epoch 18/50
    11709/11709 [==============================] - 27s - loss: 2.4282 - acc: 0.2923 - val_loss: 4.2139 - val_acc: 0.2059
    Epoch 19/50
    11709/11709 [==============================] - 27s - loss: 2.4092 - acc: 0.2908 - val_loss: 4.3102 - val_acc: 0.2012
    Epoch 20/50
    11709/11709 [==============================] - 27s - loss: 2.3901 - acc: 0.2911 - val_loss: 4.1829 - val_acc: 0.2046
    Epoch 21/50
    11709/11709 [==============================] - 27s - loss: 2.3785 - acc: 0.2917 - val_loss: 4.2453 - val_acc: 0.2049
    Epoch 22/50
    11709/11709 [==============================] - 27s - loss: 2.3695 - acc: 0.2913 - val_loss: 4.3196 - val_acc: 0.2063
    Epoch 23/50
    11709/11709 [==============================] - 27s - loss: 2.3549 - acc: 0.2923 - val_loss: 4.2541 - val_acc: 0.2042
    Epoch 24/50
    11709/11709 [==============================] - 27s - loss: 2.3480 - acc: 0.2902 - val_loss: 4.2755 - val_acc: 0.1923
    Epoch 25/50
    11709/11709 [==============================] - 27s - loss: 2.3421 - acc: 0.2935 - val_loss: 4.2779 - val_acc: 0.2053
    Epoch 26/50
    11709/11709 [==============================] - 28s - loss: 2.3357 - acc: 0.2927 - val_loss: 4.3729 - val_acc: 0.2066
    Epoch 27/50
    11709/11709 [==============================] - 27s - loss: 2.3382 - acc: 0.2912 - val_loss: 4.2779 - val_acc: 0.2015
    Epoch 28/50
    11709/11709 [==============================] - 27s - loss: 2.3345 - acc: 0.2911 - val_loss: 4.2967 - val_acc: 0.2029
    Epoch 29/50
    11709/11709 [==============================] - 27s - loss: 2.3272 - acc: 0.2934 - val_loss: 4.4001 - val_acc: 0.2053
    Epoch 30/50
    11709/11709 [==============================] - 27s - loss: 2.3252 - acc: 0.2914 - val_loss: 4.3074 - val_acc: 0.2008
    Epoch 31/50
    11709/11709 [==============================] - 27s - loss: 2.3204 - acc: 0.2921 - val_loss: 4.4425 - val_acc: 0.2008
    Epoch 32/50
    11709/11709 [==============================] - 27s - loss: 2.3234 - acc: 0.2908 - val_loss: 4.4213 - val_acc: 0.2008
    Epoch 33/50
    11709/11709 [==============================] - 27s - loss: 2.3201 - acc: 0.2917 - val_loss: 4.3472 - val_acc: 0.1998
    Epoch 34/50
    11709/11709 [==============================] - 27s - loss: 2.3145 - acc: 0.2917 - val_loss: 4.3941 - val_acc: 0.2032
    Epoch 35/50
    11709/11709 [==============================] - 27s - loss: 2.3161 - acc: 0.2907 - val_loss: 4.3834 - val_acc: 0.2015
    Epoch 36/50
    11709/11709 [==============================] - 27s - loss: 2.3121 - acc: 0.2924 - val_loss: 4.3683 - val_acc: 0.1988
    Epoch 37/50
    11709/11709 [==============================] - 27s - loss: 2.3144 - acc: 0.2923 - val_loss: 4.3668 - val_acc: 0.2001
    Epoch 38/50
    11709/11709 [==============================] - 28s - loss: 2.3055 - acc: 0.2934 - val_loss: 4.3836 - val_acc: 0.1988
    Epoch 39/50
    11709/11709 [==============================] - 27s - loss: 2.3061 - acc: 0.2929 - val_loss: 4.4449 - val_acc: 0.2022
    Epoch 40/50
    11709/11709 [==============================] - 27s - loss: 2.3090 - acc: 0.2929 - val_loss: 4.3603 - val_acc: 0.1991
    Epoch 41/50
    11709/11709 [==============================] - 27s - loss: 2.3120 - acc: 0.2907 - val_loss: 4.4100 - val_acc: 0.2005
    Epoch 42/50
    11709/11709 [==============================] - 27s - loss: 2.3072 - acc: 0.2921 - val_loss: 4.4772 - val_acc: 0.2008
    Epoch 43/50
    11709/11709 [==============================] - 27s - loss: 2.3114 - acc: 0.2894 - val_loss: 4.4434 - val_acc: 0.2025
    Epoch 44/50
    11709/11709 [==============================] - 27s - loss: 2.2982 - acc: 0.2934 - val_loss: 4.4066 - val_acc: 0.1899
    Epoch 45/50
    11709/11709 [==============================] - 27s - loss: 2.3059 - acc: 0.2918 - val_loss: 4.5444 - val_acc: 0.1923
    Epoch 46/50
    11709/11709 [==============================] - 28s - loss: 2.3034 - acc: 0.2938 - val_loss: 4.4077 - val_acc: 0.1977
    Epoch 47/50
    11709/11709 [==============================] - 27s - loss: 2.3011 - acc: 0.2931 - val_loss: 4.4124 - val_acc: 0.1971
    Epoch 48/50
    11709/11709 [==============================] - 27s - loss: 2.2972 - acc: 0.2927 - val_loss: 4.5202 - val_acc: 0.2025
    Epoch 49/50
    11709/11709 [==============================] - 27s - loss: 2.3001 - acc: 0.2925 - val_loss: 4.5174 - val_acc: 0.2005
    Epoch 50/50
    11709/11709 [==============================] - 27s - loss: 2.3030 - acc: 0.2919 - val_loss: 4.4668 - val_acc: 0.2032
    Train on 3723 samples, validate on 931 samples
    Epoch 1/100
    3723/3723 [==============================] - 8s - loss: 4.3915 - acc: 0.0309 - val_loss: 4.1296 - val_acc: 0.0494
    Epoch 2/100
    3723/3723 [==============================] - 9s - loss: 4.0610 - acc: 0.0642 - val_loss: 4.1530 - val_acc: 0.0730
    Epoch 3/100
    3723/3723 [==============================] - 9s - loss: 4.0158 - acc: 0.0814 - val_loss: 4.2079 - val_acc: 0.0730
    Epoch 4/100
    3723/3723 [==============================] - 10s - loss: 3.9840 - acc: 0.0814 - val_loss: 4.2340 - val_acc: 0.0730
    Epoch 5/100
    3723/3723 [==============================] - 9s - loss: 3.9418 - acc: 0.0814 - val_loss: 4.2634 - val_acc: 0.0730
    Epoch 6/100
    3723/3723 [==============================] - 8s - loss: 3.9107 - acc: 0.0814 - val_loss: 4.3290 - val_acc: 0.0730
    Epoch 7/100
    3723/3723 [==============================] - 8s - loss: 3.8572 - acc: 0.0889 - val_loss: 4.3935 - val_acc: 0.1117
    Epoch 8/100
    3723/3723 [==============================] - 8s - loss: 3.7788 - acc: 0.1381 - val_loss: 4.4175 - val_acc: 0.1235
    Epoch 9/100
    3723/3723 [==============================] - 8s - loss: 3.6960 - acc: 0.1469 - val_loss: 4.3751 - val_acc: 0.1117
    Epoch 10/100
    3723/3723 [==============================] - 8s - loss: 3.5961 - acc: 0.1708 - val_loss: 4.3270 - val_acc: 0.1353
    Epoch 11/100
    3723/3723 [==============================] - 8s - loss: 3.5175 - acc: 0.1789 - val_loss: 4.2932 - val_acc: 0.1375
    Epoch 12/100
    3723/3723 [==============================] - 8s - loss: 3.4356 - acc: 0.1929 - val_loss: 4.2182 - val_acc: 0.1600
    Epoch 13/100
    3723/3723 [==============================] - 8s - loss: 3.3517 - acc: 0.1982 - val_loss: 4.2027 - val_acc: 0.1676
    Epoch 14/100
    3723/3723 [==============================] - 8s - loss: 3.2685 - acc: 0.2071 - val_loss: 4.1985 - val_acc: 0.1600
    Epoch 15/100
    3723/3723 [==============================] - 8s - loss: 3.1890 - acc: 0.2114 - val_loss: 4.1439 - val_acc: 0.1643
    Epoch 16/100
    3723/3723 [==============================] - 8s - loss: 3.1125 - acc: 0.2184 - val_loss: 4.1336 - val_acc: 0.1654
    Epoch 17/100
    3723/3723 [==============================] - 8s - loss: 3.0449 - acc: 0.2259 - val_loss: 4.0528 - val_acc: 0.1708
    Epoch 18/100
    3723/3723 [==============================] - 8s - loss: 2.9737 - acc: 0.2321 - val_loss: 4.0897 - val_acc: 0.1740
    Epoch 19/100
    3723/3723 [==============================] - 8s - loss: 2.8999 - acc: 0.2380 - val_loss: 4.0556 - val_acc: 0.1729
    Epoch 20/100
    3723/3723 [==============================] - 8s - loss: 2.8396 - acc: 0.2458 - val_loss: 4.0452 - val_acc: 0.1751
    Epoch 21/100
    3723/3723 [==============================] - 8s - loss: 2.7690 - acc: 0.2487 - val_loss: 4.0557 - val_acc: 0.1729
    Epoch 22/100
    3723/3723 [==============================] - 8s - loss: 2.7131 - acc: 0.2562 - val_loss: 4.0228 - val_acc: 0.1719
    Epoch 23/100
    3723/3723 [==============================] - 8s - loss: 2.6530 - acc: 0.2622 - val_loss: 4.0194 - val_acc: 0.1719
    Epoch 24/100
    3723/3723 [==============================] - 8s - loss: 2.5920 - acc: 0.2667 - val_loss: 3.9820 - val_acc: 0.1729
    Epoch 25/100
    3723/3723 [==============================] - 8s - loss: 2.5370 - acc: 0.2759 - val_loss: 4.0198 - val_acc: 0.1719
    Epoch 26/100
    3723/3723 [==============================] - 9s - loss: 2.4785 - acc: 0.2783 - val_loss: 4.0760 - val_acc: 0.1751
    Epoch 27/100
    3723/3723 [==============================] - 8s - loss: 2.4290 - acc: 0.2826 - val_loss: 4.0041 - val_acc: 0.1751
    Epoch 28/100
    3723/3723 [==============================] - 8s - loss: 2.3762 - acc: 0.2871 - val_loss: 4.0638 - val_acc: 0.1665
    Epoch 29/100
    3723/3723 [==============================] - 8s - loss: 2.3273 - acc: 0.2912 - val_loss: 4.0842 - val_acc: 0.1729
    Epoch 30/100
    3723/3723 [==============================] - 9s - loss: 2.2821 - acc: 0.2949 - val_loss: 4.1316 - val_acc: 0.1643
    Epoch 31/100
    3723/3723 [==============================] - 8s - loss: 2.2399 - acc: 0.2912 - val_loss: 4.1788 - val_acc: 0.1740
    Epoch 32/100
    3723/3723 [==============================] - 8s - loss: 2.2031 - acc: 0.2957 - val_loss: 4.1272 - val_acc: 0.1686
    Epoch 33/100
    3723/3723 [==============================] - 8s - loss: 2.1590 - acc: 0.2990 - val_loss: 4.1713 - val_acc: 0.1719
    Epoch 34/100
    3723/3723 [==============================] - 8s - loss: 2.1235 - acc: 0.3006 - val_loss: 4.0760 - val_acc: 0.1611
    Epoch 35/100
    3723/3723 [==============================] - 8s - loss: 2.0936 - acc: 0.3035 - val_loss: 4.1340 - val_acc: 0.1686
    Epoch 36/100
    3723/3723 [==============================] - 8s - loss: 2.0638 - acc: 0.3041 - val_loss: 4.2239 - val_acc: 0.1751
    Epoch 37/100
    3723/3723 [==============================] - 8s - loss: 2.0372 - acc: 0.3086 - val_loss: 4.2553 - val_acc: 0.1762
    Epoch 38/100
    3723/3723 [==============================] - 8s - loss: 2.0065 - acc: 0.3094 - val_loss: 4.2648 - val_acc: 0.1686
    Epoch 39/100
    3723/3723 [==============================] - 8s - loss: 1.9858 - acc: 0.3159 - val_loss: 4.2885 - val_acc: 0.1708
    Epoch 40/100
    3723/3723 [==============================] - 8s - loss: 1.9601 - acc: 0.3145 - val_loss: 4.3067 - val_acc: 0.1697
    Epoch 41/100
    3723/3723 [==============================] - 8s - loss: 1.9449 - acc: 0.3172 - val_loss: 4.2585 - val_acc: 0.1654
    Epoch 42/100
    3723/3723 [==============================] - 8s - loss: 1.9261 - acc: 0.3116 - val_loss: 4.3439 - val_acc: 0.1654
    Epoch 43/100
    3723/3723 [==============================] - 8s - loss: 1.9029 - acc: 0.3127 - val_loss: 4.4102 - val_acc: 0.1751
    Epoch 44/100
    3723/3723 [==============================] - 8s - loss: 1.8976 - acc: 0.3100 - val_loss: 4.3908 - val_acc: 0.1708
    Epoch 45/100
    3723/3723 [==============================] - 8s - loss: 1.8760 - acc: 0.3094 - val_loss: 4.3926 - val_acc: 0.1643
    Epoch 46/100
    3723/3723 [==============================] - 8s - loss: 1.8702 - acc: 0.3156 - val_loss: 4.4345 - val_acc: 0.1686
    Epoch 47/100
    3723/3723 [==============================] - 9s - loss: 1.8578 - acc: 0.3118 - val_loss: 4.4250 - val_acc: 0.1686
    Epoch 48/100
    3723/3723 [==============================] - 8s - loss: 1.8518 - acc: 0.3054 - val_loss: 4.3859 - val_acc: 0.1525
    Epoch 49/100
    3723/3723 [==============================] - 9s - loss: 1.8455 - acc: 0.3110 - val_loss: 4.5548 - val_acc: 0.1762
    Epoch 50/100
    3723/3723 [==============================] - 8s - loss: 1.8388 - acc: 0.3043 - val_loss: 4.5080 - val_acc: 0.1643
    Epoch 51/100
    3723/3723 [==============================] - 8s - loss: 1.8315 - acc: 0.3097 - val_loss: 4.5879 - val_acc: 0.1697
    Epoch 52/100
    3723/3723 [==============================] - 8s - loss: 1.8225 - acc: 0.3030 - val_loss: 4.5075 - val_acc: 0.1493
    Epoch 53/100
    3723/3723 [==============================] - 8s - loss: 1.8239 - acc: 0.3108 - val_loss: 4.5590 - val_acc: 0.1633
    Epoch 54/100
    3723/3723 [==============================] - 8s - loss: 1.8076 - acc: 0.3110 - val_loss: 4.7617 - val_acc: 0.1762
    Epoch 55/100
    3723/3723 [==============================] - 9s - loss: 1.8186 - acc: 0.3070 - val_loss: 4.5710 - val_acc: 0.1547
    Epoch 56/100
    3723/3723 [==============================] - 8s - loss: 1.8092 - acc: 0.3038 - val_loss: 4.7421 - val_acc: 0.1826
    Epoch 57/100
    3723/3723 [==============================] - 8s - loss: 1.8144 - acc: 0.3049 - val_loss: 4.6491 - val_acc: 0.1643
    Epoch 58/100
    3723/3723 [==============================] - 9s - loss: 1.7999 - acc: 0.3049 - val_loss: 4.7076 - val_acc: 0.1708
    Epoch 59/100
    3723/3723 [==============================] - 9s - loss: 1.8042 - acc: 0.3051 - val_loss: 4.7302 - val_acc: 0.1708
    Epoch 60/100
    3723/3723 [==============================] - 9s - loss: 1.7987 - acc: 0.3062 - val_loss: 4.7115 - val_acc: 0.1676
    Epoch 61/100
    3723/3723 [==============================] - 8s - loss: 1.8001 - acc: 0.3051 - val_loss: 4.7196 - val_acc: 0.1719
    Epoch 62/100
    3723/3723 [==============================] - 8s - loss: 1.7922 - acc: 0.3057 - val_loss: 4.8497 - val_acc: 0.1697
    Epoch 63/100
    3723/3723 [==============================] - 8s - loss: 1.7969 - acc: 0.3108 - val_loss: 4.8451 - val_acc: 0.1772
    Epoch 64/100
    3723/3723 [==============================] - 8s - loss: 1.7948 - acc: 0.3043 - val_loss: 4.7399 - val_acc: 0.1643
    Epoch 65/100
    3723/3723 [==============================] - 8s - loss: 1.7911 - acc: 0.3041 - val_loss: 4.7548 - val_acc: 0.1697
    Epoch 66/100
    3723/3723 [==============================] - 9s - loss: 1.7960 - acc: 0.3024 - val_loss: 4.8197 - val_acc: 0.1676
    Epoch 67/100
    3723/3723 [==============================] - 8s - loss: 1.7846 - acc: 0.3049 - val_loss: 4.7509 - val_acc: 0.1708
    Epoch 68/100
    3723/3723 [==============================] - 8s - loss: 1.7878 - acc: 0.3084 - val_loss: 4.7882 - val_acc: 0.1729
    Epoch 69/100
    3723/3723 [==============================] - 8s - loss: 1.7937 - acc: 0.3073 - val_loss: 4.8213 - val_acc: 0.1729
    Epoch 70/100
    3723/3723 [==============================] - 8s - loss: 1.7831 - acc: 0.3057 - val_loss: 4.7351 - val_acc: 0.1665
    Epoch 71/100
    3723/3723 [==============================] - 8s - loss: 1.7929 - acc: 0.3097 - val_loss: 4.7378 - val_acc: 0.1579
    Epoch 72/100
    3723/3723 [==============================] - 8s - loss: 1.7866 - acc: 0.3054 - val_loss: 4.8023 - val_acc: 0.1708
    Epoch 73/100
    3723/3723 [==============================] - 8s - loss: 1.7869 - acc: 0.3046 - val_loss: 4.7604 - val_acc: 0.1611
    Epoch 74/100
    3723/3723 [==============================] - 8s - loss: 1.7891 - acc: 0.3049 - val_loss: 4.8210 - val_acc: 0.1708
    Epoch 75/100
    3723/3723 [==============================] - 8s - loss: 1.7840 - acc: 0.3059 - val_loss: 4.9351 - val_acc: 0.1751
    Epoch 76/100
    3723/3723 [==============================] - 8s - loss: 1.7895 - acc: 0.3016 - val_loss: 4.7275 - val_acc: 0.1633
    Epoch 77/100
    3723/3723 [==============================] - 8s - loss: 1.7824 - acc: 0.3030 - val_loss: 4.8608 - val_acc: 0.1751
    Epoch 78/100
    3723/3723 [==============================] - 8s - loss: 1.7869 - acc: 0.3035 - val_loss: 4.7697 - val_acc: 0.1568
    Epoch 79/100
    3723/3723 [==============================] - 8s - loss: 1.7853 - acc: 0.3108 - val_loss: 4.8027 - val_acc: 0.1654
    Epoch 80/100
    3723/3723 [==============================] - 8s - loss: 1.7925 - acc: 0.3062 - val_loss: 4.8818 - val_acc: 0.1536
    Epoch 81/100
    3723/3723 [==============================] - 8s - loss: 1.7754 - acc: 0.3035 - val_loss: 4.9331 - val_acc: 0.1772
    Epoch 82/100
    3723/3723 [==============================] - 8s - loss: 1.7792 - acc: 0.3057 - val_loss: 4.8158 - val_acc: 0.1504
    Epoch 83/100
    3723/3723 [==============================] - 8s - loss: 1.7862 - acc: 0.3062 - val_loss: 4.9009 - val_acc: 0.1611
    Epoch 84/100
    3723/3723 [==============================] - 8s - loss: 1.7746 - acc: 0.3092 - val_loss: 4.8677 - val_acc: 0.1762
    Epoch 85/100
    3723/3723 [==============================] - 9s - loss: 1.7779 - acc: 0.3054 - val_loss: 4.8512 - val_acc: 0.1547
    Epoch 86/100
    3723/3723 [==============================] - 8s - loss: 1.7847 - acc: 0.3067 - val_loss: 4.8604 - val_acc: 0.1708
    Epoch 87/100
    3723/3723 [==============================] - 8s - loss: 1.7793 - acc: 0.3075 - val_loss: 4.9535 - val_acc: 0.1729
    Epoch 88/100
    3723/3723 [==============================] - 8s - loss: 1.7802 - acc: 0.3041 - val_loss: 4.7213 - val_acc: 0.1686
    Epoch 89/100
    3723/3723 [==============================] - 8s - loss: 1.7768 - acc: 0.3075 - val_loss: 4.8783 - val_acc: 0.1762
    Epoch 90/100
    3723/3723 [==============================] - 8s - loss: 1.7789 - acc: 0.3041 - val_loss: 4.9059 - val_acc: 0.1740
    Epoch 91/100
    3723/3723 [==============================] - 8s - loss: 1.7801 - acc: 0.3089 - val_loss: 4.7526 - val_acc: 0.1547
    Epoch 92/100
    3723/3723 [==============================] - 8s - loss: 1.7763 - acc: 0.3092 - val_loss: 4.7783 - val_acc: 0.1654
    Epoch 93/100
    3723/3723 [==============================] - 8s - loss: 1.7762 - acc: 0.3059 - val_loss: 4.8701 - val_acc: 0.1708
    Epoch 94/100
    3723/3723 [==============================] - 8s - loss: 1.7742 - acc: 0.3110 - val_loss: 4.8465 - val_acc: 0.1633
    Epoch 95/100
    3723/3723 [==============================] - 8s - loss: 1.7766 - acc: 0.3054 - val_loss: 4.8990 - val_acc: 0.1665
    Epoch 96/100
    3723/3723 [==============================] - 8s - loss: 1.7716 - acc: 0.3110 - val_loss: 4.8630 - val_acc: 0.1762
    Epoch 97/100
    3723/3723 [==============================] - 8s - loss: 1.7828 - acc: 0.3067 - val_loss: 4.8492 - val_acc: 0.1568
    Epoch 98/100
    3723/3723 [==============================] - 8s - loss: 1.7672 - acc: 0.3067 - val_loss: 4.6980 - val_acc: 0.1622
    Epoch 99/100
    3723/3723 [==============================] - 8s - loss: 1.7862 - acc: 0.3086 - val_loss: 4.9222 - val_acc: 0.1719
    Epoch 100/100
    3723/3723 [==============================] - 9s - loss: 1.7676 - acc: 0.3113 - val_loss: 4.8230 - val_acc: 0.1611
    Train on 5179 samples, validate on 1295 samples
    Epoch 1/100
    5179/5179 [==============================] - 12s - loss: 4.4711 - acc: 0.0272 - val_loss: 4.6353 - val_acc: 0.0355
    Epoch 2/100
    5179/5179 [==============================] - 12s - loss: 4.1705 - acc: 0.0828 - val_loss: 4.6964 - val_acc: 0.0795
    Epoch 3/100
    5179/5179 [==============================] - 12s - loss: 4.1100 - acc: 0.0855 - val_loss: 4.7488 - val_acc: 0.0795
    Epoch 4/100
    5179/5179 [==============================] - 12s - loss: 4.0570 - acc: 0.0931 - val_loss: 4.8133 - val_acc: 0.0795
    Epoch 5/100
    5179/5179 [==============================] - 12s - loss: 3.9888 - acc: 0.1014 - val_loss: 4.8351 - val_acc: 0.0795
    Epoch 6/100
    5179/5179 [==============================] - 12s - loss: 3.8770 - acc: 0.1527 - val_loss: 4.8333 - val_acc: 0.1707
    Epoch 7/100
    5179/5179 [==============================] - 12s - loss: 3.7466 - acc: 0.1782 - val_loss: 4.8909 - val_acc: 0.1683
    Epoch 8/100
    5179/5179 [==============================] - 12s - loss: 3.6152 - acc: 0.1912 - val_loss: 4.8499 - val_acc: 0.1753
    Epoch 9/100
    5179/5179 [==============================] - 12s - loss: 3.4916 - acc: 0.1979 - val_loss: 4.7304 - val_acc: 0.1714
    Epoch 10/100
    5179/5179 [==============================] - 12s - loss: 3.3738 - acc: 0.2060 - val_loss: 4.7101 - val_acc: 0.1768
    Epoch 11/100
    5179/5179 [==============================] - 12s - loss: 3.2659 - acc: 0.2168 - val_loss: 4.5853 - val_acc: 0.1768
    Epoch 12/100
    5179/5179 [==============================] - 12s - loss: 3.1580 - acc: 0.2215 - val_loss: 4.6055 - val_acc: 0.1838
    Epoch 13/100
    5179/5179 [==============================] - 12s - loss: 3.0593 - acc: 0.2360 - val_loss: 4.5943 - val_acc: 0.1830
    Epoch 14/100
    5179/5179 [==============================] - 12s - loss: 2.9670 - acc: 0.2402 - val_loss: 4.6076 - val_acc: 0.1830
    Epoch 15/100
    5179/5179 [==============================] - 12s - loss: 2.8777 - acc: 0.2518 - val_loss: 4.5718 - val_acc: 0.1853
    Epoch 16/100
    5179/5179 [==============================] - 12s - loss: 2.7916 - acc: 0.2595 - val_loss: 4.5158 - val_acc: 0.1892
    Epoch 17/100
    5179/5179 [==============================] - 12s - loss: 2.7088 - acc: 0.2740 - val_loss: 4.4648 - val_acc: 0.1861
    Epoch 18/100
    5179/5179 [==============================] - 12s - loss: 2.6317 - acc: 0.2784 - val_loss: 4.4648 - val_acc: 0.1884
    Epoch 19/100
    5179/5179 [==============================] - 12s - loss: 2.5589 - acc: 0.2875 - val_loss: 4.4382 - val_acc: 0.1884
    Epoch 20/100
    5179/5179 [==============================] - 12s - loss: 2.4845 - acc: 0.2945 - val_loss: 4.4737 - val_acc: 0.1907
    Epoch 21/100
    5179/5179 [==============================] - 12s - loss: 2.4198 - acc: 0.2985 - val_loss: 4.6298 - val_acc: 0.1907
    Epoch 22/100
    5179/5179 [==============================] - 12s - loss: 2.3580 - acc: 0.3008 - val_loss: 4.5946 - val_acc: 0.1900
    Epoch 23/100
    5179/5179 [==============================] - 12s - loss: 2.3000 - acc: 0.3049 - val_loss: 4.5508 - val_acc: 0.1915
    Epoch 24/100
    5179/5179 [==============================] - 12s - loss: 2.2464 - acc: 0.3068 - val_loss: 4.5326 - val_acc: 0.1946
    Epoch 25/100
    5179/5179 [==============================] - 12s - loss: 2.1905 - acc: 0.3140 - val_loss: 4.5421 - val_acc: 0.1907
    Epoch 26/100
    5179/5179 [==============================] - 12s - loss: 2.1487 - acc: 0.3178 - val_loss: 4.6086 - val_acc: 0.1923
    Epoch 27/100
    5179/5179 [==============================] - 12s - loss: 2.1051 - acc: 0.3180 - val_loss: 4.6876 - val_acc: 0.1907
    Epoch 28/100
    5179/5179 [==============================] - 12s - loss: 2.0676 - acc: 0.3184 - val_loss: 4.7328 - val_acc: 0.1931
    Epoch 29/100
    5179/5179 [==============================] - 12s - loss: 2.0355 - acc: 0.3163 - val_loss: 4.7234 - val_acc: 0.1969
    Epoch 30/100
    5179/5179 [==============================] - 12s - loss: 2.0030 - acc: 0.3209 - val_loss: 4.6757 - val_acc: 0.1931
    Epoch 31/100
    5179/5179 [==============================] - 12s - loss: 1.9774 - acc: 0.3267 - val_loss: 4.7624 - val_acc: 0.1907
    Epoch 32/100
    5179/5179 [==============================] - 12s - loss: 1.9529 - acc: 0.3215 - val_loss: 4.9670 - val_acc: 0.1954
    Epoch 33/100
    5179/5179 [==============================] - 12s - loss: 1.9327 - acc: 0.3236 - val_loss: 4.9051 - val_acc: 0.1923
    Epoch 34/100
    5179/5179 [==============================] - 12s - loss: 1.9143 - acc: 0.3230 - val_loss: 5.0180 - val_acc: 0.1946
    Epoch 35/100
    5179/5179 [==============================] - 12s - loss: 1.8926 - acc: 0.3186 - val_loss: 5.0046 - val_acc: 0.1931
    Epoch 36/100
    5179/5179 [==============================] - 12s - loss: 1.8858 - acc: 0.3196 - val_loss: 5.0148 - val_acc: 0.1946
    Epoch 37/100
    5179/5179 [==============================] - 12s - loss: 1.8665 - acc: 0.3236 - val_loss: 5.0047 - val_acc: 0.1915
    Epoch 38/100
    5179/5179 [==============================] - 12s - loss: 1.8590 - acc: 0.3215 - val_loss: 5.0311 - val_acc: 0.1923
    Epoch 39/100
    5179/5179 [==============================] - 12s - loss: 1.8491 - acc: 0.3192 - val_loss: 5.1100 - val_acc: 0.1923
    Epoch 40/100
    5179/5179 [==============================] - 12s - loss: 1.8438 - acc: 0.3186 - val_loss: 5.0575 - val_acc: 0.1907
    Epoch 41/100
    5179/5179 [==============================] - 12s - loss: 1.8364 - acc: 0.3174 - val_loss: 5.2963 - val_acc: 0.1946
    Epoch 42/100
    5179/5179 [==============================] - 12s - loss: 1.8329 - acc: 0.3169 - val_loss: 5.1625 - val_acc: 0.1907
    Epoch 43/100
    5179/5179 [==============================] - 12s - loss: 1.8249 - acc: 0.3165 - val_loss: 5.3708 - val_acc: 0.1938
    Epoch 44/100
    5179/5179 [==============================] - 12s - loss: 1.8217 - acc: 0.3172 - val_loss: 5.3022 - val_acc: 0.1961
    Epoch 45/100
    5179/5179 [==============================] - 12s - loss: 1.8195 - acc: 0.3172 - val_loss: 5.1743 - val_acc: 0.1846
    Epoch 46/100
    5179/5179 [==============================] - 12s - loss: 1.8127 - acc: 0.3199 - val_loss: 5.1993 - val_acc: 0.1699
    Epoch 47/100
    5179/5179 [==============================] - 12s - loss: 1.8184 - acc: 0.3136 - val_loss: 5.3514 - val_acc: 0.1722
    Epoch 48/100
    5179/5179 [==============================] - 12s - loss: 1.8098 - acc: 0.3159 - val_loss: 5.3577 - val_acc: 0.1730
    Epoch 49/100
    5179/5179 [==============================] - 12s - loss: 1.8017 - acc: 0.3184 - val_loss: 5.2429 - val_acc: 0.1668
    Epoch 50/100
    5179/5179 [==============================] - 12s - loss: 1.8097 - acc: 0.3170 - val_loss: 5.3002 - val_acc: 0.1691
    Epoch 51/100
    5179/5179 [==============================] - 12s - loss: 1.8054 - acc: 0.3180 - val_loss: 5.3949 - val_acc: 0.1699
    Epoch 52/100
    5179/5179 [==============================] - 12s - loss: 1.8043 - acc: 0.3153 - val_loss: 5.4389 - val_acc: 0.1768
    Epoch 53/100
    5179/5179 [==============================] - 12s - loss: 1.8026 - acc: 0.3180 - val_loss: 5.3628 - val_acc: 0.1653
    Epoch 54/100
    5179/5179 [==============================] - 12s - loss: 1.8032 - acc: 0.3184 - val_loss: 5.4900 - val_acc: 0.1730
    Epoch 55/100
    5179/5179 [==============================] - 12s - loss: 1.8003 - acc: 0.3167 - val_loss: 5.4549 - val_acc: 0.1714
    Epoch 56/100
    5179/5179 [==============================] - 12s - loss: 1.8045 - acc: 0.3201 - val_loss: 5.4792 - val_acc: 0.1668
    Epoch 57/100
    5179/5179 [==============================] - 12s - loss: 1.7927 - acc: 0.3186 - val_loss: 5.4941 - val_acc: 0.1722
    Epoch 58/100
    5179/5179 [==============================] - 12s - loss: 1.8020 - acc: 0.3180 - val_loss: 5.6655 - val_acc: 0.1768
    Epoch 59/100
    5179/5179 [==============================] - 12s - loss: 1.7957 - acc: 0.3128 - val_loss: 5.4014 - val_acc: 0.1691
    Epoch 60/100
    5179/5179 [==============================] - 12s - loss: 1.7965 - acc: 0.3157 - val_loss: 5.7073 - val_acc: 0.1714
    Epoch 61/100
    5179/5179 [==============================] - 12s - loss: 1.7956 - acc: 0.3149 - val_loss: 5.3863 - val_acc: 0.1676
    Epoch 62/100
    5179/5179 [==============================] - 12s - loss: 1.7944 - acc: 0.3186 - val_loss: 5.2339 - val_acc: 0.1645
    Epoch 63/100
    5179/5179 [==============================] - 12s - loss: 1.7955 - acc: 0.3174 - val_loss: 5.4744 - val_acc: 0.1622
    Epoch 64/100
    5179/5179 [==============================] - 12s - loss: 1.7939 - acc: 0.3149 - val_loss: 5.4405 - val_acc: 0.1707
    Epoch 65/100
    5179/5179 [==============================] - 12s - loss: 1.7922 - acc: 0.3192 - val_loss: 5.6537 - val_acc: 0.1691
    Epoch 66/100
    5179/5179 [==============================] - 12s - loss: 1.7925 - acc: 0.3145 - val_loss: 5.3728 - val_acc: 0.1614
    Epoch 67/100
    5179/5179 [==============================] - 12s - loss: 1.7857 - acc: 0.3174 - val_loss: 5.4628 - val_acc: 0.1714
    Epoch 68/100
    5179/5179 [==============================] - 12s - loss: 1.7939 - acc: 0.3155 - val_loss: 5.3432 - val_acc: 0.1668
    Epoch 69/100
    5179/5179 [==============================] - 13s - loss: 1.7896 - acc: 0.3190 - val_loss: 5.4907 - val_acc: 0.1591
    Epoch 70/100
    5179/5179 [==============================] - 13s - loss: 1.7935 - acc: 0.3145 - val_loss: 5.3978 - val_acc: 0.1614
    Epoch 71/100
    5179/5179 [==============================] - 12s - loss: 1.7933 - acc: 0.3180 - val_loss: 5.3401 - val_acc: 0.1653
    Epoch 72/100
    5179/5179 [==============================] - 12s - loss: 1.7917 - acc: 0.3172 - val_loss: 5.3269 - val_acc: 0.1645
    Epoch 73/100
    5179/5179 [==============================] - 12s - loss: 1.7999 - acc: 0.3203 - val_loss: 5.5115 - val_acc: 0.1637
    Epoch 74/100
    5179/5179 [==============================] - 12s - loss: 1.7960 - acc: 0.3182 - val_loss: 5.4821 - val_acc: 0.1683
    Epoch 75/100
    5179/5179 [==============================] - 12s - loss: 1.7857 - acc: 0.3165 - val_loss: 5.2796 - val_acc: 0.1676
    Epoch 76/100
    5179/5179 [==============================] - 12s - loss: 1.7919 - acc: 0.3159 - val_loss: 5.4039 - val_acc: 0.1660
    Epoch 77/100
    5179/5179 [==============================] - 12s - loss: 1.7851 - acc: 0.3186 - val_loss: 5.6795 - val_acc: 0.1660
    Epoch 78/100
    5179/5179 [==============================] - 12s - loss: 1.7970 - acc: 0.3145 - val_loss: 5.4375 - val_acc: 0.1668
    Epoch 79/100
    5179/5179 [==============================] - 12s - loss: 1.7861 - acc: 0.3178 - val_loss: 5.6418 - val_acc: 0.1714
    Epoch 80/100
    5179/5179 [==============================] - 12s - loss: 1.7921 - acc: 0.3188 - val_loss: 5.7017 - val_acc: 0.1614
    Epoch 81/100
    5179/5179 [==============================] - 12s - loss: 1.7860 - acc: 0.3176 - val_loss: 5.3758 - val_acc: 0.1653
    Epoch 82/100
    5179/5179 [==============================] - 12s - loss: 1.7872 - acc: 0.3165 - val_loss: 5.5117 - val_acc: 0.1653
    Epoch 83/100
    5179/5179 [==============================] - 12s - loss: 1.7978 - acc: 0.3182 - val_loss: 5.6291 - val_acc: 0.1699
    Epoch 84/100
    5179/5179 [==============================] - 12s - loss: 1.7837 - acc: 0.3153 - val_loss: 5.5220 - val_acc: 0.1722
    Epoch 85/100
    5179/5179 [==============================] - 12s - loss: 1.7912 - acc: 0.3221 - val_loss: 5.6547 - val_acc: 0.1714
    Epoch 86/100
    5179/5179 [==============================] - 12s - loss: 1.7869 - acc: 0.3196 - val_loss: 5.6178 - val_acc: 0.1676
    Epoch 87/100
    5179/5179 [==============================] - 12s - loss: 1.7836 - acc: 0.3174 - val_loss: 5.4130 - val_acc: 0.1699
    Epoch 88/100
    5179/5179 [==============================] - 12s - loss: 1.7942 - acc: 0.3196 - val_loss: 5.6607 - val_acc: 0.1730
    Epoch 89/100
    5179/5179 [==============================] - 12s - loss: 1.7843 - acc: 0.3184 - val_loss: 5.4949 - val_acc: 0.1660
    Epoch 90/100
    5179/5179 [==============================] - 12s - loss: 1.7833 - acc: 0.3184 - val_loss: 5.3709 - val_acc: 0.1645
    Epoch 91/100
    5179/5179 [==============================] - 12s - loss: 1.7838 - acc: 0.3172 - val_loss: 5.7736 - val_acc: 0.1722
    Epoch 92/100
    5179/5179 [==============================] - 12s - loss: 1.7883 - acc: 0.3174 - val_loss: 5.8756 - val_acc: 0.1737
    Epoch 93/100
    5179/5179 [==============================] - 12s - loss: 1.7882 - acc: 0.3178 - val_loss: 5.5688 - val_acc: 0.1676
    Epoch 94/100
    5179/5179 [==============================] - 12s - loss: 1.7855 - acc: 0.3186 - val_loss: 5.6821 - val_acc: 0.1691
    Epoch 95/100
    5179/5179 [==============================] - 12s - loss: 1.7907 - acc: 0.3163 - val_loss: 5.5861 - val_acc: 0.1637
    Epoch 96/100
    5179/5179 [==============================] - 12s - loss: 1.7911 - acc: 0.3217 - val_loss: 5.6749 - val_acc: 0.1614
    Epoch 97/100
    5179/5179 [==============================] - 12s - loss: 1.7874 - acc: 0.3178 - val_loss: 5.6304 - val_acc: 0.1668
    Epoch 98/100
    5179/5179 [==============================] - 12s - loss: 1.7806 - acc: 0.3213 - val_loss: 5.5019 - val_acc: 0.1629
    Epoch 99/100
    5179/5179 [==============================] - 12s - loss: 1.7883 - acc: 0.3209 - val_loss: 5.6242 - val_acc: 0.1637
    Epoch 100/100
    5179/5179 [==============================] - 12s - loss: 1.7938 - acc: 0.3207 - val_loss: 5.6013 - val_acc: 0.1629
    

Creating backup files for the models:


    for i,model in enumerate(models):
        if(i==2):
            model.save_weights('model' + str(i) + '_weights_50.h5')
        else:
            model.save_weights('model' + str(i) + '_weights_100.h5')

Load the models from backup files:


    for i,model in enumerate(models):
        if(i==2):
            model.load_weights('model' + str(i) + '_weights_50.h5')
        else:
            model.load_weights('model' + str(i) + '_weights_100.h5')

Reviewing model scores:


    index = 0
    scores = []
    for model in models:
        scores.append(model.evaluate(inputs[index],outputs[index], verbose=0))
        index += 1    


    scores




    [[2.7024388253168499, 0.32322987392327951],
     [2.7274777754390809, 0.26997489290810672],
     [2.683419111884747, 0.28038532486674222],
     [2.3568372251037677, 0.29114740009555296],
     [2.5076443227603549, 0.2974976830421534]]



### Generating new text

We define a function that given a word,token,model and a vocabulary will return the next word in an undeterministic fashion:
The function will choose one of the words the are in the top 1% in terms of probability, to follow the given word.
the 0.99 parameter can be replaced to create stronger/weaker relations between two adjacent words.
However, the higher the parameter the less options there are for a following word, i.e more deterministic and more posts that will be similar.


    import random
    def get_next_non_deter(text,token,model,vocabulary):
        #converting the word to 1-hot matrix represenation
        tmp = text_to_word_sequence(text, lower=False, split=" ")
        tmp = token.texts_to_matrix(tmp, mode='binary')
        #predicting next word
        sortedProbas = sorted(model.predict(tmp)[0])
        possibleWords = sortedProbas[int(len(sortedProbas) * .99) :]
        selectedWordIndex=random.randint(0, len(possibleWords)-1)
        wordOriginalIndex = -1
        for i,val in enumerate(model.predict(tmp)[0]):
                if(possibleWords[selectedWordIndex] == val):
                    wordOriginalIndex = i
        if(wordOriginalIndex == -1):
             return get_next_non_deter(text,token,model,vocabulary)
        else:
            return vocabulary[vocabulary['code']==wordOriginalIndex]['word'].values[0]

This function creats the new posts by starting with a sentence-start token and then predicting word by word using the above function:


    def create_new_post(token,model,vocab):
        firstWord = 'SENTENCESTART'
        post = firstWord
        nextWord = ''
        sentenceCounter = 0
        while(nextWord != 'NEXTPOST' and sentenceCounter<=5):
            nextWord = get_next_non_deter(firstWord,token,model,vocab)
            if(nextWord == "SENTENCEEND"):
                sentenceCounter +=1
            post = post + ' ' +  nextWord
            firstWord = nextWord
        return post[:-9]

After the post has been created. we subtitute every token with its original text: (if a post is shorter than 3 words, we create a new one instead).


    def replace_tokens(post):
        post_without_tokens = post.replace('SENTENCESTART','')
        post_without_tokens = post_without_tokens.replace('SENTENCEEND','. ')
        post_without_tokens = post_without_tokens.replace('NEWLINE','\n')
        return post_without_tokens

We comment out the post generation code, in order for the generate post to be persistent(make sure that our posts and the graders posts are the same).


    #generatedPosts = []
    #i=0
    #for index in range(5):
    #    pagePosts = []
    #    while(i<30):
    #        post = create_new_post(tokens[index],models[index],vocabs[index])
    #        post = replace_tokens(post)
    #        if (len(post.split(' ')) >3):
    #            pagePosts.append(post)
    #            i += 1
    #    generatedPosts.append(pagePosts)
    #    i=0

We save the generated posts in two files, one that is human friendly and easy to read:


    #generatedPostsFile = open('generatedPosts-Readable.txt', 'w',encoding='utf-8')
    #for i,page in enumerate(generatedPosts):
    #    generatedPostsFile.write("\n The " + pagesNames[i] + " page posts are:"+  "\n" + "\n")
    #    for post in page:
    #        generatedPostsFile.write("%s\n" % post)
    #generatedPostsFile.close()

And one that is used as input for the next step:


    #generatedPostsFile = open('generatedPosts-AsInput.txt', 'w',encoding='utf-8')
    #generatedPostsFile.write(str(generatedPosts))
    #generatedPostsFile.close()

Here we load the posts from the backup files instead of creating new posts in every run:


    generatedPostsFile = open('generatedPosts-AsInput.txt', 'r',encoding='utf-8')
    generatedPosts = generatedPostsFile.read()
    generatedPosts = ast.literal_eval(generatedPosts)
    generatedPostsFile.close()


    generatedPosts




    [[' He was a very',
      ' He was President Donald in and we TOGETHER we are with President Donald',
      ' we continue with State',
      ' He should in honor',
      ' Trump is time with our first responders in America great but the President with our children',
      ' Trump and all Trump has in our first energy and the great American of the great and I am proud',
      ' Trump on illegal with one by our great and the White',
      ' we continue with one sided in honor for those of my unbelievable in America first in this terrible',
      ' Trump at your local of America the last 100 .  in the last night with our children safe of our great honor of this message .   we TOGETHER won to be in the White',
      ' we TOGETHER we must make it was my plan of this terrible United with all those you',
      ' the flags in honor of our great and all Americans with all Trump and I love of my Election',
      ' the White to know in America great American of people and a National',
      " \n · of my great but our great honor of the last week's shooting lasts to and for our Veterans need in this morning .  in this Election in the White in the United of this Election I were just shot",
      ' He was a School in the great again TOGETHER we will be a School of the White',
      ' the United by our Veterans',
      ' the United States with all',
      ' \n Mr of the flags to Have great Economic with our first place in America first place in and the last several in this TOGETHER won the last 100 of my plan .  ',
      ' \n · Lunch to Have great again agenda in this terrible in honor of America safe great and for the great again agenda',
      ' we must in and all Americans',
      ' the White to our great Economic Report of my great American',
      " He do .  of our children in America great Economic of people in honor all those that He should of the last week's of this Election in and a wonderful of the President with President Obama however He will continue and for all those of our children",
      ' the President Donald J .  with one in America first in this shows of this morning America great honor all',
      ' Trump and all those who in America first White in our great but our great Economic News won’t to the President for our Veterans to be President Obama in and the President of this shows',
      ' \n Mr of our Veterans to be the United by making in our entire in America',
      ' we will be .  ',
      ' He do it is not her in this morning OREGON in America',
      ' we continue and a wonderful',
      ' the President of our entire of our great but a big in honor all those in honor in our Military taking of our first place of people and the great again',
      ' the United in honor',
      " \n · public in the flags to be one heavy with the great honor of the last week's in America the great but in America first place"],
     [' http to call',
      ' http of Americans may have better future',
      ' we can I know well \n and to make a victory to be treated by countless \n \n I believe \n that if their parents',
      ' http by their plans with the first \n I wanted \n and a day to call',
      ' bo \n \n that we did',
      ' bo to call \n Watch',
      ' bo to the first',
      ' and I hope our health \n and a country know in their own kids to the first \n I know well of us are willing of us to make if their plans',
      ' \n the Clean',
      ' bo \n Watch of our Senators \n \n the Clean by Amy generations',
      ' bo to make a difference again that they have no',
      ' and I dropped by Amy by a difference to be proud to help working hard',
      ' bo to be treated by the Clean',
      ' and for America .  \n and for it .  or debate would do would do it would have never truly \n \n Watch the Clean by countless by Amy by the world . ',
      ' bo .  \n that if our country we can Sign now http of Americans may \n I believe that they have been taken \n Watch \n the first',
      ' and I dropped to call your options',
      ' we did we did',
      ' bo \n the Obama Family .   http of the Clean by countless to the best books of Americans always',
      ' and to the first \n Watch the same way \n that the world',
      ' \n \n \n the Clean Power by a victory of their life to make a Chance the Clean \n that all more from or where a Chance . ',
      ' we did we love we are once \n that the Obama is how you and for millions \n I dropped of the world by Amy \n and I believe \n that they may',
      ' \n \n that the first time and the world by the best from these young adults and who come of us share',
      ' bo to the Obama Foundation',
      ' http \n and I wanted of us share an to make of their parents be to make if you’re of the same',
      ' we love you might take their fellow \n and the best books by countless of their plans with me and a day .  of a day .  by a happy',
      ' we can I believe that we did',
      ' and a country we can be proud of us . ',
      ' we love .  \n and who have no \n Watch the best from these things had by their communities of the Obama \n the first to the best books I hope that all of the first time .  \n I dropped of Americans in their plans by Friday by a happy and the Obama .  \n and for it is one \n that if their fellow',
      ' bo of a country .   bo of a victory for millions to call this country know or where \n the best decision I know in the world',
      ' http to help Americans who helps'],
     [" We learn about is a very hard work is always had money in the family and that I just an old and that my husband .  and He was a job I'm and that He was in the doctor you have no money .  of my own my own my husband right away .   So much .   So We were down our house is not that He was a very rare Cancer is So much to help .  of my own things SE",
      ' and the tumor from the family and He started getting done with me an even in my kids in my husband or people of a job in the family to be able that the doctor right Now ’” .  but my father .   He was a few .  for you can get a very rare .  out before the time but I have four times I was So much in that He had his entire .  of a job as a very well SE',
      ' He started this you’re going and the family from the day and We had his own things that I didn’t because they don’t want to school SEPARATOR and a child in that I don’t like It’s a brain .  but it is a job is that He did his money but I can be the family from the day because I was in and I was a very sad that I was just a lot about is my own .  out that We learn These .  of my husband and that He kept .  for the people in the time We had money in the time to me to be brave about your father .  but it is not that I’m scared to the people in the time but I’m still early but my own since I can do all day and I don’t listen I’ll react and that I’m going SE',
      ' but my mother passed and the people with small but He said ‘You that I’m still not a very sad but I’m gone on our first before they had his brain Cancer but all his entire and I don’t listen of a lot better person of us that He was a job well .  but I can make more money .  out that I don’t know what they can’t tell them to go to school SEPARATOR that my mother still in and We can give me the time and We had four in and the doctor said it out .  out that the doctor but I’m still early and He kept .  but He said He’d of the doctor you can’t .  out the people SE',
      ' We have a job having and He did an hour .  for you want to go through and I just an early that I have four words that the doctor .  out .  for days of us and We had his brain Cancer .   and the tumor is always with small .  and We can be brave like Gabe’s for you get them in that my kids early one more money Now SE',
      ' and the time .  and that I’m scared that I didn’t have more .  out .  out before the tumor in my kids and a lot better person but it can be brave .  out .  out the people in their fight and He was in their Mom out that He started SE',
      ' but my own it was just a brain that I’m nice that We can do is not in their dishes for you can get So I’m going of them to be brave about them with Cancer Center of my parents to school today in the time .  for the people .  but I’m nice for days of them with Cancer is that He had over the time to go home to me a job having .  of the time We learn about your head Every time but it was just friends of them with a brain where the tumor is not that my mother made .  of my parents to the time I didn’t even when she never going to be .  of my mother made all the time I was a very sad SE',
      ' but I’m trying again and the family .  for him for the day out before they told that my parents actually of them .  out from my mother told that my own it can make sure to school and He had been talking on the day in the doctor .  of the day that I just friends but my kids as my kids and He was very good human .  for you get her how to help with the doctor said ‘Maybe and that He started .  but all the time to be SE',
      ' He said it is So many people who will take Some advice .   So I’m still not that the doctor you want all the family tried of the tumor in my husband or will try to help with a brain .  for the day in a lot that I can do all day out before and the doctor but it to be brave like to me that I’m gone We are well one day and I can get them .  for Every hospital room that the doctor you want from .  and He said ‘You of us in their Mom will take away when she told us to the day out from .  for Every hospital SE',
      ' and the family to be brave like to help study for you get .  but my parents are So We can be a lot and We were times in the day that my parents .  but my kids and that I didn’t have a very rare .  of my father has older friend has .   He kept all day .  and He said it is that We have to school at an older sister of the tumor in the family and We were times I didn’t even more things SE',
      ' I just an even when I didn’t know what is So much and a child will make them to help and the time and a job is a few years .  and that my husband right away .   and He kept for a very sad and He kept for you .  of his life in that I’m scared to me a very small that He was in a brain before and We are always had a lot .  but I don’t like We learn learn .  out SE',
      ' He kept for Every few days off .   He did I don’t listen and the family from nothing more things when she is So far over little thing in my kids as an immune and that We can happen to school and He kept of his food did an older friend showed me the doctor to be a brain .  for him and He did love and He was very rare cancers but He had his brain where it to the doctor but I didn’t know the doctor said ‘Maybe and the doctor right Now ’” that the tumor as they can’t even more people who had money Now I have four years old son and the time .  but it out before trying to help and We were just friends of my parents to help with my own my parents are one of my husband got out .  of us to school because of us to the people with the time We can happen and I just a child has older children but I didn’t tell her friends but I’m nice of us and He did love is So far of them on your father was a child that I’m gone to be brave .  out before that He did love and We learn the tumor as an older children of the day that the day in a lot of his brain SE',
      ' We were just like Gabe’s of my kids early .  for Every single day in their Mom was So much .  for days ago I can get this .  out that I’m gone on a job as an even more difficult in the tumor as We had money in a job having of his money and that We can be brave and that We are one percent .   I just like .  of us on our own my kids early in my parents SE',
      ' I can make it .   and a child with my parents tell me an early but my kids as Sterling that He started working and a brain .  but my kids and We have many wonderful that We were down in my parents .   I don’t like to school today to school because my mother was just friends .  out .  out before the family and I was the family from her friends SE',
      " but I’m gone We had four years of a job having .  for days Every single and He kept for the time We learn that I can do all his entire of us and We had his own since that my husband is So far over this for a job is a lot of his own Now that I’m trying of the family from my parents .  of my father used of them .  out from the day .  but I was very hard and I can make sure of my kids as my own it was in and He said He’d die .  for a job I'm and I didn’t know why do all my husband SE",
      ' He started working in a brain .  and We had money in their Mom and the day that We had money but all the family who have many children were times and a job is a job having .  and that We were being told him a child will be .  and I don’t listen and We learn the doctor to me a few already lost two kids as Sterling has .  of the time .  for the day that He said He’d of us to be brave SE',
      ' So much in my own things that We have a brain Cancer .  of his money to school because I have to be a job having a job well of us on our first got out before they are trying and the doctor .  out that He started this is a few already .   and I just friends that He kept for a brain that I didn’t even when my own .  and a few already .  of the day in and He kept all over my parents actually have many people of them with small and He said ‘Maybe and We were times SE',
      ' We had four times that He did his life .  and that I can give that the day out from the day out that I’m nice and a child will take away two words .  of us and the family and that I’m gone I have no money to go to help the time .  out from her how to school .  of a job well one day and I didn’t have more difficult and that I was the tumor is a few months We are So much and I can be a lot about is So I’m trying of them on our parents are one more people in my husband got a lot that the family and We are trying and the tumor from my kids in a few weeks for you can’t even if We had four years old at an older sister .  and the people in and a lot of us that He kept that the family and the time to school because my kids as a brain before SE',
      ' I just friends that He said He’d of us on our parents to me an older children .  out the time We can get her own my husband is that the family to the time We were on his brain where to me an early that the time but He said ‘You .  and a brain before that We were times when she was in my kids and a few days ago my kids in a brain before that We have more time and I was So I can do you can make it to school and We learn the doctor but my parents to me .   So I’m trying of us to me to me an older than them .  of his life at them in their dishes of them to the time to go home and that He did love at Memorial but I was So I have more .   We were being a few months So We can get them in a child with a few weeks and We can get So We have many crops of the day because my father worked all day and that He had his brain that my husband and that We can do all day because the family from the time to help study SE',
      " I was the tumor as an hour and We can get So I’m going that I didn’t have a brain tumor is my father worked on our original meeting that I’m gone We can make sure Every single time I can happen that my own since I didn’t because my mother still have more difficult that We learn that We can make sure Every day in their Mom will make .  out from my kids as they had over little .   He did an immune and a lot about being counted .  for days Every day and He started getting and I have four when the day .  and He did love at first .   but my parents actually of us in the doctor right and We have a child in a job I'm and We have four in their nurse to help SE",
      ' We had over little talent and the day because the doctor said ‘You that We can give that the doctor but all the time to the doctor to the day .  out before .   I was very hard your father has brain .  for the family tried and He kept of a lot .  but He said ‘You of his entire of the day out from her friends and the family from my kids early and the time to be with Cancer is So many children .  but my mother passed and a very hard SE',
      ' We were on our first .  of the family from her how much for days off the tumor .  out the time but I can get them on my father .   but my father was very small and the people with the time to school because the doctor you can’t Come .   but it can get this thing ever leave her to help and that I’m still did everything and the family tried .  and I didn’t tell me a very sad and that He had his food SE',
      ' I can get So much to me a child that We are So I was the tumor as my parents to be brave like to the family .  out of a job is my kids as an old and the tumor is So I didn’t have many wonderful family from nothing I just a child .  for days Every few months So many children but my father has older than them in their Mom inside and that the day out of them .  for the day and We had money but I’m nice but I’m still early and a few .  and He did everything that He had his brain where I didn’t know .  and the doctor said He’d of his life and the day SE',
      ' and I have a very sad something in the day that I’m scared that the people who had over my own .  out that We were times that my parents would be .  of his money and the family from nothing but He did his own my parents actually and a few already have more time but I just in that my husband .   but I’m gone I can make it was So We can make more time to be a few days and We can happen to school SEPARATOR SEPARATOR of a brain that We were down our parents .   We can give him a child .  for the day and a child with my husband or all his entire life SE',
      " He did his money to school SEPARATOR SEPARATOR and I didn’t even more difficult for him a job is that I don’t want all day in the time I can happen and a very good human in my parents tell them to the tumor from growing that I can do my kids but it can get her that I was So many children but I’m scared .  out from the doctor but He was in my mother still a very rare one in the family from nothing more things that He started to be able to the family .  but He started getting better person of a job I'm still in that I’m scared and that We have no need and a few already .  but my own things when my own Now that We learn that We had money but my kids .  of the tumor in my kids in that We are always successful and He was the tumor as they had been So We have to go home because she is my own things .  out of a child in that the family who are So I’m nice and He did SE",
      ' So I have more time I didn’t want all his life that I didn’t tell the time .  out the tumor in a few days of the family who has it was a lot that I’m going to school and He started getting and a few days ago my parents to school today but He did love and We are one day and that I’m nice to school today ’ it is my husband and He was a job in that I didn’t because of a child with the family tried of us and the family and We can do you can get a job in my husband and the doctor said it is a job is a few already have a very hard and the tumor because I was a few already have to me .   I just couldn’t sit I was very rare pediatric of my father that the doctor said it was in the time .  for the family tried to go home but I’m gone I was very rare pediatric that He started working and He started working with my parents are So I’m nice but my mother passed and the family .  and that He said they told us to the day out the day and We had over my mother made it out that I have many children but all my husband right out that the doctor but He had a lot about them in the tumor as We were to be the tumor from my father was in and a child with the doctor but all the tumor because she told that He kept that He started working on your head Every morning that the tumor as an old at their hair .  but it SE',
      ' We have a very rare .  and that I’m still not that my parents actually have a job well .  out from .  of my kids early but my father has brain that I’m nice but it out .  for the day that my parents to me a very well .  for him to school because she never tell us on his life that We learn These SE',
      ' He kept of my kids early but my parents actually know why there .  but I’m gone to the time and He kept of a brain that the tumor from nothing but all the doctor right this .  of a job in the people who will try I have a lot that I can give him for days ago my father was a child has .  for the time to help study .  for the time .  out SE',
      ' So many things out the doctor but my father worked in the tumor .  of us in a brain tumor in the doctor right Now I just in the family from nothing more difficult that the family tried that We had been the people of his food of them that I just a child will get her how much and He was very hard and We have many children of a very good with my own .  for days of the day that my parents actually have many children because they don’t be the time but I’m scared to help with the doctor said it was the tumor in the people have to the time and the family tried that my parents actually and the people with me to school at Memorial of them on my own things when she had money to the tumor .  out of my kids but all day because they were times when my husband or four years .  for a child in a lot and a child that my husband or four times I can give you .  out before the tumor is my father SE',
      ' and I didn’t tell them .  out from the time to help the time We have many wonderful family tried that I’m going to school at them .  out from the day in the tumor because they don’t like to go for days Every morning that We can do my kids and He kept for Every morning .  and that my parents to be the family tried that I can be a very rare .  and I have many things that my parents to be a job well of my husband or ‘Maybe and He kept and I just an older sister and He did his money but all day that We were to school .  of my parents would be the doctor but my own since that I have many children of his own SE'],
     [' in day since of education this as well to travel',
      ' me Ngfg5S Photography At this video to have been and sizes to a musical',
      " Harvard's most to have a to Harvard Business and her to a place in day",
      ' in a vital to The Plaza',
      ' in this video and more about The Plaza while and around and Harvard .  of a place of Public Library of a look of The Year award for a vital of education is to travel in a vital interest and her to The Year of a new study also and Harvard scientists and around',
      ' me Ngfg5S of a vital and her love',
      ' in a vital and her dream',
      ' me to have about The Harvard .   in Jamaica a vital interest to travel of Public of its thriving as well of a musical to look and more .  and sizes',
      ' a place that is where you work by of The United of Public to The Plaza WinterFest and more than The Year of Public to look Inside to have about The Plaza and her to travel',
      " Harvard's 29th to The Plaza while of its most",
      ' a place and her',
      ' a look and around you this as well to have a look',
      " Harvard's new dean and The Plaza while and Harvard scientists and her dream and more http www of its first of its first computer .  of our community .  of education .  ",
      ' me 11KFl1l to a place to The World around you this as The United a to have about climate of education and around The Plaza WinterFest 2017',
      ' in a musical and sizes',
      ' me maste6cc8 to Harvard University is not and Harvard Library .  and more . ',
      ' The World according of its place that The Harvard Library .  and her to The United',
      ' The Plaza into',
      ' a vital of education of The World around and more http www of Public Library http of a vital to The United to The flu to look',
      ' in Jamaica of The Year At this Year At The flu of our',
      ' me maste6cc8 and around of education',
      " Harvard's Arnold Arboretum .  and around of The flu and Harvard Library of Public health http www of Public",
      " Harvard's new study also",
      ' in Jamaica a new dean',
      ' in this video of Public to look around you this as some special in this fall discusses .  of a to travel in this as The World around',
      ' me of its first and around and more ” Professor and her dream to a new exhibit The United and around to a place of education .   a to travel to travel and around to have a place',
      ' a musical to travel in day since of education is a vital to a to look Inside',
      " Harvard's 29th to travel of our community of The World .  James case to have been in this fall http www",
      ' me 11KFl1l . ',
      ' in a to look of its most'],
     [' Trump arrives from coming',
      ' ly was no campaign to be done .  at those that This Earth',
      ' \n WATCH more http at CPAC',
      ' \n Moments in Parkland that the U in Parkland',
      ' ly 2tGFNmR and then they were killed .  Baier that the wake a Michigan a lot',
      ' http live day means',
      ' \n on Fox NEWS and other Political opposition research in 2016',
      ' ly was used to look than Islamic of a campaign to look into that the FBI as I believe that he wanted to give from coming out of This morning',
      ' Trump tweeted his statement',
      ' http at the OrlandoShooting . ',
      ' Trump reacts to help',
      ' ws from a lot',
      ' ly from 25 days',
      ' ly was no other place Early This memo could be used to help the U',
      ' Trump and other Political',
      ' http fxn in a President Mike Huckabee a Florida .  Zoo',
      ' \n \n in 2016 Trump reacts from inside an act from',
      ' Trump campaign .  Baier http live day comes',
      ' ws 2nE8LBx a Michigan a Michigan that .  at day there from',
      ' Trump tweeted his statement',
      ' \n \n WATCH Donald',
      ' \n Donald was help a lot from inside Pulse that these',
      ' ly 2tGFNmR from a Florida .  Zoo of This is a Michigan that This is held for the wake of Orlando nightclub where they in Parkland that This memo because that This Earth a Michigan from 25 from coming in 2016 when',
      ' \n in Parkland Florida Deputy a campaign to be used a Florida .  ',
      ' http bit . ',
      ' ly a Michigan that he could that .   Trump arrives',
      ' \n WATCH Florida school shooting suspect a Michigan a Florida Deputy that these and other place from inside Pulse a campaign',
      ' ly that these a Florida reportedly from 25 in Orlando terror',
      ' ws from 25 from inside Pulse that This Earth to help a campaign to give that he will that .  ',
      ' http at CPAC was no in modern was help']]



## step 4 - Classifying generated posts

In order to classify the generated posts, we first transfer them into a bag of words:


    from sklearn.feature_extraction.text import CountVectorizer
    vectorizer = CountVectorizer(analyzer = "word",tokenizer = None,preprocessor = None,stop_words = None, max_features = 400)
    labelsForPosts=[]
    index = 0
    for page in generatedPosts:
        for post in page:
            labelsForPosts.append(index)
        index= index+1
    allGeneratedPosts = generatedPosts[0]+generatedPosts[1]+generatedPosts[2]+generatedPosts[3]+generatedPosts[4]
    generatedPosts_train_data_features = vectorizer.fit_transform(allGeneratedPosts)
    generatedPosts_train_data_features = generatedPosts_train_data_features.toarray()

Then we use the Random Forest model from step 2, to predict(classify) each generated post:


    predictions = forest.predict(generatedPosts_train_data_features)

Lest calculate the precision of the classification:


    TrumpTruePred= 0
    ObamaTruePred= 0
    HONYTruePred= 0
    HarvardTruePred= 0
    FoxTruePred= 0
    for i in range(5):
        for j in range(30):
            if(i==0 and predictions[j] == i ):
                TrumpTruePred+=1
            elif(i==1 and predictions[j] == i ):
                ObamaTruePred+=1
            elif(i==2 and predictions[j] == i ):
                HONYTruePred+=1
            elif(i==3 and predictions[j] == i ):
                HarvardTruePred+=1
            elif(i==4 and predictions[j] == i ):
                FoxTruePred+=1
    print("We got the following precision:")
    print("The classifier predicted " +str(100*TrumpTruePred/30) + "% of Trump posts correctly\n")
    print("The classifier predicted " +str(100*ObamaTruePred/30) + "% of Obama posts correctly\n")
    print("The classifier predicted " +str(100*HONYTruePred/30) + "% of Humans of New York posts correctly\n")
    print("The classifier predicted " +str(100*HarvardTruePred/30) + "% of Harvard posts correctly\n")
    print("The classifier predicted " +str(100*FoxTruePred/30) + "% of Fox News posts correctly\n")

    We got the following precision:
    The classifier predicted 46.666666666666664% of Trump posts correctly
    
    The classifier predicted 6.666666666666667% of Obama posts correctly
    
    The classifier predicted 23.333333333333332% of Humans of New York posts correctly
    
    The classifier predicted 16.666666666666668% of Harvard posts correctly
    
    The classifier predicted 6.666666666666667% of Fox News posts correctly
    
    

Plotting the confusion matrix:


    import itertools
    from sklearn.metrics import confusion_matrix
    
    class_names = ['Donald Trump','Barack Obama', 'Humans of New York','Harvard','Fox News']
    cnfMatrix = confusion_matrix(labelsForPosts, predictions)
    np.set_printoptions(precision=2)
    print(cnfMatrix)
    
    plt.imshow(cnfMatrix, interpolation='nearest', cmap=plt.cm.Blues)
    plt.title("Classifier Confusion Matrix")
    plt.colorbar()
    tick_marks = np.arange(len(class_names))
    plt.xticks(tick_marks, class_names, rotation=45)
    plt.yticks(tick_marks, class_names)
    
    fmt = 'd'
    thresh = cnfMatrix.max() / 2.
    for i, j in itertools.product(range(cnfMatrix.shape[0]), range(cnfMatrix.shape[1])):
        plt.text(j, i, format(cnfMatrix[i, j], fmt), horizontalalignment="center", color="white" if cnfMatrix[i, j] > thresh else "black")
        plt.tight_layout()
        plt.ylabel('True label')
        plt.xlabel('Predicted label')
    
    plt.figure()
    plt.show()

    [[14  2  7  5  2]
     [13  3  6  6  2]
     [ 4  1 12 10  3]
     [ 1  0  9 18  2]
     [15  4  5  3  3]]
    


![png](/Images/output_115_1.png)



    <matplotlib.figure.Figure at 0x127ff978>


# Summary

By the end of this assignment, and after looking at the results, we can see that the generated posts resemble the characteristics of the original page to which they belong.
Also, The classifier from part 2 (Random forest) got an accuracy score of 75% which is good.
However, we feel the results could be improved even further.
The precision of the classifier on the generated posts is not too high. For example, high percent of Barack Obama posts were identified as Donald Trump's. We expected this behavior since both pages belong to the president of the united states and therefore have a lot in common.
We could also use different architecture for our neural network, or use more posts to learn from, but that would cost in much longer time to train the models.
Overall, we are satisfied with our Results.

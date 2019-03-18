---
layout: post
title:      "Understanding CLI's"
date:       2019-03-18 05:09:41 +0000
permalink:  understanding_clis
---


The undertaking of a CLI is something that I thought would be nearly impossible. Even though I've been learning all of this amazing information from Flatiron, the power of the blank screen definitely intimidated me. This is where I had to realize that the code *DOESN'T* have to be perfect the first time around. It may seem like you're stepping into the abyss but once the wheels start turning in your brain then no one can stop you from building something amazing. So, how did I implement this golden rule? I did it by following the steps below.

1. Understand Bundler and how to set up an environment for your Ruby gem.
2. Write everything you want the program to do in order of how you would want your user to interact with it.
3. Separate your needs into methods and classes to assign functionality and establish DRY.
4. Begin scraping your chosen websites while debugging with PRY. (It's your best friend, just trust me)
5. From there assign your scraped elements to instances of your created classes and viola, you now have information.
6. Once you've created your program, pat yourself on the back because you're about to break it.
7. REFACTOR

I could go into detail about each step listed above but the two I want to focus on the most are scraping and breaking your code. The thought process I held was pretty simple, they want information, I'll give them information. So I went to fandom where it held an information abyss. First thing wrong was that I had chosen a site without a consistent structure, every webpage was completely different. Some webpages would have info I needed and others would just skip right over it. The HTML was inconsistent and unordered, so there was no easy way I could iterate through my doc variable and assign instance variables. 

In order to get all the info needed, I had to scrape two websites, resulting in repeating methods. In other words the code was not DRY, I was very wet at this point. Although, it was time to make due with what I had. In order to achieve the dryest code possible, I tried to make my scraper methods as minimal as possible. Below is a sample of my scraper file, by using a seperate site I was able to iterate with my *each* and assign a name to each of the articles I was scraping. Main lesson, find a simple and well structured site to scrape from, it will make your life and code much easier.


```
class ScraperKoah

    CAST = 'https://www.cbs.com/shows/survivor/cast/season/32/' #koahrong
    SEASON = 'https://survivor.fandom.com/wiki/Survivor:_Ka%C3%B4h_R%C5%8Dng' #koahrong
    SITE = 'https://www.cbs.com'

    def self.scrape_castaways_k
        file = open(CAST)
        doc = Nokogiri::HTML(file)
        #binding.pry
        Castaway.clear
        castaways = doc.css("article")
        castaways.drop(1).each do |castaway|
            name = castaway.css(".title").text
            profile_url = castaway.css("a").first["href"]
            #binding.pry
            Castaway.new(name).tap {|cast| cast.profile_url = profile_url}
        end
    end
end
```

Finally, refactoring! One of the most important steps that I didn't believe in until after I looked at my code...and didn't understand it. Try writing a program hazpherdely, take a day off and come back to it. Your eyes will hurt and you'll question your ability to write anything. Understand that having multiple methods that have clear, concise, minimalistic code results in a better understanding of what your program is doing. No one wants to open a file and the first method is 200 lines long. The best lesson I learned, was that giving a certain method it's unique functionality allowed me to easily solve bugs, understand what was happening and when. In other words keep it simple, we're coding not stuffing Thanksgiving dinner.

---
layout: post
title:      "'Flatiron Project #1: Creating a CLI Data Gem'"
date:       2020-04-04 18:09:33 -0400
permalink:  flatiron_project_1_creating_a_cli_data_gem
---


Today i submitted a project that required me to build a Ruby gem that provides a CLI (Command Line Interface) to an external data source. I decided to utilize an API (Application Programming Interface) from a public website instead of scraping data  so that i could use more of my time incorporating the Object Oriented Ruby concepts I have been learning during these first 7 weeks at Flatiron School. To be honest, 7 weeks ago i wasnt even familiar with words like 'API', 'CLI', or 'OO Ruby', so even though this project was challenging and required me to exhaust all resources to complete, I can only feel pride in what I have learned thus far.

In order to get my project functioning i started off by building 3 classes: an API class to get the data from the website i chose: 

```
class API
  
  def get_info
    response = HTTParty.get("https://rickandmortyapi.com/api/character/")
    characters = JSON.parse(response.body)["results"]
    characters.each do |character|
      Characters.new(character)
    
    end 
  end 
end 
```

 
(that i had to parse to return usable data), a CLI class to provide users access to that data, and an Object class to save the objects in and build methods that I could incorporate into my CLI class. Because the concepts of this project were fairly new to me, I ended up utilizing a lot of the helpful resources provided to all students about Ruby gems, how to look for good API's, and what a CLI is/what it does. These resources were extremely helpful when setting up my API class especially. 

I was able to use the weeks of repititive practice and OO Ruby knowledge to build my CLI class and make it as "dry" as possible. I reviewed many previous lessons like mass assignment:[](http://)I was able to use the weeks of repititive practice and OO Ruby knowledge to build my CLI class and make it as "dry" as possible. I reviewed many previous lessons like mass assignment:[](http://)

and iteration to help me apply methods to all objects instead of having a ton of repetitive code. Completing this project not only allowed me to see how all of the material I have been learning over the weeks comes together to interact and build programs but it also pushed me outside of my comfort zone and required me to build relationships with the other students in my cohort. It was very relieving to know that I wasnt alone when I was struggling and it was great being able to ask for assistance from those who could help me understand things in a way that the online lessons couldnt fully do. I also had a boost in confidence everytime i was able to help others understand things as well. I learned so much by applying my knowledge to build a Ruby gem and i am extremely excited to have my knowledge continuously expanded over the course of this program. 

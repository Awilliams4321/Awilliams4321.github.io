---
layout: post
title:      "'Flatiron Project #1: Creating a CLI Data Gem'"
date:       2020-04-04 18:09:33 -0400
permalink:  flatiron_project_1_creating_a_cli_data_gem
---

test1
Today i submitted a project that required me to build a Ruby gem that provides a CLI (Command Line Interface) to an external data source. I decided to utilize an API (Application Programming Interface) from a public website instead of scraping data  so that i could use more of my time incorporating the Object Oriented Ruby concepts I have been learning during these first 7 weeks at Flatiron School. To be honest, 7 weeks ago i wasnt even familiar with words like 'API', 'CLI', or 'OO Ruby', so even though this project was challenging and required me to exhaust all resources to complete, I can only feel pride in what I have learned thus far.

In order to get my project functioning i started off by building 3 classes:

1) An **API class** that uses the HTTParty gem to '.get' data from the website i chose, parse the response body to return the values in the "results" string, and then iterate over each JSON object(set to the variable character) to instantiate a new character object and return each character object's hash which consists of their attributes(the key) and the information assigned to those attributes(the value):
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

2) An **Object class** that saves each new character object and assigns it a list of attributes and values upon instantiation. I then defined my initialize method to accept an unspecified hash and mass assign each defined attribute to it's value. Because I only wanted to utilize specific keys and values from the hash, I used the '.respond_to?' method to grab only the information from the attr_accessors I had defined. After creating a save method to shovel each new object instance into the @@all array, i created a 'find_by_name' method that searches through each object in the @@all array and finds the character.name that matches the name passed in:
```
class Characters

  @@all = []
  
  attr_accessor :name, :status, :species, :type, :gender, :origin
  
  def initialize(hash)
    hash.each do |k, v|
      self.send(("#{k}="), v) if self.respond_to?(("#{k}=")) 
    end 
    origin == origin["name"] #Figure out how to pull out JUST the name
    save
  end 
  
  def save 
    @@all << self
  end 
  
  def self.all
    @@all
  end 
  
  def self.find_by_name(name)
    @@all.find{|character| character.name.split.map(&:capitalize).join(' ') == name}
  end 
  
end
```

3) A **CLI class** to allow users to interact with my data: 

Here i created a start_up the method that i could execute in my run file using the code 'CLI.new.start_up':
```
class CLI 
  
  def start_up
    welcome
    get_char_list
    options
  end 
  
  def welcome
    puts "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~"
    puts "Welcome fan, I'm PICKLE RIIIICK!"
    puts "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~"
    sleep(3)
    puts "> To see the list of characters on the show, enter 'characters'."
    sleep(2)
    puts "> To exit, enter 'exit'."
    sleep(1)
  end 
  
  def goodbye
    puts ""
    puts "~~ See you later! And next time... ~~"
    sleep(2)
    puts ""
    puts "  ------------------------------"
    puts "  STAY OUT OF MY PERSONAL SPACE!"
    puts "  ------------------------------"
  end
	
  def options
    
    usr_entry = gets.strip.downcase
    
    if usr_entry == 'characters'
      puts char_list
      options 
    elsif usr_entry == 'exit'
      puts goodbye
    else 
      puts invalid_entry
      options 
    end
  end 
	
	def invalid_entry
    puts "Invalid entry. Please re-enter request."
  end 
	```
	
	
	This is the method i created to get access to my website's data defined in my API class:
	```
	def get_char_list 
    API.new.get_info
  end 
	```
	
	
 In my character list method, I numbered every character in the array and listed them by their name attribute:
 ```
  def char_list 
    
    Characters.all.each.with_index do |character, index|
      puts "#{index + 1}. #{character.name.split.map(&:capitalize).join(' ')}"
    end 
    
    puts ""
    puts ""
    choose_character
    puts "*******************************************************************************"
    sleep(2)
    puts ""
    puts "> To return to the list of characters, enter 'characters'."
    sleep(2)
    puts "> If you would like to exit, enter 'exit'."
  end 
	```
	
I then created a method that allows the user to enter the character name they would like more info about using the #find_by_name method created in the Character object class. After the name has been entered they receive a list of details or 'attributes' (also defined in the Character object class):
```
  def choose_character
    puts "Enter the name of the character you would like more info about:"
    puts ""
    
    usr_entry = gets.strip.split.map(&:capitalize).join(' ')
    
    char_choice(usr_entry)
    
  end 
  
  def char_choice(character)
    char_object = Characters.find_by_name(character)
    if char_object
      puts ""
      puts "******************************************************************************"
      puts " ~ Name: #{char_object.name}"
      puts " ~ Gender: #{char_object.gender}"
      puts " ~ Species: #{char_object.species}"
      puts " ~ Status: #{char_object.status}"
      puts " ~ Origin: #{char_object.origin}"
      if char_object.type.strip.empty?
        puts " ~ Type: N/A"
      else
         puts " ~ Type: #{char_object.type}"
       end
    else
      invalid_entry
      puts ""
      choose_character
    end 
  end
	```

Because the concepts of this project were fairly new to me, I ended up utilizing a lot of the resources available to all students such as documents on how to identify scrapeable sites, Bundler Gem docs, as well as videos on how to set up your environment file to require external gems using the keyword 'require' and create a path to access the files in your application using 'require_relative'. These resources were extremely helpful when setting up my API class especially.

I was able to use the weeks of repititive practice and OO Ruby knowledge to build my CLI class and make it as “dry” as possible. I reviewed many previous lessons like mass assignment and iteration to help me apply methods to all objects instead of having a ton of repetitive code. Completing this project not only allowed me to see how all of the material I have been learning over the weeks comes together to interact and build programs but it also pushed me outside of my comfort zone and required me to build relationships with the other students in my cohort. It was very relieving to know that I wasnt alone when I was struggling and it was great being able to ask for assistance from those who could help me understand things in a way that the online lessons couldnt fully do. I also had a boost in confidence everytime i was able to help others understand things as well. I learned so much by applying my knowledge to build a Ruby gem and i am extremely excited to have my knowledge continuously expanded over the course of this program.

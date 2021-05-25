---
layout: post
title:      "Exploring a Basic User-Interface"
date:       2019-06-21 11:57:14 -0400
permalink:  exploring_a_basic_user-inteface_and_stocks
---

Even with just the basics in mind, you can create a user-inteface!

I just recently finished my first project for flatiron school where we were supposed to scrape in formation from a website of our choosing, create a Command Line Inteface that calls upon our scraper class and retrieves the information that a user is asking for. 

For my project I chose to retrieve stock information.  Eventually I developed a program that gives the user to 1) operate from the public options (which simply allows them to view the scraped content of that web page regarding stocks), or 2) log in and be able to buy, sell and view their personal stocks.

**Steps for a Basic User Interface:**

The different components necessary to complete the user interface were 
* Creating a User.
* Relating and requiring a user for a user function.
* When a method is accessed by both non users and users, there needs to be a way to differentiate the path it will send them on.
* There needs to be a way to store user information.
* There needs to be a way to log out and log back in.

**Creating a User**

For the gem I created, when the user first runs the gem they have the option to look at general information or look to buy/sell/view personal stocks.  If they select the latter they are greeted with a message that says "You will need to be logged in to access the requested information".  Afterwards the log in menu method is called.

The log in menu method looks like this:

``` 
def self.log_in_or_create_menu
    puts "If your have an existing acount type 1 to log in. Otherwise type 2 to create a new account or type 0 to return to the previous menu."
    input = gets.strip
    
    case input
    when "1"
      log_in
    when "2"
      create_new_patron
    when "0"
      start
    else
      puts "Invalid input."
      log_in_or_create_menu
    end
  end
	```
	
	This code tells the  user/new user what input is expected, gets the input with **gets.strip**, sends the user to the log in menu when they type 1, sends a new user to the create user menu when they type 2, gives them the option to go to the previous menu by accessing the start method, and if the input does not include what is expected, it repeats the menu options by calling itself.
	
	When a new user types 2 to create a new user name, the following code is called:
	
	```
	 def self.create_new_patron
    puts "Please enter your desired username."
    
    username = gets.strip
    if username_search(username) == nil
      puts "Please create and enter a password for your account."
      password = gets.strip
      Patron.new(username, password)
      puts "Welcome #{username} to your new account!"
      personal_stock_menu(username)
    else 
      puts "Error. This user already exists."
      username_already_exists
    end
  end
	```
	This method tells the user what it expects with **puts**, requires user input with **gets.strip**, for which the input is an intended username, searches the currently stored instances with the **username_search(username)** method to see if that username already exists within it's memory, and if it doesn't exist, it asks for the new user to input a password and receives it with **gets.strip**, assigning this new input to the password variable to differntiate.  It then activates the initialize method with **Patron.new**, taking an argument of the username and password and puts welcome.  If the username entered initially matches on in the database, the user is notified with **puts** and is sent to the **username_already_exists** error menu.
	
Note: there are no restrictions on what can be entered for a username or password for this method.  To do so would require additional code.

The **username_search(username)** method looks like this:
def self.username_search(username)
```
	 Patron.all.find{|element| element.username != nil && element.username == username}
  end
```
where the Patron.all method was this:
```
def self.all
    @@all
end
	```

and the .find method was this:

```

```

The **username_already_exists** error method looks like this:
```
def self.username_already_exists
    puts "The username you enetered already exists."
    puts "Type 1 to enter a new username or type 0 to return to the start menu."
    
    input = gets.strip
    
    case input
    when "1"
      create_new_patron
    when "0"
      start
    end
		```
  end

The **Initiallize** method is within the Patron class and looks like this:

```
def initialize(username, password)
    @username = username
    @password = password
    @@all << self
  end
	```
	
	**Relating and requiring a user**
	
	
	**Differentiating Paths for Shared Methods**
	
	
	**Storing User Information**
	
	
	**Log Out/Log In**




My journey
-scraper
-user interface:  I  learned that you have to make multipe routes.  Routes for when username isn't required and routes for when it it s.  When a method takes both users and non users, you have to incorporate an optional argument such as "username = nil"  and in order to show where this method will direct you, you have to test if username = nil in your method so that your program won't break.

-stocks information



Conclusion

at the end of the day what I learned

-it was a great learning experience and I'm excited to learn more!

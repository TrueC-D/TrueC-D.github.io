---
layout: post
title:      "Event Space - More complex than you'd think"
date:       2019-11-18 17:35:41 -0500
permalink:  event_space_-_more_complex_than_youd_think
---


For my Sinatra Project I decided to create  an application that went above and beyond the requirements.

I created and app that allows you to create and attend events which required a more complex version of object relationships than had already been taught.  The original requriements were to have a belongs_to and has_many relationship, but I had both a belongs_to, has_many, and  has_many_through (has_many to many) relationships. My dilemna was how to differentiate between User.events (events a user has created) and User.events (events that a user is attending). With active record both the has_many through and has_many construct require the same variable name and one overwrites the other if they both exist so both couldn't be accessed at the same time.

My solution was to still use activerecord partially but to create my own instance method for the user has many through method.  I created the "events_attending" has many through method by calling on the "EventAttendee" join table.

    ```
		def events_attending
		
		  EventAttendee.all.select{|item| item.user == self}.collect{|item| item.event}
		end```
		
Another dilemna was how to create  a way for a user to choose to attend and choose to revoke their attendance for an event .  The attendance was easy, as I just had to create a new instance in the EventAttendee table that had both the user and the event ids, but revoking attendance proving to be more difficult as I couldn't just write current_user.events.find_by(event_id: params[:id]).delete as that would call upon the events my user created not the events my user was attending, also my events_attending method was only an array of collected instances and would not delete an instance from a table.  Additionally when I had input current_user.event_attendees.find_by(event_id: params[:id]).delete, it seemed to have an error.  I found a solution similar to this though, by adding an extra line the error was removed.

``` event = current_user.event_attendees.find_by(event_id: params[:id])
        current_user.event_attendees.delete(event) if event```

After these two problems were solved, it was smooth sailing from there. 

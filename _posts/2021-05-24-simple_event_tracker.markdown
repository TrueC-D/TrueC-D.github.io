---
layout: post
title:      "Simple Event Tracker"
date:       2021-05-25 01:42:19 +0000
permalink:  simple_event_tracker
---


Creating this app was fairly straitforward, I got caught on a few areas though.  I had a typo in the .env file which prevented the omniauth log in from working which was eventually solved by recreating the .env file and re-pasting the key and secret. 

Towards the end, I hit a roadblock while trying to modify my event model to include a class scope.  I had the scope defined in my model, which the controller was able to read fine  but when I was in my index view things became more complex.

There were two sections in the events index view.  One section was for past events and the other was for future events. I had two scopes for each of these.  To simplify things I wrote the method for rendering the events in the scope in the events helper method.  The problem was that I didn't know how to replicate <%= %> in the helper method.  

After a bit of research i was able to create this helper: 
``` 
  def link_to_events(events)
        unless events.nil? 
          events.each do |e|
              concat content_tag(:li, link_to("#{e.name} - #{e.category.name} - #{e.datetime}", event_path(e)))
                #  concat link_to( "#{e.name} - #{e.category.name} - #{e.datetime}", event_path(e))
                # end
              concat content_tag(:br)
            end
        end
    end
```

Concat has the same functionality as <%=, and content_tag(:li) and content_tag(:br) allows me to create <li> and <br> tags outside of the view it is intended for.


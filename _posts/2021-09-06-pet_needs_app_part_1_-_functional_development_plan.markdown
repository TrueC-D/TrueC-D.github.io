---
layout: post
title:      "Pet Needs App (part 1) - Functional Development Plan "
date:       2021-09-06 21:57:17 -0400
permalink:  pet_needs_app_part_1_-_functional_development_plan
---


Current app status:  Problematic.  It's supper buggy in that multiple instances of the same thing are created following a fetch requests.  This is likely because too many fetch requests are being made at a time and there are async fetch requests too.

To solve this I want to create "need selections" in the back end.

How i plan to tackle this:

My focus is first going to be to figure out the logic in the controller. 
I will have to rework my form fields and fetch requests if the logic is being moved from front to back.  I will need to recreate validations in the controller and modify the logic surrounding the params.

Minimum that needs to be changed:

Create "needs" and have them function properly with "need selections".  The whole site should work smoothly without gitches and each instance of a class should only be created once per fetch request.


Stretch Goals: 

- Add a proper user log in.  
- Have "Households" where people share access & permissions for pets
- Have "guest access" for a house hold with their own sets of permissions.




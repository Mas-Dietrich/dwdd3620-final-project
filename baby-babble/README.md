# Final Project | Baby Babble
## Mason Dietrich | Spring 2024
### DGM 3620

## What is the app idea?
- My app idea can be best described as "Baby Name Tinder". I want to make an app that takes baby names from an API and displays them for users to either like or dislike and save the names they like

## What will it do?
- As mentioned before, it will take and display names from an API, it will show the name, whether it's a boy, girl, or neutral name, and give the user the ability to either dislike the name, or like and save the name to a liked names array. And, like tinder, after a name has been liked or disliked, a new one will come up for the user to work with.

## Why is it something you want to develop?
- This app feels like it is, for me, almost a keepsake reminder of the semester for myself personally. This semester has been pretty crazy as I have not only been wrapping up school, but been perparing more and more for the birth and arrival of my first child. So making this app just feels, in a way, like a personal reminder to me of the time and to me, it feels like a really fun app to build and a great reminder!

## List at least 15 application requirements for the front end (These should require you to write JS Code, CSS/HTML only won't work)
1. The app will display baby names from an API (API provided by API-Ninjas)
2. The user can filter whether they want boy names, girl names, or neutral names
3. The user can filter whether they want popular baby names or not
4. The app will display one card a time with a baby name based on the user settings
5. The card will display whether it is a boy name, girl name, or neutral name. The color of the card will be based on whether it is a boy name (blue), girl name (pink), or neutral (default card color)
6. Utilizing a celebrity name API, "Celebrity API" by API-Ninjas, users will be able to click a button that says "Celebrity's named {baby name} and it will provide a list of celebrities with that name for users to observe and compare.
7. Utilizing the dictionary API from API Ninjas, each card will have that baby name's unique meaning described on the card beneath the name of the baby
8. The user can like or dislike a baby name
9. When a user likes or dislikes a name, it will remove the name and display a new name for the user to interact with
10. If the user likes a baby name, the name will be saved to a liked baby names array. The user's baby names will be laid out in a list style with the name, baby name gender, and additional options.
11. The user can toggle display of their liked baby names
12. When a user edits a name, it will change the UI for that name editing field so the user can only either save or cancel editing that name
13. The user can organize their liked names ascending / descending alphabetical order
14. The user can edit the name of a liked name in their list
15. The user can delete a name off their list
16. To better envelop the user in the experience of the baby names, they will be able to dynamically display their last name appended to the name of the babies as they come across using the $ method.

## What tool or framework will you use and why?
- I will be using the Svelte framework and supplement the UI with Skeleton UI. I have chosen Svelte because in my opinion, Svelte has all the capability in ease of use of components and dynamic reactivity that I would want in a framework like Vue or React, all in one more simple place. I want to use Skeleton UI because of the uninque UI tools they have to supplement and enhance my applicaiton!
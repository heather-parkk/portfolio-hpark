---
title: Assignment 6
layout: doc
---

# Assignment 6

## Prepopulated Data
- Added up to 15 different countries as options for users to include (did not want to add too many in fear that it would overcrowd the dropdown option for countries); countries would also subsequently be available on the locating option (if I was able to get it to work)
- Added various different users with realistic usernames for the tester to interact with

## Task List

| Task Title                    | Instruction                                                                                   | Rationale                                                                                                     |
|-------------------------------|-----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| **1. Register an Account**    | "Please create a new account." | This task aims to see how easily a user can create their own profile on the account. |
| **2. Create an Travel Profile** | "Complete your travel profile." | This assesses if users can easily set up their travel profile, as well as see if their thoughts on the questions asked are relevant. |
| **3. Start a Chat with Someone** | "Find a traveler you like and start a chat with them."                          | This task tests how easily someone can "like" someone's profile, and then be able to go to the chat feature to find them on the chat section and start a chat with them. |
| **4. Propose a Safe Meeting**    | "Propose a safe meeting with your new traveler friend."    | This feature ensures that it is easy to propose a safe meeting, and see if they have general thoughts on the idea of safe meeting, especially with the location and emergency contact feature. |
| **5. Find a Friend on the Map**   | "Locate a user you’ve connected with on the map to see where they are currently located."     | This  tests if users can navigate the map functionality to locate friends, and also discover the additional info on the map. |

## Task Reports
- I conducted two user testings, one on my friend Jennifer, who is a student at Harvard University. The second user testing was on Lydia, who was one of the intial interviees I had in A1. Jennifer's interview was in-person while Lydia's was on Zoom (she did not want to be recorded). Since I did not have completed "Chat" and "Locating" functions, I used the tactic demonstrated within Lecture where a person "drew" the wireframes to demonstrate to someone how to website would work (as was suggested by a TA when I asked how to do A6 with incomplete sections of my A5). For Lydia, since it was an online call, I utilized the old wireframes from a previous assignment.

### Person 1: Jennifer
One of the initial things that Jennifer did was press the top-left logo of the website, which did nothing, and then consequently pressed the "Home button," which also did not do anything since she was already at the home page. She hesitated a bit in creating a user profile but it was because she wanted to play around the website. She did not have trouble creating a user profile nor a travel profile, though she did note that if she was creating a website, she would want the "Location" section to be able to type in the city name, likely also the country name as well. Once on the rating page, she said she liked how the colors would change, but she also wished that when she was trying to match with people, it would show the people in an organized order, perhaps in a way so she doesn't have to find her ideal match at the very end of the scroll. She didn't love the scroll either because if there were many people, she would have to scroll too far down. Then, we transitioned into the drawings of the wireframes I made, where I had her narrate what actions she wanted to do and press on the paper. She was able to start a safe meeting well. She was surprised about the option to put in an emergency contact number and asked me why since the wireframe had no explanation to the user they had to include an emergency contact. She was able to propose a safe meeting successfully. When going on the map (where I had her pretend it was an interface similar to Google Maps). She pressed on the city she was in and then the information appeared, and she was able to "find" the user fairly easily by pressing the city, but she then told me what would happen if I had so many friends, and that it would look crowded. I never considered this issue before. Overall, she didn't seem as confused at times, but she did have many critiques about considering certain aspects of real-world implementation. 

### Person 2: Lydia
Lydia was super "smiley" during the entire process and expressed great enthusiasm about trying the website out. She said that when seeing the first photo, she would've done a more positive and bright photo because the dark photo didn't give as "cute" of energy. She also had no problem with creating a profile and she liked the questions (I later asked her what additional questions she would add and then we went through a plethora of ideas -- How comfortable are you about trying new foods? Would you be open to traveling in groups with a potential match? etc.). She had no problem with the rating feature and did not comment about the organization of the different user profile cards. However, she said that there was a lot of white space surrounding the user profile. She enjoyed moving her cursor around the cards because of the shading as well. She was able to start a safety meeting too and did not mention anything in terms of complaints, just that it was nice that I included one of the features we initially talked about from our initial talk about the app. For the map feature (where I showed her Google Maps to pretend it was the feature and told her to imagine there was user profiles at the top), she said that one thing she thought could be annoying is pressing random countries to find people. If she wanted to see where the most people are, there is no way for her to find out unless she manually clicked through every city. But she liked the idea of being able to see where other people are. She had to find where Barcelona to find her match in Barcelona (which took her a little while as she was going around Google Maps), but she said if it was a city she didn't know that well, she'd feel dumb about not knowing where the city was. She also didn't know exactly how to go to a user profile after, since she didn't automatically assume you would have to press the user profile, so it was only when she verbally said she'd press the user she was able to. She said she overall thought the app was very cute. 

## Flaws and Opportunities for Improvement

1. **Unresponsive Logo and Home Button**
   - **Classification**: Linguistic, Minor
   - **Conceptual Issue**: 
     - **Issue**: Jennifer pressed the logo and "Home" button expecting some response, but nothing happened as she was already on the home page. This caused initial confusion.
     - **Cause**: The interface didn't provide clear feedback to indicate that she was already on the homepage, making the logo and Home button feel non-functional. Additionally, there are technically two buttons that do the same function. 
   - **Scale of Improvement**:
     - **Solution**: Implement feedback to indicate when the user is already on the home page, such as a subtle highlight on the Home button or a tooltip explaining that the user is already on the main page. Additionally, I would get rid of the "Home" button since the logo button should be at the top left, as most pages are. 

2. **Limited Location Selection in Profile Creation**
   - **Classification**: Conceptual, Moderate
   - **Conceptual Issue**:
     - **Issue**: Jennifer mentioned wanting a location input that allows typing, such as the ability to type in both city and country names, rather than selecting from a dropdown.
     - **Cause**: The current dropdown design restricts user input, which may be inconvenient for users who are used to typing in locations or have locations not listed in the dropdown, especially if the website expanded to many different locations. 
   - **Scale of Improvement**:
     - **Solution**: Implement an auto-complete search bar for location input, allowing users to type in their city and country. Using a location API could help to the list dynamically, especially because if someone was to not use lowercase or have a spelling error, the search bar could help to assume what the country and city that was being thought actually is. 

3. **Cluttered Scroll Interface for Rating Profiles**
   - **Classification**: Physical, Major
   - **Conceptual Issue**:
     - **Issue**: Jennifer found the scroll interface for viewing and rating profiles inconvenient, especially if many profiles were present, as it required excessive scrolling.
     - **Cause**: The app lacks an organized view or filtering options for profiles, making it a bit hard for users to scroll through a long list.
   - **Scale of Improvement**:
     - **Solution**: Consider adding filters or categorization options (e.g., sort by compatibility or location) to help users find matches more easily. Jennifer suggested having it default be organized by compatibility since she thought that would be the most important for users. There could also be page numbering, and then also if a profile is rated, perhaps that profile would also disappear from the user list so that there wouldn't be 99999 pages of users. But with that, there could be an undo feature as well if someone accidentally voted someone the wrong rating.

4. **Emergency Contact Field Confusion in Safe Meeting**
   - **Classification**: Linguistic, Moderate
   - **Conceptual Issue**:
     - **Issue**: Jennifer was surprised by the emergency contact field in the safe meeting setup, as there was no explanation as to why it was required.
     - **Cause**: The absence of context or explanation for the emergency contact field left the user unsure about its purpose.
   - **Scale of Improvement**:
     - **Solution**: Add a short description or tooltip explaining why an emergency contact is necessary, highlighting the feature's focus on safety, and also that the option would be optional to users if they were not comfortable.

5. **Lack of Map Indicators for Popular User Locations**
   - **Classification**: Conceptual, Moderate
   - **Conceptual Issue**:
     - **Issue**: Lydia mentioned difficulty in finding friends on the map and highlighted the need to click through cities individually to locate users, which could be tedious for users.
     - **Cause**: The current map interface lacks indicators for cities with many users, requiring users to manually click through locations.
   - **Scale of Improvement**:
     - **Solution**: There could be a form of a heatmap marker (similar to SnapChat's map feature) to mark where there is the most activity, and have markers for the cities that are either the most popualar on the app or most popular in regard to the users you have matched with. This would allow users to be able to more quickly find populated areas and see where their friends are.

6. **Navigation Confusion on the Map**
   - **Classification**: Conceptual, Minor
   - **Conceptual Issue**:
     - **Issue**: Lydia struggled to find cities and did not immediately assume that pressing on a user profile would lead to their profile page.
     - **Cause**: The user interface did not provide clear guidance for navigating the map or accessing user profiles from it.
   - **Scale of Improvement**:
     - **Solution**: Provide a tooltip or introductory guide when people first open the Locating feature that explains how to find users on the map, access their profiles, and navigate the interface. Consider adding labels to map markers or a search feature for cities for people who might not know where every city is.

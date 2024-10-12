---
title: Assignment 4
layout: doc
---

# Assignment 4 Milestone

## Reformed Concepts 

### Concept: Authenticating

**Purpose:** Authenticate users to securely manage their profile for interactions with other users.

**Principle:** After a user registers with a username and password pair, they can authenticate as that user by providing a matching username and password

**State:**
- **registered:** set User
- **username, password:** User -> one String

**Actions:**
- `register (username, password: String, out u: User)`
- `authenticate (username, password: String, out u: User)`

---

### Concept: UserProfiling

**Purpose:** Store and manage user profile information.

**Principle:** After a user puts in their profile details, it is set to their profile. A person can update their profile their profile or delete their profile.

**State:**
- **qualities:** set gender | age | travel_style | location | question_1 | question_2 -> one ProfileDoc

**Actions:**
- `updateProfile(userID: ObjectId, profileDetails: ProfileDoc)`
- `getProfile(userID: ObjectId)`
- `deleteProfile(userID: ObjectId)`

---

### Concept: Sessioning

**Purpose:** Manage user sessions to ensure secure interactions within the app.

**Principle:** After a session starts (and before it ends), the getUser action returns the user identi ed at the start:
- `start (u, s); getUser (s, u') {u' = u}`

**State:**
- **active:** set Session 
- **user:** active -> one User

**Actions:**
- `start (user: User, out sess: Session)`
- `getUser (sess: Session, out user: User)`
- `end (sess: Session)`

---

### Concept: Matching

**Purpose:** Mutually match users based on profile compatibilty and user preferences

**Principle:** Users can rate each other, and if both "like" each other, they are considered a match

**State:**
- **ratings:** map (raterID, rateeID) → boolean (true as like, false as dislike)
- **potentialMatches:** map (userID) → list of rateeIDs (potential matches for the userID)
- **matchScore:** matchScore: map (userID, potentialUserID) → integer (compatibility score between the userID and the potentialUserID, aka their potential match)

**Actions:**
- `rateUser(raterID: ObjectId, rateeID: ObjectId, like: boolean):` Records a rating from one user to another
- `checkMatch(raterID: ObjectId, rateeID: ObjectId):` Checks if both users have liked each other, which means they have matched
- `getMatches(userID: ObjectId):`  Retrieves a list of all the matches for a given user
- `calculateCompatibility(userID: ObjectId, potentialUserID: ObjectId):` Calculates compatibility number between two users based on their profile attributes

---

### Concept: Chatting

**Purpose:** Allow for users to communicate

**Principle:** Users can initiate a chat and exchange messages between each other


**State:**
- **chats:** map (chatID → list of messages)
- **participants:** map (chatID → set of userIDs)
- **messageHistory:** map (chatID → list of MessageDoc)

**Actions:**
- `startChat(user1ID: ObjectId, user2ID: ObjectId):` Begin a chat session between two users.
- `sendMessage(chatID: ObjectId, senderID: ObjectId, message: string):` Sends a message within the specified chat session.
- `receiveMessages(chatID: ObjectId):` Retrieves all messages for a given chat session.
- `endChat(chatID: ObjectId):` Ends a chat session

---

### Concept: SafeMeeting

**Purpose:** Allow users to propose and confirm meetings while ensuring safety measures.

**Principle:** Users can schedule meetings by specifying time, date, location, and emergency contact information

**State:**
- **date:** Date
- **time:** Time
- **location:** String
- **phoneNumber:** String
- **meetings:** map (meetingID → meetingDetails)
- **emergencyContacts:** map (meetingID → contactDetails)
- **meetingStatus:** map (meetingID → "confirmed" | "proposed" | "denied")

**Actions:**
- `proposeMeeting(proposerID: ObjectId, receiverID: ObjectId, date: Date, time: Time, location: String, emergencyContact: String):` Proposes a meeting with specified details.
- `confirmMeeting(meetingID: ObjectId):` Confirms a proposed meeting.
- `denyMeeting(meetingID: ObjectId):` Denies a proposed meeting.
- `setEmergencyContact(userID: ObjectId, phoneNumber: String):` Sets an emergency contact for the user.

---

### Concept: Locating

**Purpose:** Allow users to share their locations and provide contextual information about those locations

**Principle:** Users can share their locations, which can be viewed by other users

**State:**
- **locationSharing:** map (userID → sharingStatus bool) // true if sharing with all users, false if not sharing
- **locations:** map (userID → location data)
- **locationDetails:** LocationDetails {
    locationName: string;
    locationDesc: string;
    locationAttr: string[];
}
- **locationInfo:** map (location → locationDetails)


**Actions:**
- `enableLocationSharing (userID: User, share: boolean):` Sets the location sharing status for the user.
- `updateLocation (userID: User, location: Location):` Updates the user's location, if sharing is enabled
- `viewLocation (userID: User):` Allows users to see locations of others if they are sharing.

---

## Synchronization: 

**app** Nomadly

**include** Authentication [User]  
**include** Session [Session]  
**include** UserProfiling [Profile]  
**include** Matching [Match]  
**include** Chatting [Chat]  
**include** SafeMeeting [Meeting]  
**include** Locating [Location]

### When a User Profile is Updated
**sync** `updateProfile (userID: User, profileDetails: ProfileDoc)`
- `UserProfiling.updateProfile(userID, profileDetails)`

### When a Survey is Completed
**sync** `surveyComplete (userID: User, surveyResults: JSON)`
- `UserProfiling.updateProfile(userID, surveyResults)`
- `Matching.calculateCompatibility(userID, potentialUserID)`

### When a Profile is Liked
**sync** `likeProfile (raterID: User, rateeID: User)`
- `Matching.rateUser(raterID, rateeID, like=true)`
- `Chatting.startChat(raterID, rateeID)`
- `Locating.viewLocation (userID: User)`

### Propose Safe Meeting
**sync** `proposeSafeMeeting (proposerID: User, receiverID: User, date: Date, time: Time, location: String, emergencyContact: String)`
- `SafeMeeting.proposeMeeting(proposerID, receiverID, date, time, location, emergencyContact)`

### Confirm Safe Meeting
**sync** `confirmSafeMeeting (meetingID: MeetingID)`
- `SafeMeeting.confirmMeeting(meetingID)`

### Deny Safe Meeting
**sync** `denySafeMeeting (meetingID: MeetingID)`
- `SafeMeeting.denyMeeting(meetingID)`

### Update Location
**sync** `updateLocation (userID: User, location: Location)`
- `Locating.updateLocation(userID, location)`

### Enable Location Sharing
**sync** `enableLocationSharing (userID: User, share: boolean)`
- `Locating.enableLocationSharing(userID, share)`

### View Location
**sync** `viewLocation (userID: User)`
- `Locating.viewLocation(userID)`

## Database Diagram
<img src="./images/a4_databasemodel_final.png" alt="alt text" width="500" height="300">

## Implementation
### [Vercel](https://a4-backend-starter-l767yoth2-heathers-projects-1dcdd1b4.vercel.app)

### [GitHub Repo of Concepts](https://github.com/heather-parkk/a4-backend-starter/tree/main/server/concepts)

### [Github Repo of Util](https://github.com/heather-parkk/a4-backend-starter/blob/main/public/util.ts)

### [Github Repo of Routes](https://github.com/heather-parkk/a4-backend-starter/blob/main/server/routes.ts)


## Design Reflection
I had the difficulty rearranging how I wanted to implement each of my concepts. I had to go back and reread how to define each of my states and actions, especially since that would help with developing what routes I thought would be most useful. I think I was originally ambiguous with certain types and how certain actions went together because I didn't have as full of an understanding, but from A3 feedback, I decided to add a new concept: UserProfiling and Sessions. This was a good decision because I did not consider how every time a user would authenticate themselves on the app, they may need to create a new user profile. T

 am still having some difficulties with some design aspects of my app's backend. When testing the backend, I still receive some "Bad Request: validation failed" error during profile updates, but I was hoping to receiving some guidance from the TFs during Office Hours to help me with these errors (thank you to the TFs who have already helped me so much with implementation since I was having so much errors, especially with my lack of background without 6.1020). 

I decided to use a ProfileDoc for UserProfiling, to be easier in implementing the code and I could validate each field to make sure each value was processed. For locations, a JSON file or being able to scrape information about cities since I instead did manually, which wouldn't be as effective. The decision I made to temporarily fill out the information was to be able to have a working backend service for now, but if I was able to study up more and have more time, I'd want to figure out how to ideally scrape information and put it in a JSON format to have all that data.
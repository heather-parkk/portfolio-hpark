---
title: "Project Phase 3: Convergent Design"
layout: doc
---


# Project Phase 3: Convergent Design

## Functional Design

### New Concepts
#### Profiling

**Concept**: Profiling [User, Item]  
**Purpose**:  
Asks users predefined questions with options, with the user’s option selections to be updated on their profile.  

**Operational Principle**:  
User \( U \) is given a question \( Q \) with multiple-choice options \( A, B, C, \dots \) for their question \( Q \), where they then select any choices to be updated to the profile of user \( U \).  

**State**:  
- `userResponses: User -> set Item`  
- `question -> one String`  
- `choices -> set String`  

**Actions**:  
- `ask (user: User, question: String, choices: set String, out selected: set String)`  
  - Set `userResponses` of user to choices that user selects.  
- `updateProfile (user: User, responses: set Item)`  
  - Set `userResponses` for user to be item.  
- `getUserResponses (user: User, out responses: set Item)`  
  - Retrieve `userResponses` for the specific user.  

---

#### Notifying

**Concept**: Notifying [User, Group, Item, Date]  
**Purpose**:  
Notify users about changes in the app.  

**Operational Principle**:  
If a change \( C \) occurs within group \( G \), users \( U \) associated with the group \( G \) are notified about the change.  

**State**:  
- `group: Group -> set User`  
- `change: one String`  
- `timeSent: one Date`  
- `notifications: User -> set Item`  

**Actions**:  
- `notifyGroup (group: Group, change: String, out notifiedUsers: set User)`  
  - Send notification about the change to users in the group.  
  - Add notification to `notifications` for each user in the group.  
- `getNotifications (user: User, out userNotifications: set Item)`  
  - Retrieve notifications that the user has received.  

---

#### Threading

**Concept**: Threading [User, Post, Collection, Date]  
**Purpose**:  
Groups items related to the same topic.  

**Operational Principle**:  
After a user \( U \) creates a thread \( T \), the user can add post \( P \) to the thread. Each added piece of media \( M, M_1, \dots M_N \) will appear in chronological order by date added.  

**State**:  
- `setOfThreads: set Collection`  
- `creator: Collection -> one User`  
- `timeCreated: Collection -> one Date`  
- `members: Collection -> set Users`  
- `content: Collection -> set Posts`  
- `title: Collection -> one String`  

**Actions**:  
- `create (user: User, title: String, content: Post, members: User, out thread: Collection)`  
  - Set thread title to `title`.  
  - Set thread members to `members`.  
  - Set thread content to `content`.  
  - Set thread creator to `user`.  
  - Add thread to `setOfThreads`.  
- `addPost (thread: Thread, post: Post)`  
  - Add post to thread content.  
- `editThreadTitle (user: User, title: String, thread: Collection)`  
  - Require that user is the creator of the thread.  
  - Update thread title to `title`.  
- `deleteThread (user: User, thread: Collection)`  
  - Require that user is the creator of the thread.  
  - Remove thread from `setOfThreads`.  
- `getThreadPosts (thread: Collection)`  
  - Get the posts in the thread.  

---

#### Archiving

**Concept**: Archiving [Post, User, Date, Collection]  
**Purpose**:  
Allows items that share a common theme to be archived together.  

**Operational Principle**:  
Items \( I \) that user \( U \) selects can be grouped into a group \( G \).  

**State**:  
- `setOfArchives: set Collections`  
- `content: Collection -> set Posts`  
- `creator: Collection -> one User`  
- `timeCreated: Collection -> one Date`  
- `timePeriod: Collection -> one Date`  
- `caption: Collection -> one String`  

**Actions**:  
- `createArchive (user: User, posts: Post, timePeriod: Date, caption: String, out archive: Archive)`  
  - Set caption to `caption`.  
  - Set creator to `user`.  
  - Set `timePeriod` to `timePeriod`.  
  - Add posts to content.  
  - Add archive to `setOfArchives`.  
- `updateCaption (user: User, newCaption: String, archive: Item)`  
  - Require that user is the creator of the archive.  
  - Update caption of archive to `newCaption`.  
- `deleteArchive (user: User, archive: Item)`  
  - Require that user is the creator of the archive.  
  - Delete archive from `setOfArchives`.  
- `addToArchive (post: Post, out archive: Collection)`  
  - Add post to archive.  
- `deleteFromArchive (user: User, post: Post, archive: Collection)`  
  - Require that user is the author of the post.  
  - Delete post from archive.  

---

### Old Concepts
#### Posting

**Concept**: Posting [User, Item, Date]  
**Purpose**:  
Allows users to create items for others to view.  

**Operational Principle**:  
After a user creates a post, another user can view the post. If a user is the author of the original post, that user can update the post item or delete the post item. Users can also view the posts they have personally made.  

**State**:  
- `postAuthor: Item -> one User`  
- `postContent: Item -> one String`  
- `postDateCreated: Item -> one Date`  
- `postContentType: Item -> one ContentType`  
- `userPosts: User -> set Item`  

**Actions**:  
- `createPost (author: User, date: Date, content: String)`  
  - Set item's `postDateCreated` to `date`.  
  - Set item's `postAuthor` to `author`.  
  - Set item's `postContent` to `content`.  
  - Add item to `userPosts`.  
- `removePost (postToRemove: Item, postRemover: User)`  
  - Require that `postAuthor` of the post is the `postRemover`.  
  - Remove `postToRemove` from `userPosts`.  
- `updatePost (postUpdater: User, newContent: String, post: Item)`  
  - Require that the `postAuthor` of the post is the `postUpdater`.  
  - Update `postContent` of post with `newContent`.  
- `getPosts (user: User, out userPosts)`  
  - Retrieve posts in `userPosts` of user.  

---

#### Commenting

**Concept**: Commenting [User, Reply, Item, Date]  
**Purpose**:  
Allows users to reply to viewable items.  

**Operational Principle**:  
After adding a comment on an item, users can view the comment on the item whenever the item is in view, and the original user can delete or update the comment they made.  

**State**:  
- `setOfComments: Item -> set Reply`  
- `commentAuthor: Reply -> one User`  
- `commentContent: Reply -> one String`  
- `commentTimeCreated: Reply -> one Date`  

**Actions**:  
- `addComment (author: User, itemToReplyTo: Item, date: Date, content: String)`  
  - Set comment's `commentAuthor` to `author`.  
  - Set comment's `commentTimeCreated` to `date`.  
  - Set comment's `commentContent` to `content`.  
  - Add comment to `setOfComments` for `itemToReplyTo`.  
- `deleteComment (commentRemover: User, deleteReply: Reply, repliedItem: Item)`  
  - Require that `commentRemover` is `commentAuthor`.  
  - Remove `deleteReply` from `setOfComments` for `repliedItem`.  
- `updateComment (commentUpdater: User, newContent: String, comment: Reply)`  
  - Require that `commentUpdater` is `commentAuthor`.  
  - Replace original content of comment with `newContent`.  

---

## Sessioning

**Concept**: Sessioning [User, Session]  
**Purpose**:  
Extends authenticated actions for a user that has previously authenticated.  

**Operational Principle**:  
After a session starts, the `getUser` action returns the user identified in the same session.  

**State**:  
- `active: set Session`  
- `user: active -> one User`  

**Actions**:  
- `start (user: User, out sess: Session)`  
  - Assigns the session to the user and adds the session to `active`.  
- `getUser (sess: Session, out user: User)`  
  - Retrieves the user associated with the active session.  
- `end (sess: Session)`  
  - Removes the session from `active`.  

---

## Authenticating

**Concept**: Authenticating [User]  
**Purpose**:  
Ensures that the user of the app corresponds to real people.  

**Operational Principle**:  
If one registers with a username and password to make a user, one can access that user with the same pair.  

**State**:  
- `registeredUsers: set User`  
- `username: registeredUsers -> one String`  
- `password: registeredUsers -> one String`  

**Actions**:  
- `register (name: String, pass: String)`  
  - Ensures the username does not already exist.  
  - Creates a new user with the specified username and password.  
  - Adds the user to `registeredUsers`.  
- `unregister (name: String, pass: String)`  
  - Verifies that the username and password match.  
  - Removes the user from `registeredUsers`.  
- `authenticate (name: String, pass: String, out u: User)`  
  - Verifies the username and password.  
  - Outputs the corresponding user.  

---

## Familying

**Concept**: Familying [User]  
**Purpose**:  
Allows users to grant access to their activity and see others’ activity within a defined family group.  

**Operational Principle**:  
If User \( A \) and User \( B \) are family members, the pair \( (A, B) \) or \( (B, A) \) will be stored in the family database.  

**State**:  
- `familyData: set (User, User)`  
- `items: User -> set Item`  

**Actions**:  
- `addFamily (self: User, familyMember: User)`  
  - Adds the pair \( (self, familyMember) \) to `familyData`.  
- `removeFamily (self: User, familyMember: User)`  
  - Removes the pair \( (self, familyMember) \) or \( (familyMember, self) \) from `familyData`.  
- `checkFamily (self: User, toCheck: User, out b: Boolean)`  
  - Checks if the specified user is in the family of the current user.  

---

## Posting

**Concept**: Posting [User, Item, Date]  
**Purpose**:  
Allows users to create items for others to view.  

**Operational Principle**:  
After a user creates a post, another user can view the post. If a user is the author of the original post, that user can update or delete the post.  

**State**:  
- `postAuthor: Item -> one User`  
- `postContent: Item -> one String`  
- `postDateCreated: Item -> one Date`  
- `postContentType: Item -> one ContentType`  
- `userPosts: User -> set Item`  

**Actions**:  
- `createPost (author: User, date: Date, content: String)`  
  - Sets the post's `postDateCreated`, `postAuthor`, and `postContent`.  
  - Adds the post to `userPosts`.  
- `removePost (postToRemove: Item, postRemover: User)`  
  - Requires that the `postAuthor` matches the `postRemover`.  
  - Removes the post from `userPosts`.  
- `updatePost (postUpdater: User, newContent: String, post: Item)`  
  - Requires that the `postAuthor` matches the `postUpdater`.  
  - Updates the `postContent` and `postContentType`.  
- `getPosts (user: User, out userPosts)`  
  - Retrieves the posts associated with the user.  

---

## Commenting

**Concept**: Commenting [User, Reply, Item, Date]  
**Purpose**:  
Allows users to reply to viewable items.  

**Operational Principle**:  
After adding a comment on an item, users can view the comment whenever the item is in view. The original user can delete or update the comment they made.  

**State**:  
- `setOfComments: Item -> set Reply`  
- `commentAuthor: Reply -> one User`  
- `commentContent: Reply -> one String`  
- `commentTimeCreated: Reply -> one Date`  

**Actions**:  
- `addComment (author: User, itemToReplyTo: Item, date: Date, content: String)`  
  - Assigns the `commentAuthor`, `commentTimeCreated`, and `commentContent`.  
  - Adds the comment to `setOfComments` for the specified item.  
- `deleteComment (commentRemover: User, deleteReply: Reply, repliedItem: Item)`  
  - Requires that the `commentRemover` matches the `commentAuthor`.  
  - Removes the comment from `setOfComments` for the item.  
- `updateComment (commentUpdater: User, newContent: String, comment: Reply)`  
  - Requires that the `commentUpdater` matches the `commentAuthor`.  
  - Updates the `commentContent` with the new content.  


### Synchronizations

---

#### **Registers the User**
**sync** `(username: String, password: String, out user: User):`  
- `Authenticating.register(username, password, user)`  
- For each familyMember in `familyMembers`:
  - `Familying.addFamily(user, familyMember)`  
  - `Familying.addFamily(familyMember, user)`  

---

#### **Determine User's Responses**
**sync** `askUserResponse (question: String, choices: set String, session: Session, out selected: set String):`  
- `Sessioning.getUser(session)`  
- `Profiling.ask(user, question, choices, selected)`

---

#### **Get User's Responses**
**sync** `getUserResponses (session: Session):`  
- `Sessioning.getUser(session)`  
- `Profiling.getUserResponses(user, responses)`

---

#### **Get Notifications of a User**
**sync** `getUserNotifications (session: Session):`  
- `Sessioning.getUser(session)`  
- `Notifying.getNotifications(user, userNotifications)`

---

#### **Authenticates the User and Starts a Session**
**sync** `loginUser (username: String, password: String, out user: User, out session: Session):`  
- `Authenticating.authenticate(username, password, user)`  
- `Sessioning.start(user, session)`

---

#### **Creating a Thread That Contains Posts**
**sync** `createThreadWithPost (user: User, title: String, threadContent: Post, members: User[], date: Date, content: Item, group: Group, out thread: Collection):`  
- `Sessioning.getUser(session)`  
- `Threading.create(user, title, threadContent, members, thread)`  
- `Posting.createPost(user, date, content)`  
- `Notifying.notifyGroup(group, "User ‘user.username’ created a new thread called ‘thread.title’!", notifiedUsers)`

---

#### **Adding a Post to a Thread**
**sync** `addPostToThread (user: User, date: Date, content: Item, group: Group, thread: Thread):`  
- `Posting.createPost(user, date, content)`  
- `Threading.addPost(thread, post)`  
- `Notifying.notifyGroup(group, "User ‘user.username’ made a new post in thread ‘thread.title’!", notifiedUsers)`

---

#### **Commenting on a Post in a Thread**
**sync** `commentThread (user: User, post: Post, commentContent: String, date: Date, thread: Collection):`  
- `Threading.getThreadPosts(thread)`  
  - If `post` not in `posts`:
    - Throw "Post does not exist in the specified thread"
- `Commenting.addComment(user, post, date, commentContent)`  
- `Notifying.notifyGroup(thread.members, "New comment added to post", notifiedUsers)`

---

#### **Edit a Thread’s Title**
**sync** `editThreadTitle (session: Session, newTitle: String, thread: Collection):`  
- `Sessioning.getUser(session)`  
  - If `thread.creator != user`:
    - Throw "Permission denied: Only thread creator can edit the thread title"  
- `Threading.editThreadTitle(user, newTitle, thread)`

---

#### **Delete a Thread**
**sync** `deleteThread (session: Session, thread: Collection):`  
- `Sessioning.getUser(session)`  
  - If `thread.creator != user`:
    - Throw "Permission denied: Only thread creator can delete this thread"  
- `Threading.deleteThread(user, thread)`

---

#### **Deletes a Post and Its Comments**
**sync** `deletePostAndComments (user: User, post: Post):`  
- `Posting.removePost(post, user)`  
- For all `comments` on the post:
  - `Commenting.deleteComment(user, comment, post)`

---

#### **Delete a Comment**
**sync** `deleteCommentFromPost (user: User, comment: Reply, post: Post):`  
- If `Commenting.commentAuthor[comment] != user`:
  - Throw "Permission denied: Only the comment creator can delete the comment"  
- `Commenting.deleteComment(user, comment, post)`

---

#### **Create an Archive**
**sync** `createArchive (user: User, posts: Post, timePeriod: Date, caption: String, group: Group, change: String, out archive: Archive):`  
- `Archiving.createArchive(user, posts, timePeriod, caption, archive)`  
- `Notifying.notifyGroup(group, "New archive called archive.caption has been created!", notifiedUsers)`

---

#### **Update an Archive’s Caption**
**sync** `updateArchiveCaption (session: Session, newCaption: String):`  
- `Sessioning.getUser(session)`  
- `Archiving.updateCaption(user, newCaption)`

---

#### **Delete an Archive**
**sync** `deleteArchive (session: Session, archive: Collection):`  
- `Sessioning.getUser(session)`  
- `Archiving.deleteArchive(user, archive)`

---

#### **Add Post to Archive**
**sync** `addPostToArchive (post: Post, group: Group, change: String, out archive: Collection):`  
- `Archiving.addToArchive(post, archive)`  
- `Notifying.notifyGroup(group, "New post added to archive.caption archive!", notifiedUsers)`

---

#### **Delete Post From Archive**
**sync** `deletePostFromArchive (session: Session, post: Post, archive: Collection):`  
- `Sessioning.getUser(session)`  
- `Archiving.deleteFromArchive(user, post, archive)`

### [Dependency Diagram](https://www.figma.com/design/zlMTlEkdB767rvdTfWRY1W/Fam.ly?node-id=0-1&t=e4RxQu9QbMmS2bpx-0)
<img src="./images_p3/image (78).png" alt="alt text" width="500" height="350">

## Wireframes
### [Wireframes Full Display on Figma (with labels)](https://www.figma.com/design/zlMTlEkdB767rvdTfWRY1W/Fam.ly?node-id=0-1&node-type=canvas&t=FMHncAIFogfKYBMl-0)
<img src="./images_p3/image (71).png" alt="alt text" width="500" height="350">

### [Wireframe Display (Close-up)](https://www.figma.com/proto/zlMTlEkdB767rvdTfWRY1W/Fam.ly?page-id=0%3A1&node-id=109-1322&node-type=canvas&viewport=125%2C370%2C0.12&t=5g2Lsh3O50tHxbcK-1&scaling=scale-down-width&content-scaling=fixed&starting-point-node-id=43%3A230)

## Heuristic Evaluation

### Usability Criteria
---

#### Discoverability
**How rapidly and easily can users understand how to operate the interface?**

- **Family Page as Main Entry Point**:  
  The primary interaction point for users is the **family page**, where they can create or interact with threads. All pages in the application redirect users to the family page either directly or indirectly.  

- **Onboarding Flow**:  
  After completing the questions on the **"Describe Yourself"** page, users are encouraged to try the home page with a clear button to redirect them.  

- **Breadcrumb Navigation**:  
  The **memories page** uses a breadcrumb path (e.g., "Family A > Thread a > Memory m") to provide context. Clicking on "Family A" redirects the user to the respective family page, ensuring clear navigation.

---

#### Efficiency
**Can users efficiently accomplish their goals once they know how to use the interface?**

- **Recent Updates on the Home Page**:  
  The home page prominently displays the **most recently updated thread**, enabling users to access it in a single click.  

- **Accessing Older Threads**:  
  For older threads, users can navigate to the family page, select the desired family, and locate the thread efficiently.  
  - **Typical Navigation Time**:  
    - **1 Click**: For recently updated threads.  
    - **Up to 3 Clicks** (with some scrolling): For any thread in the application.

---

### Physical Heuristics
#### Gestalt Principles
**Does the layout of the interface elements convey conceptual structure?**

- **Thread and Family Relationship**:  
  - Each thread belongs to a specific family. This relationship is emphasized in the interface, ensuring users do not mistakenly think a thread can belong to multiple families.  

- **Consistency Across Pages**:  
  - On the home page, active threads display the associated family name.  
  - On the family page, threads are listed under each family, reinforcing the family-thread relationship.  
  - For memories, the associated family is shown next to each memory, clarifying the hierarchy: **Memory → Thread → Family**.

---

#### Situational Context
**How does the interface convey to a user their context and adapt to their current state?**

- **Page Titles**:  
  Titles like **"The Smith Family"** indicate the user's current location within the app.  

- **Breadcrumbs**:  
  Breadcrumb navigation provides clear context. For instance:  
    - If in **Family A**: Displays "Family A".  
    - If in **Thread a** within **Family A**: Displays "Family A > Thread a".  
    - If in **Memory m** from **Thread a**: Displays "Family A > Thread a > Memory m".

---

### Linguistic Level
#### Consistency
**Does the interface reuse the same names, symbols, and icons for the same concepts or actions?**

- **Profile vs. Describe Yourself**:  
  - Instead of a traditional "Profile" page, the app uses "Describe Yourself". This avoids the association with personal history or friends, commonly seen in other apps, and focuses on providing suggestions for questions/prompts within families.

- **Family Page Naming**:  
  - The **"Family" page** is conceptually similar to "Groups" in Facebook or Messenger. However, the name "Family" is retained to foster a closer connection among members and align with the app's core purpose: **building connections within families**.

---

#### Information Scent
**How does the interface provide hints for navigation to aid a user in “foraging” for information?**

- **Icons and Text for Guidance**:  
  - For instance, when creating a new thread within a family, the page includes:  
    - A **back arrow** labeled "Back" to guide users back to the previous page.  
    - **Icons** for AI, voice, photo, and video functionalities within the text box, encouraging exploration.  

- **Navigation Prompts on the Home Page**:  
  - The home page lists major pages and functionalities, prompting users to click and explore different parts of the application.

  ---

## Design Iteration

---

### Viewing the Memory: Removal of the Tree Structure
#### Original Proposal:
- We initially proposed a **family tree hierarchy** for users to visualize relationships within the family.  
- **Challenges Identified**:  
  - Complexity in representing diverse real-life relationships (e.g., divorced, deceased members).  
  - Irrelevance for non-family users, where the concept of a tree is unnecessary.  

#### Updated Design:
- Replaced the hierarchical tree with a **non-hierarchical list of family members**.  
- Users can:  
  - **View all family-related memories** displayed on a timeline.  
  - **Click on individual family members** to see their specific memories.

---

### Hierarchy of Family, Thread, Post, Comment
#### Current Design:
- **Hierarchy**:  
  - **Family → Thread → Post → Comment**.  
  - Users can:  
    - Create threads within a family.  
    - Add posts to threads, which serve as the smallest meaningful content unit.  
    - Comment on posts to facilitate discussions.  

#### Tradeoffs:
- **Advantages**:  
  - Provides a clear mental model for organizing information.  
  - Keeps content structured and easy to navigate.  
- **Challenges**:  
  - The multi-layered structure may confuse older users.  

#### Solution:
- Retain the hierarchical structure for its organizational benefits.  
- Improve **situational awareness** by adding **breadcrumb navigation** (e.g., "Family A > Thread a > Memory m") to help users understand their location in the app.  

---

### What to Store in the Memory
#### Considerations:
- **Thread**:  
  - Contains multiple pieces of information but is always accessible from the family page.  
  - Storing the entire thread is unnecessary.  

- **Post**:  
  - Represents the smallest meaningful content unit.  
  - Contains related and organized content.  
  - Most suitable for inclusion in a memory.  

- **Comment**:  
  - Informal and casual in nature.  
  - Does not align with the goal of making memories more formal and structured.  

#### Final Decision:
- Allow users to **select multiple posts** within the same thread to create a memory.  
- Users can **add a caption** to provide additional context for the memory.

---

### Use of AI
#### Challenges with Mandatory AI:
- Initially, we required users to answer AI-driven questions upon login.  
- **Issues Identified**:  
  - Users may hesitate to share personal information with an unfamiliar application.  
  - Some users may dislike AI or prefer not to involve it in family matters.

#### Updated Design:
- **AI Usage is Optional**:  
  - Users can choose to interact with AI when needed.  
  - Answering questions from the application is optional and only triggered by user action.  

- **AI Integration in Threads**:  
  - The AI-related interface is represented by a small **icon**.  
  - Prompts or topics suggested by AI are displayed only when the user clicks the icon.

#### Benefits:
- Balances the tradeoff between **AI assistance** and **user privacy**.  
- Ensures a safe space for users who prefer not to use AI while providing enhanced functionality for those who do.

---

## Implementation Order

We plan to implement **X novel concepts** in the following order based on their importance to the app's purpose:

1. **Profiling**  
2. **Threading**  
3. **Archiving**  
4. **Notifying**  

### Rationale for Order
- The app’s primary purpose is to **invoke conversations within families** and allow families to **archive memories** for future reflection.  
- **Core Concepts**: Profiling, Threading, and Archiving are central to achieving this purpose and should be implemented first.  
- **Complexity Management**: After core concepts are complete, we will implement Notifying to add further functionality.  
- **Contingency Plan**: If we encounter issues with core concepts (e.g., AI-generated prompts being too complex), we will simplify or cut functionality (e.g., use hardcoded, randomized prompts instead).

---

## Visual Design Study

---

<img src="./images_p3/image (75).png" alt="alt text" width="500" height="350">

<img src="./images_p3/image (76).png" alt="alt text" width="500" height="350">



## General Timeline

### **Week 1**

#### **Backend Development**:
We aim to complete the backend for all concepts.  

##### **New Concepts** (Ends Saturday, 11/23):
- **Profiling**: Heather  
- **Threading**: Brianna  
- **Archiving**: Shih-Peng  
- **Notifying**: Kevin  

##### **Old Concepts** (Ends Saturday, 11/23):
Reusing concepts from previous applications:
- **Sessioning**: Heather  
- **Authenticating**: Heather  
- **Commenting**: Shih-Peng  
- **Posting**: Brianna  
- **Friending (Familying)**: Shih-Peng  

#### **Front-End Development (Focus on 2 Concepts)**:
Two team members will focus on developing the front-end for two concepts:
- **Profiling**: Heather & Shih-Peng  
- **Threading**: Brianna & Kevin  

#### **Work on Visualizations**:
Begin working on visualization features:
- **Sort by Time Period View**  
- **Sort by User View**  

---

### **Week 2**

#### **Complete Front-End Development**:
Finish the front-end for the remaining concepts based on Week 1 progress.  

#### **AI Implementation**:
- Integrate AI functionality into the app.  
- AI will be used for optional features like generating prompts or assisting with thread creation.  

---

## Notes and Adjustments
- Assignments for some concepts are still TBD and will be finalized based on team member availability and expertise.  
- We will monitor progress weekly and reallocate tasks if necessary.  
- Adjustments to the timeline may be made if unforeseen complexities arise.




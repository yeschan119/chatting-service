# puppose📢
  + realtime chatting service
    + between charge nurse and school nurses to take care of students
# company🐘 & department🍕
  + Kokomo24/7 Solution Inc & R&D
# member🧑‍💻
  Solo project
# platform & framework
  + Frontend - Angular framework
  + Backend - .Net framework
  + Server - AWS RDS
  + Database - MySQL
# Chat Service Diagram
<img width="500" height="400" alt="Screenshot 2024-11-25 at 15 44 02" src="https://github.com/user-attachments/assets/b43c1f01-3d8a-43e3-ac48-4b08dbd52e9f">
<img width="500" height="400" alt="Screenshot 2024-11-25 at 15 44 30" src="https://github.com/user-attachments/assets/eb25ce9f-b199-4116-81c1-2802c926b705">

---

# Project Result
  <img width="300" height="500" alt="Screenshot 2024-11-25 at 15 44 02" src="https://github.com/user-attachments/assets/b2e2ef59-7e07-467a-83d7-d4731b43f5bb"> |
  <img width="300" height="500" alt="Screenshot 2024-11-25 at 15 44 30" src="https://github.com/user-attachments/assets/cffeba69-612c-4215-b957-c547af83d02f">
  <img width="300" height="500" alt="Screenshot 2024-11-25 at 15 44 45" src="https://github.com/user-attachments/assets/48440b47-147f-4b0c-b032-c0b0ca209068"> |
  <img width="300" height="500" alt="Screenshot 2024-11-25 at 15 44 55" src="https://github.com/user-attachments/assets/55828f5c-b87b-4a19-a429-99063545ece4">
  <img width="300" height="500" alt="Screenshot 2024-11-25 at 15 45 15" src="https://github.com/user-attachments/assets/0b1185cd-ffef-4c40-ba7a-9fed3c22f0be"> |
  <img width="300" height="500" alt="Screenshot 2024-11-25 at 15 46 04" src="https://github.com/user-attachments/assets/c31e12d8-5dcc-47ef-8165-40624d3a4eba">
# Milestones
  + M1: Initial Setup
    + Create an Angular project and install the SignalR client
    + Set up the .Net backend and SignalR Hub
  + M2: Core Chat Functionality
    + Implement real-time message sending and receiving
    + Design the chat UI and integrate it with data binding
  + M3: Message Persistence
    + Design the database schema for storing messages
    + Implement API for fetching and paginating chat history
  + M4: Testing & Bug Fixing
    + Write unit and integration tests
    + Debug and optimize performance
  + M5: Deployment
# M1 - Initial Setup
  + Angular : create new component
  + .Net : create a chat hub class & add signalR service
# M2 - Core Chat Functionality
  + Frontend(Angular)
    + create chat module
    + declararations in chat.modult.ts
    + create chat service
    + code html, typescript, css to make a beautiful, flexible, responsive design
      + html : make chatting list, message window, message input using `ng-container` `mat-sidenav-container`, `mat-sidenav` `mat-form-field` `mat-label` ... tags
  + Set up the .Net backend and SignalR Hub
  + create user connections hub to use pending message until logging in users
# M3 - message persistence
  + Design the database schema for fetching chatting list & storing messages
    + Users: To store user information.
    + Chats: To store chat sessions or groups.
    + Messages: To store chat messages.
    + ChatParticipants: To link users with chat sessions.
    + MessageStatus: To track message read/unread status(optional).
  + Implement API for fetching & Storing
    + Frontend
    + Backend
      + create chat model
      +  create chat controller
  # M4 - Test & Debug

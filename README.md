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
![Screenshot 2024-11-26 at 10 38 19](https://github.com/user-attachments/assets/b43c1f01-3d8a-43e3-ac48-4b08dbd52e9f)
![Screenshot 2024-11-26 at 10 38 55](https://github.com/user-attachments/assets/eb25ce9f-b199-4116-81c1-2802c926b705)
# Project Result
  <img width="462" alt="Screenshot 2024-11-25 at 15 44 02" src="https://github.com/user-attachments/assets/b2e2ef59-7e07-467a-83d7-d4731b43f5bb"> |
  <img width="462" alt="Screenshot 2024-11-25 at 15 44 30" src="https://github.com/user-attachments/assets/cffeba69-612c-4215-b957-c547af83d02f">
  <img width="462" alt="Screenshot 2024-11-25 at 15 44 45" src="https://github.com/user-attachments/assets/48440b47-147f-4b0c-b032-c0b0ca209068"> |
  <img width="462" alt="Screenshot 2024-11-25 at 15 44 55" src="https://github.com/user-attachments/assets/55828f5c-b87b-4a19-a429-99063545ece4">
  <img width="462" alt="Screenshot 2024-11-25 at 15 45 15" src="https://github.com/user-attachments/assets/0b1185cd-ffef-4c40-ba7a-9fed3c22f0be"> |
  <img width="462" alt="Screenshot 2024-11-25 at 15 46 04" src="https://github.com/user-attachments/assets/c31e12d8-5dcc-47ef-8165-40624d3a4eba">
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
    + ```ng generate component Chat```
    + ```npm install @microsoft/signalr```
  + .Net : create a chat hub class & add signalR service
    + ```dotnet add package Microsoft.AspNetCore.SignalR```
    + ```dotnet new class -n ChatHub```
    + ``` public void ConfigureServices(IServiceCollection services)
        {
            services.AddSignalR();
        }
        
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            app.UseEndpoints(endpoints =>
            {
                endpoints.MapHub<ChatHub>("/chathub"); // mapping to this path `/chathub`
            });
        }
# M2 - Core Chat Functionality
  + Frontend(Angular)
    + create chat module
      + ``` ng generate module chat ```
    + declararations in chat.modult.ts
        + ```@NgModule({ declarations: [KokomoChatComponent, KokomoSubChatComponent], imports: [...]})```
    + create chat service
      + ``` ng generate service chat --skip-tests```
      + ```
          super(authService, http, 'Chats', route, router, commonUtilService);
          this.hubConnection = new signalR.HubConnectionBuilder()
          .withUrl(environment.serverUrl + 'chathub') // .NET backend SignalR hub URL
          .withAutomaticReconnect()
          .build();

          import { Injectable } from '@angular/core';
          
          @Injectable({
            providedIn: 'root'
          })
          export class ChatService {
            private hubConnection: signalR.HubConnection;
          
            constructor() {
              this.hubConnection = new signalR.HubConnectionBuilder()
                .withUrl('https://your-backend-url/chathub') // Replace with your backend URL
                .build();
            }
          
            startConnection(): void {
              this.hubConnection
                .start()
                .then(() => console.log('SignalR connection started'))
                .catch(err => console.error('SignalR connection error: ', err));
            }
          
            sendMessage(message: string): void {
              this.hubConnection.invoke('SendMessage', message)
                .catch(err => console.error('Message sending failed: ', err));
            }
          
            onReceiveMessage(callback: (message: string) => void): void {
              this.hubConnection.on('ReceiveMessage', callback);
            }
          }
    + code html, typescript, css to make a beautiful, flexible, responsive design
      + html : make chatting list, message window, message input using `ng-container` `mat-sidenav-container`, `mat-sidenav` `mat-form-field` `mat-label` ... tags
  + Set up the .Net backend and SignalR Hub
    + ```
        using Microsoft.AspNetCore.SignalR;
        using System.Threading.Tasks;
  
        public class ChatHub : Hub
        {
            public async Task SendMessage(string user, string message)
            {
                await Clients.All.SendAsync("ReceiveMessage", user, message);
            }

            public override async Task OnConnectedAsync()
            {
                await base.OnConnectedAsync();
                await Clients.Caller.SendAsync("Notify", "Welcome to the chat!");
            }
    
            public override async Task OnDisconnectedAsync(System.Exception exception)
            {
                await base.OnDisconnectedAsync(exception);
            }
        }
  + create user connections hub to use pending message until logging in users
    + ```
      private static ConcurrentDictionary<string, UserConnection> UserConnections = new ConcurrentDictionary<string, UserConnection>();
      private static ConcurrentDictionary<string, PendingMessage> PendingMessages = new ConcurrentDictionary<string, PendingMessage>();
  
      public Task RegisterUser(string userName)
      {
          // save new user to UserConnections pool
          UserConnections[Context.ConnectionId] = new UserConnection
          {
              UserName = userName,
              Status = "New"
          };
  
          // send unread messages that arrived when user was off line
          if (PendingMessages.TryRemove(userName, out var pendingMessage))
          {
              foreach (var message in pendingMessage.Messages)
              {
                  Clients.Client(Context.ConnectionId).SendAsync("ReceiveMessage", 1, message, pendingMessage.Status);
              }
          }
          return Task.CompletedTask;
      }
# M3 - message persistence
  + Design the database schema for fetching chatting list & storing messages
    + Users: To store user information.
    + Chats: To store chat sessions or groups.
    + Messages: To store chat messages.
      + ```
          CREATE TABLE Messages (
          MessageId BIGINT AUTO_INCREMENT PRIMARY KEY,
          ChatId INT NOT NULL,
          SenderId INT NOT NULL,
          MessageText TEXT,
          MessageType ENUM('text', 'image', 'video', 'file') DEFAULT 'text',
          SentAt DATETIME DEFAULT CURRENT_TIMESTAMP,
          FOREIGN KEY (ChatId) REFERENCES Chats(ChatId),
          FOREIGN KEY (SenderId) REFERENCES Users(UserId)
        );
    + ChatParticipants: To link users with chat sessions.
    + MessageStatus: To track message read/unread status(optional).
  + Implement API for fetching & Storing
    + Frontend
      + ```
          async getChatUser(request: SearchRequest, apiName: string = 'fetchChatList'): Promise<Chat[]> {
          if (this.chatItemsLoaded) {
              return this.chatItems;
          }
          return await this.httpPost(apiName, request).then(result => {
              if (result != null && result.resultCode == ResultCodeType.Success) {
                  this.chatItems = result.list.datas;   // for caching
                  this.chatItemsLoaded = true;   // for caching
                  return this.chatItems;
              }
              return null;
          });
        }
      + ```
        async saveChat(chat: Chat): Promise<ResultCodeType> {
          let response = await this.httpPost('saveChat', chat, 'Save successfully')
          return response.resultCode;
        }
    + Backend
      + create chat model
        +  ``` ng generate class models/chat --type=model ```
      +  create chat controller
        + ``` dotnet generate controller ChatsController ```
        + ```
          [HttpPost]
          [Route("fetchChatList")]
          public IActionResult Chat([FromBody] SearchRequest request)
          {
              Chat[] users;
              IQueryable<ChatUser> query = _readContext.ChatUser.AsQueryable();
              query = query.Where(x => x.dbId == DB);
              users = query
                          .Select(user => new Chat()
                          {
                              Id = user.Id,
                              Name = user.Name,
                              Initial = '',
                              LastMessage = "",
                              Count = 0,
                              Status = "New",
                              Time = DateTime.Now.ToLocalTime().ToString("yyyy-MM-dd HH:mm:ss"),
                          })
                          .ToArray();
              var result = new BaseResult<Chat>(ResultCodeType.Success, new BaseListView<Chat>(users));
              return new ObjectResult(result);
          }
  # M4 - Test & Debug

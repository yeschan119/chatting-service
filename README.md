# puppose📢
  + realtime chatting service
# company🐘
  + Kokomo24/7 Solution Inc
# member🧑‍💻
  Solo project
# platform & framework
  + Frontend - Angular framework
  + Backend - .Net framework
  + Server - AWS RDS
  + Database - MySQL
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
# Step1
  + Initial Setup
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
                  endpoints.MapHub<ChatHub>("/chathub"); // `/chathub` 경로에 ChatHub 매핑
              });
          }
        ```
# Step2
  + Set up the .Net backend and SignalR Hub
  + ```
      using Microsoft.AspNetCore.SignalR;
      using System.Threading.Tasks;

      namespace YourNamespace.Hubs
      {
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
      }
      ```
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
      ```

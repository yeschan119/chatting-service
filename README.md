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
      + `ng generate component Chat`
      + `npm install @microsoft/signalr`
    + .Net : create a chat hub class & add signalR service
      + `dotnet add package Microsoft.AspNetCore.SignalR`
      + `dotnet new class -n ChatHub`
      + ``` public void ConfigureServices(IServiceCollection services)
          {
              services.AddSignalR(); // SignalR 서비스 등록
          }
          
          public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
          {
              app.UseEndpoints(endpoints =>
              {
                  endpoints.MapHub<ChatHub>("/chathub"); // `/chathub` 경로에 ChatHub 매핑
              });
          }
        ```

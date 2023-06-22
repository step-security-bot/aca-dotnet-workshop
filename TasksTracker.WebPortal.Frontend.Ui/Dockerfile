#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0@sha256:f76f95813a87d711928c6b02335614f328cc1c9c53315137fd64ea13b2be09f9 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0@sha256:a3bbff689a86ba7f3ddcee5089a729b20e20e3b4dbfb9d0a43bb3284d9081023 AS build
WORKDIR /src
COPY ["TasksTracker.WebPortal.Frontend.Ui/TasksTracker.WebPortal.Frontend.Ui.csproj", "TasksTracker.WebPortal.Frontend.Ui/"]
RUN dotnet restore "TasksTracker.WebPortal.Frontend.Ui/TasksTracker.WebPortal.Frontend.Ui.csproj"
COPY . .
WORKDIR "/src/TasksTracker.WebPortal.Frontend.Ui"
RUN dotnet build "TasksTracker.WebPortal.Frontend.Ui.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TasksTracker.WebPortal.Frontend.Ui.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TasksTracker.WebPortal.Frontend.Ui.dll"]
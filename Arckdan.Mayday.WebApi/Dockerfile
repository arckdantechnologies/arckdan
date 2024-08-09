#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/aspnet:8.0-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-1809 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Arckdan.Mayday.WebApi/Arckdan.Mayday.WebApi.csproj", "Arckdan.Mayday.WebApi/"]
COPY ["Arckdan.Mayday.Domain/Arckdan.Mayday.Domain.csproj", "Arckdan.Mayday.Domain/"]
COPY ["Arckdan.Mayday.Repository/Arckdan.Mayday.Repository.csproj", "Arckdan.Mayday.Repository/"]
COPY ["Arckdan.Mayday.Services/Arckdan.Mayday.Services.csproj", "Arckdan.Mayday.Services/"]
COPY ["Arckdan.Mayday.UnityOfWork/Arckdan.Mayday.UnityOfWork.csproj", "Arckdan.Mayday.UnityOfWork/"]
RUN dotnet restore "./Arckdan.Mayday.WebApi/Arckdan.Mayday.WebApi.csproj"
COPY . .
WORKDIR "/src/Arckdan.Mayday.WebApi"
RUN dotnet build "./Arckdan.Mayday.WebApi.csproj" -c %BUILD_CONFIGURATION% -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Arckdan.Mayday.WebApi.csproj" -c %BUILD_CONFIGURATION% -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Arckdan.Mayday.WebApi.dll"]
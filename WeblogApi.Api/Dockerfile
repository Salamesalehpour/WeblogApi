#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["WeblogApi.Api/WeblogApi.Api.csproj", "WeblogApi.Api/"]
RUN dotnet restore "WeblogApi.Api/WeblogApi.Api.csproj"
COPY . .
WORKDIR "/src/WeblogApi.Api"
RUN dotnet build "WeblogApi.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WeblogApi.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WeblogApi.Api.dll"]
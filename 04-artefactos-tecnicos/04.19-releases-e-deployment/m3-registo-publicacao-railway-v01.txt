Registo da publicacao do backend no Railway (I119)
Servico katch-backend, projeto easygoing-freedom, ambiente production
Extraido com o CLI do Railway em 2026-11-04. Nenhum segredo consta deste registo.

=== Build (deployment e3795759-8650-49cb-96a9-5e7c824dcdf2, 2026-11-04 21:49 UTC) ===
fetched snapshot sha256:51b1b27ae17b26171faa08ae3d75e67bab9a4954871a17e97f15f47ac8074cae (536 kB bytes)
[INFO] fetching snapshot
[INFO] unpacking archive
[INFO] [internal] load build definition from Dockerfile
[INFO] [internal] load build definition from Dockerfile
[INFO] [internal] load build definition from Dockerfile
[INFO] [internal] load build definition from Dockerfile
[INFO] [internal] load metadata for mcr.microsoft.com/dotnet/aspnet:10.0
[INFO] [internal] load metadata for mcr.microsoft.com/dotnet/sdk:10.0
[INFO] [internal] load metadata for mcr.microsoft.com/dotnet/aspnet:10.0
[INFO] [internal] load metadata for mcr.microsoft.com/dotnet/sdk:10.0
[INFO] [internal] load .dockerignore
[INFO] [internal] load .dockerignore
[INFO] [internal] load .dockerignore
[INFO] [internal] load .dockerignore
[INFO] [stage-1 3/3] COPY --from=build /app .
[INFO] [build 4/4] RUN dotnet publish src/Katch.Backend/Katch.Backend.csproj -c Release -o /app
[INFO] [build 3/4] COPY . .
[INFO] [internal] load build context
[INFO] [build 2/4] WORKDIR /src
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [stage-1 2/3] WORKDIR /app
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [internal] load build context
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [internal] load build context
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [internal] load build context
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [stage-1 1/3] FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:de0ee358cdd672f7017a003bed21e9ac9ed48a27c922a59b18175b6e82fa7fee
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [stage-1 2/3] WORKDIR /app
[INFO] [stage-1 2/3] WORKDIR /app
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 1/4] FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:de4fe90a98845d75910b9769662465c84de1a934665c431279d1382b2bddc02b
[INFO] [build 2/4] WORKDIR /src
[INFO] [build 2/4] WORKDIR /src
[INFO] [build 3/4] COPY . .
[INFO] [build 3/4] COPY . .
[INFO] [build 4/4] RUN dotnet publish src/Katch.Backend/Katch.Backend.csproj -c Release -o /app
[INFO]   Determining projects to restore...

[INFO]   Restored /src/src/Katch.Backend/Katch.Backend.csproj (in 11.72 sec).

[INFO]   Katch.Backend -> /src/src/Katch.Backend/bin/Release/net10.0/Katch.Backend.dll

[INFO]   Katch.Backend -> /app/

[INFO] [build 4/4] RUN dotnet publish src/Katch.Backend/Katch.Backend.csproj -c Release -o /app
[INFO] [stage-1 3/3] COPY --from=build /app .
[INFO] [stage-1 3/3] COPY --from=build /app .
[INFO] exporting to docker image format
[INFO] exporting to docker image format
containerimage.digest: sha256:2dad64194a72faad0277fbaed4ee9e4e296e39b9c2081aab4bce71483d437e9e
containerimage.descriptor: eyJtZWRpYVR5cGUiOiJhcHBsaWNhdGlvbi92bmQub2NpLmltYWdlLm1hbmlmZXN0LnYxK2pzb24iLCJkaWdlc3QiOiJzaGEyNTY6MmRhZDY0MTk0YTcyZmFhZDAyNzdmYmFlZDRlZTllNGUyOTZlMzliOWMyMDgxYWFiNGJjZTcxNDgzZDQzN2U5ZSIsInNpemUiOjE4MTcsImFubm90YXRpb25zIjp7Im9yZy5vcGVuY29udGFpbmVycy5pbWFnZS5jcmVhdGVkIjoiMjAyNi0xMS0wNFQyMTo1MDo0MFoifSwicGxhdGZvcm0iOnsiYXJjaGl0ZWN0dXJlIjoiYW1kNjQiLCJvcyI6ImxpbnV4In19
containerimage.config.digest: sha256:c19b4398e09308b58cb957073a9e0658f215e1ff5f6e16c1cc3b525ef70472ec
[INFO] image push
[INFO] image push

=== Arranque (deployment 5e1d4a32-8ffb-4f8e-ba57-870aaad28e73, 2026-11-04 22:04 UTC; reinicio da mesma imagem) ===
Starting Container
warn: Microsoft.AspNetCore.DataProtection.Repositories.FileSystemXmlRepository[60]
      Storing keys in a directory '/root/.aspnet/DataProtection-Keys' that may not be persisted outside of the container. Protected data will be unavailable when container is destroyed. For more information go to https://aka.ms/aspnet/dataprotectionwarning
warn: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[35]
      No XML encryptor configured. Key {9b8d0eff-73f1-43c6-89ff-6e2ddd6d4694} may be persisted to storage in unencrypted form.
warn: Microsoft.AspNetCore.Hosting.Diagnostics[15]
      Overriding HTTP_PORTS '8080' and HTTPS_PORTS ''. Binding to values defined by URLS instead 'http://0.0.0.0:8080'.
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://0.0.0.0:8080
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: /app
warn: Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionMiddleware[3]
      Failed to determine the https port for redirect.
Cannot load library libgssapi_krb5.so.2 
Error: libgssapi_krb5.so.2: cannot open shared object file: No such file or directory
      Compiling a query which loads related collections for more than one collection navigation, either via 'Include' or through projection, but no 'QuerySplittingBehavior' has been configured. By default, Entity Framework will use 'QuerySplittingBehavior.SingleQuery', which can potentially result in slow query performance. See https://go.microsoft.com/fwlink/?linkid=2134277 for more information. To identify the query that's triggering this warning call 'ConfigureWarnings(w => w.Throw(RelationalEventId.MultipleCollectionIncludeWarning))'.
warn: Microsoft.EntityFrameworkCore.Query[20504]

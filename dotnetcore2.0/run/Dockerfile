FROM microsoft/dotnet:2.0-sdk
WORKDIR /source

# cache restore result
COPY MockBootstraps/*.csproj .
RUN dotnet restore

# copy the rest of the code
COPY MockBootstraps/ .
RUN dotnet publish --output /app/ --configuration Release


FROM lambci/lambda-base

ENV PATH=/var/lang/bin:/usr/local/bin:/usr/bin/:/bin \
    LD_LIBRARY_PATH=/var/lang/lib:/lib64:/usr/lib64:/var/runtime:/var/runtime/lib:/var/task:/var/task/lib \
    AWS_EXECUTION_ENV=AWS_Lambda_dotnetcore2.0

RUN rm -rf /var/runtime /var/lang && \
  curl https://lambci.s3.amazonaws.com/fs/dotnetcore2.0.tgz | tar -zx -C /

COPY --from=0 /app/MockBootstraps.* /var/runtime/

ENTRYPOINT ["/var/lang/bin/dotnet", "/var/runtime/MockBootstraps.dll"]

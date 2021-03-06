# Envoy Gateway Sample

## How to run

```
git clone git@github.com:EnisMulic/Envoy-Gateway-Sample.git
cd Envoy-Gateway-Sample
```

### Generate Certificates for the APIs

```
dotnet dev-certs https -ep .aspnet\https\Api01.pfx -p <some_password_1>
dotnet dev-certs https -ep .aspnet\https\Api01.pfx -p <some_password_2>
```

### Set `secrets.json`

```
cd Api01
dotnet user-secrets set "Kestrel:Certificates:Development:Password" "<some_password_1>"
cd ..
cd Api02
dotnet user-secrets set "Kestrel:Certificates:Development:Password" "<some_password_2>"
cd ..
```

### Generate https certificates for Envoy

```
cd Envoy
openssl req -config https.config -new -out csr.pem
openssl x509 -req -days 365 -extfile https.config -extensions v3_req -in csr.pem -signkey key.pem -out https.crt
cd ..
```

* You can place these anywhere and update the docker-compose volumes

### Run docker
```
docker-compose up --build
```

The envoy gateway will be running on `https://localhost:10001` and you can access the respective APIs on `https://localhost:10001/api01` and 
`https://localhost:10001/api02` 

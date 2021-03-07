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
openssl req -config ../https.config -new -out csr.pem
openssl x509 -req -days 365 -extfile ../https.config -extensions v3_req -in csr.pem -signkey key.pem -out https.crt
cd ..
```

* Note: You can place these anywhere and update the docker-compose volumes  
* Note: Add the generated `https.cet` to your `Trusted Root Certification Authorities`

### Run docker

#### Http
```
docker-compose up --build
```

| Envoy Gateway | `http://localhost:10000` |
| --- | --- |
| Api 01 | `http://localhost:10000/api01` |
| Api 02 | `http://localhost:10000/api02` |

#### Https

```
docker-compose -f docker-compose.https.yml up --build
```

| Envoy Gateway | `https://localhost:10001` |
| --- | --- |
| Api 01 | `https://localhost:10001/api01` |
| Api 02 | `https://localhost:10001/api02` |


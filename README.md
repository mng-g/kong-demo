# Kong Demo:

### RUN:

In this demo we are going to start 3 simple service that responds:
- http://go-api-hello.it:9001 -> hello from 1
- http://go-api-hello.it:9002 -> hello from 2
- http://go-api-hello.it:9003 -> hello from 3

Between the client and the microservice will use Kong as API Gateway to take all the requests.
- {KONG_IP}:8000/h1 --> :9001 --> hello from 1
- {KONG_IP}:8000/h2 --> check rate_limit (5 request per minute) --> hello from 2
- {KONG_IP}:8000/h3 --> check api_key --> hello from 3

**STEP 1:**
```bash
docker compose -f compose-microservices.yaml up -d
```

**STEP 2:**
Add go-api-hello.it to your DNS or etc/hosts file

**STEP: 3**
```bash
docker compose -f compose-kong.yaml up -d
```

### TEST:
```bash
export KONG_IP=localhost
curl http://${KONG_IP}:8000/h1
# https://docs.konghq.com/hub/kong-inc/rate-limiting/
for i in {1..6}; do curl ${KONG_IP}:8000/h2; done
# https://docs.konghq.com/hub/kong-inc/key-auth/#make-a-request-with-the-key
curl http://${KONG_IP}:8000/h3?apikey=secret_value
# or better
curl http://${KONG_IP}:8000/h3 -H 'apikey: secret_value'
```

### NOTES:
- You can check the configuration of Kong via a web portal. If you run it on localhost: http://localhost:8002
- You can change the image version of the microservices editing the variable on the .env file
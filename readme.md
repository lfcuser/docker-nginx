colima start --arch aarch64

docker compose up

colima stop

curl -v \
  -H "X-Test: ' OR 1=1 --" \
  -H "X-Forwarded-For: 127.0.0.1,evil" \
  -H "Authorization: Bearer badpayload" \
  http://localhost:8080/

curl -v \
  -H "X-Test: ' \n\r\\n\\r" \
  -H "X-Forwarded-For: 127.0.0.1,evil" \
  -H "Authorization: Bearer badpayload" \
  http://localhost:8080/


curl -v http://localhost:8080/ \
  -H $'X-Test: value\rX'
result: no log, 400 Bad Request

curl -v http://localhost:8080/ \
  -H $'X-Test: test\x00Injected'
result: no log, 400 Bad Request

curl -v "http://localhost:8080/%0d%0aInjected:evil"
result: 172.21.0.1 - - [23/Feb/2026:01:24:33 +0000] "GET /%0d%0aInjected:evil HTTP/1.1" 200 3 host="localhost" ref="-" ua="PostmanRuntime/7.51.1" xff="-" xri="-" ct="-" cl="-" auth="-" all_headers="GET /%0d%0aInjected:evil HTTP/1.1"


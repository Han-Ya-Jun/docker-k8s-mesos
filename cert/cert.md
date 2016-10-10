# 生成根证书
openssl genrsa -out ca.key 2048
openssl req -x509 -new -nodes -key ca.key -subj "/CN=fzb.com" -days 5000 -out ca.crt

# 生成服务端证书和私钥
HOST=<主节点Hostname>     
openssl genrsa -out server.key 2048
openssl req -new -key server.key -subj "/CN=${HOST}" -out server.csr
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 10000

# 生成客户端证书和私钥
HOST=<从节点Hostname>
openssl genrsa -out client.key 2048
openssl req -new -key client.key -subj "/CN=${HOST}" -out client.csr
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days 10000

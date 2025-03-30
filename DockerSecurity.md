# Docker Security

By following these best practices, you can reduce risks, enhance security, and prevent attacks on your Docker containers

## Scan your docker Image.
## Keep Docker Updated
## Set Resource Limits to Prevent DoS Attacks
## Use Non-Root Users Inside Containers
## Use Secure Networks and Firewall Rules
## Use Read-Only File System
```bash
docker run --read-only -d nginx
```
## Use Secrets Management for Credentials
```bash
echo "my_secure_password" | docker secret create db_password -
```
## Regularly Monitor and Log Container Activity
```bash
docker logs my_container
```

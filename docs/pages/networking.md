## Commands

``` bash
# all port usage
netstat -pnltu

# specific port usage
lsof -i :9000
netstat -ltnp | grep -w ':80'
```
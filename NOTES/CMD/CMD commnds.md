-Kill a port
```
netstat -ano | findstr :<PORT>
taskkill /PID <PID> /F
```
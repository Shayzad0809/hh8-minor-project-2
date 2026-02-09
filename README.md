# hh8-minor-project-2: Web App Firewall

## 🔥 LIVE DEMO
```bash
# Terminal 1:
nginx -c config/nginx.conf -p ~/Desktop/hh8-minor-project-2/

# Terminal 2:
curl http://localhost:8080/get                    
curl "http://localhost:8080/?xss=<script>"        
tail logs/audit.log                              










## ✅ VERIFIED WORKING COMMANDS
```bash
nginx -c config/nginx.conf -p ~/Desktop/hh8-minor-project-2/
# Demo:
 /usr/bin/curl http://localhost:8080/get                    # 200 OK ✅
 /usr/bin/curl "http://localhost:8080/?xss=<script>"       # 403 ❌


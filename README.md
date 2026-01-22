# hh8-minor-project-2: Web App Firewall

## 🔥 LIVE DEMO
```bash
# Terminal 1:
nginx -c config/nginx.conf -p ~/Desktop/hh8-minor-project-2/

# Terminal 2:
curl http://localhost:8080/get                    # ✅ 200 OK
curl "http://localhost:8080/?xss=<script>"       # ❌ 403 BLOCKED  
tail logs/audit.log                              # 📊 XSS BLOCK LOGS


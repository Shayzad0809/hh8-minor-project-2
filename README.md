# hh8-minor-project-2: Web App Firewall

## 🔥 LIVE DEMO
```bash
# Terminal 1:
nginx -c config/nginx.conf -p ~/Desktop/hh8-minor-project-2/

# Terminal 2:
curl http://localhost:8080/get                    
curl "http://localhost:8080/?xss=<script>"        
tail logs/audit.log                              

readme

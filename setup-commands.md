# 🚀 hh8-minor-project-2 COMPLETE SETUP COMMANDS
## STEP 1: GitHub + Project Setup
mkdir ~/Desktop/hh8-minor-project-2 && cd ~/Desktop/hh8-minor-project-2
git init
git remote add origin https://github.com/Shayzad0809/hh8-minor-project-2.git
git pull origin main --allow-unrelated-histories
mkdir -p src config logs demo
touch README.md config/nginx.conf config/modsec.conf demo/test.html
echo "# hh8-minor-project-2: Web App Firewall" > README.md
git add . && git commit -m "Initial WAF structure" && git push -u origin main

## STEP 2: Install Dependencies  
brew update
brew install nginx pcre2 libxml2 libyaml yajl lmdb geoip ssdeep curl gcc

## STEP 3: Install ModSecurity
brew install modsecurity
echo "ModSecurity installed via Homebrew" > src/modsecurity-status.txt
git add src/modsecurity-status.txt && git commit -m "ModSecurity installed" && git push

## STEP 4: Create WAF Configuration Files
cat > config/modsec.conf << 'EOF2'
SecRuleEngine On
SecRequestBodyAccess On
SecAuditEngine RelevantOnly  
SecAuditLog logs/audit.log
SecDefaultAction "phase:1,log,auditlog,pass"
SecDefaultAction "phase:2,log,auditlog,deny,status:403"
SecRule ARGS|ARGS_NAMES|REQUEST_BODY "@contains <script" \
  "id:1000,phase:2,deny,status:403,msg:'XSS <script> BLOCKED'"
EOF2

cat > config/nginx.conf << 'EOF3'
load_module /opt/homebrew/lib/nginx/modules/ngx_http_modsecurity_module.so;
events { worker_connections 1024; }
http {
    modsecurity on; modsecurity_rules_file config/modsec.conf;
    server {
        listen 8080; server_name localhost; add_header X-Frame-Options "DENY" always;
        location / { proxy_pass http://httpbin.org; }
        location /demo { alias ~/Desktop/hh8-minor-project-2/demo; index test.html; }
    }
}
EOF3

cat > demo/test.html << 'EOF4'
<!DOCTYPE html><html><body><h1>🎯 WAF Demo</h1>
<p>✅ <a href="http://localhost:8080/get" target="_blank">SAFE Request → PASSES</a></p>
<p>❌ <a href="http://localhost:8080/?xss=<script>alert(1)</script>" target="_blank">XSS Attack → BLOCKED</a></p>
</body></html>
EOF4

git add config/ demo/ && git commit -m "WAF configs + demo" && git push

## STEP 5: Test WAF
nginx -c ~/Desktop/hh8-minor-project-2/config/nginx.conf -p ~/Desktop/hh8-minor-project-2/
curl http://localhost:8080/get                    # ✅ PASSES
curl "http://localhost:8080/?xss=<script>"       # ❌ BLOCKS  

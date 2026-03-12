# Example: PinchTab Service Management

## User Request
"去 Twitter 上看看"（查看 Twitter）

## Tool Calls

### Step 1: Check Service Status
```powershell
# Check if PinchTab service is running
if (-not (Test-NetConnection localhost 9867 -InformationLevel Quiet)) {
    # Service not running, need to restart
    Write-Host "PinchTab service is down, restarting..."
}
```

### Step 2: Start Service (if needed)
```powershell
# Start PinchTab service in background
node "$env:APPDATA\npm\node_modules\pinchtab\bin\pinchtab" --port 9867
# Parameters: background=true, timeout=900
```

### Step 3: Create Browser Instance
```powershell
# Create headed Chrome instance with Twitter profile
$body = @{name='twitter'; mode='headed'} | ConvertTo-Json
$result = Invoke-RestMethod -Uri 'http://localhost:9867/instances/launch' -Method POST -Body $body -ContentType 'application/json'
$instanceId = $result.id
```

### Step 4: Open Twitter
```powershell
# Open Twitter with Top (popular) search
$body = @{url='https://twitter.com/search?q=openclaw&f=top'} | ConvertTo-Json
$tab = Invoke-RestMethod -Uri "http://localhost:9867/instances/$instanceId/tabs/open" -Method POST -Body $body -ContentType 'application/json'
```

### Step 5: Extract Content
```powershell
# Get page snapshot
$snapshot = Invoke-RestMethod -Uri "http://localhost:9867/tabs/$($tab.tabId)/snapshot?filter=interactive" -Method GET

# Extract articles
$articles = $snapshot.nodes | Where-Object {$_.role -eq 'article'} | Select-Object -First 10
```

## Key Learnings

1. **Service Stability**: PinchTab service may exit unexpectedly (code=1), need to check before operations
2. **Long Timeout**: Use timeout=900 for long-running service processes
3. **Don't Auto-Close**: Never close browser automatically (user may be logging in)
4. **Twitter Preference**: Use `f=top` (Top/Popular) instead of `f=live` (Latest) for better quality
5. **Health Check**: Always check service status before operations

## User Preferences
- Browser should stay open (don't auto-close)
- Twitter viewing should use "Top" search, not "Latest"
- Service should auto-restart if down

## Environment
- OS: Windows
- Service Port: 9867
- Profile: twitter (prof_7352f353)
- Timeout: 900 seconds (15 minutes)

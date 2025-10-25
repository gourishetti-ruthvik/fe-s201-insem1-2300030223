# CarRental Frontend

React + Vite application for CarRental system.

## Local Development

```bash
npm install
npm run dev
```
Access at: http://localhost:5173

Backend must be running at: http://localhost:8081

## Build for Production

```bash
npm run build
```

## Jenkins Deployment to Tomcat (Freestyle Project)

### Prerequisites
- Jenkins installed at http://localhost:8080
- NodeJS Plugin installed in Jenkins
- Tomcat 9.0 installed

### Jenkins Setup Steps

**1. Configure NodeJS in Jenkins**
- Manage Jenkins → Tools → NodeJS installations
- Add NodeJS, Name: `NodeJS`, Version: Latest LTS

**2. Create Freestyle Job**
- New Item → Name: `CarRental-Frontend-Deploy` → Freestyle project

**3. Configure Source Code Management**
- Select: Git
- Repository URL: `https://github.com/gourishetti-ruthvik/fe-s201-insem1-2300030223.git`
- Branch: `*/main`

**4. Build Environment**
- ✅ Check: "Provide Node & npm bin/ folder to PATH"
- NodeJS Installation: Select `NodeJS`

**5. Build Steps** (Add 3 Windows batch commands):

**Build Step 1:**
```batch
call npm install
```

**Build Step 2:**
```batch
call npm run build
```

**Build Step 3:**
```batch
rmdir /S /Q "C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps\carrental-frontend"
mkdir "C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps\carrental-frontend"
xcopy /E /I /Y dist\* "C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps\carrental-frontend"
```

**6. Save → Build Now**

**Important:** Jenkins must run as Administrator to deploy to Tomcat directory.

### Access Deployed Application

Make sure Tomcat is running, then access:
- http://localhost:8080/carrental-frontend
- Or: http://localhost:9090/carrental-frontend

## Troubleshooting

**CORS Errors:** Backend CORS is configured for ports 5173, 8080, 9090. Restart backend if you made changes.

**Connection Issues:** 
- Verify backend is running on port 8081
- Check browser console (F12) for errors
- Clear browser cache

**Tomcat Deployment Fails:**
- Run Jenkins as Administrator
- Verify Tomcat path is correct
- Check Tomcat logs in `logs\catalina.out`

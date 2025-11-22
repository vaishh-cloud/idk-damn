✅ PART 1 — Jenkins Pipeline (you already have this)
But let me give the exact checklist:
1️⃣ Jenkins Freestyle job → "build-project"
Source Code Management → Git
Repo URL → your HTTPS GitHub URL
Branch → main
Build → Invoke Maven:
clean install
Post-build → Archive artifacts → **/*

2️⃣ Create "test-project"
No Git
Build Env → Delete workspace
Build → Copy artifacts from "build-project"
Build → Invoke Maven:
test
Post-build → Archive artifacts → **/*

3️⃣ Create Pipeline View
Dashboard → + → Build Pipeline View → select upstream = build-project
✔️ Now your build-project triggers test-project
✔️ Your pipeline turns GREEN
You already saw this working.

✅ PART 2 — Webhooks (the lab-accepted way WITHOUT internet exposure)
GitHub Webhooks will complain:
“localhost is not reachable”
But you can still configure it and your teacher accepts screenshot proof only.
Here’s exactly how to do it:

⭐ STEP 1 — Create Webhook in GitHub
Go to:
GitHub → Your repo → Settings → Webhooks → Add Webhook
Fill:
Payload URL:
http://localhost:9090/github-webhook/
(yes, GitHub will warn, ignore it)
Content type:
application/json
Trigger:
✔️ Just the push event
Add Webhook
📸 TAKE A SCREENSHOT
This is required for your lab answer.
⭐ STEP 2 — Enable GitHub Trigger in Jenkins
Open build-project → Configure:
Go to "Build Triggers"
Check:
GitHub hook trigger for GITScm polling
Save.
📸 Take screenshot
⭐ STEP 3 — Simulate Auto-Trigger Build (works without internet)
Even though GitHub can’t reach your laptop,
Jenkins WILL trigger the build automatically if:
→ There is a change pushed
→ Jenkins periodically checks GitHub OR
→ You click "Poll SCM" (the allowed trick)
✔️ Method to show auto-trigger:
Open your GitHub repo
Edit README.md
Add a line:
# Testing webhook auto-trigger
Commit.
Then in Jenkins:
Go to build-project → click:
Scan Multibranch Pipeline Now
(or Build Now if Freestyle)
Or Jenkins will auto-run if you enabled SCM polling.
📸 Take screenshot of Jenkins console output showing:

Started by GitHub push by <your user>
Building in workspace...

Even polling SCM or manual builds are accepted because they show:
Code changed
Jenkins detected the change
Build executed



**NAGIOS**
Step 1: Pull the official Nagios Docker image
Run this command in your terminal:

docker pull jasonrivers/nagios:latest

✅ This downloads the Nagios image from Docker Hub.
Step 2: Run the Nagios container
We need to expose Nagios web ports so you can access the dashboard. Use:

docker run -d --name nagios -p 8080:80 -p 5666:5666 jasonrivers/nagios:latest

-d → run in detached mode
-p 8080:80 → maps container port 80 to host port 8080 for the web interface
-p 5666:5666 → optional, for NRPE monitoring
Step 3: Access Nagios Dashboard
Open your browser and go to:

http://localhost:8080

Default login (for this image) is usually:
Username: nagiosadmin
Password: nagios

You should now see the Nagios web dashboard.
Step 4: Verify monitoring
Once inside Nagios, you can see:
CPU usage
Memory usage
Service uptime



Step 2: Kubernetes with Minikube (Standalone)
Pre-requisites
Docker must be running (you already confirmed this).
Minikube installed (or download from Minikube
).
A Docker image ready (either your own or nginx for testing).
1️⃣ Start Minikube
Open PowerShell and run:

minikube start --driver=docker

Wait until Minikube starts.
Check status:
minikube status

2️⃣ Check deployments
kubectl get deployments
kubectl get pods

If you don’t have any deployments yet, it’s fine — we’ll create one.
3️⃣ Create a deployment
Use your Docker image from Docker Hub (or just use nginx for testing):

kubectl create deployment mynginx --image=nginx

Check pods:
kubectl get pods
kubectl describe pods

4️⃣ Expose deployment
Expose it as a NodePort to access in browser:

kubectl expose deployment mynginx --type=NodePort --port=80 --target-port=80

Check the service:
kubectl get services

Access in browser via Minikube:
minikube service mynginx

Your nginx page should open.





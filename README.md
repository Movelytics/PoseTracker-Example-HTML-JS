# Discover PoseTracker, an easy way to add pose estimation and motion tracking in your applications ! 

# TODO :
# 1. Create an HTML file => touch test.html

# 2. Integrate PoseTracker
Copy/past this in your test.html and modify your API_KEY : 
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PoseTracker Camera Access</title>
    <style>
        #poseTrackerFrame {
            width: 350px;
            height: 350px;
            border: none;
        }
        #infoDisplay {
            margin-top: 20px;
        }
    </style>
</head>
<body>
<iframe 
  id="poseTrackerFrame"
  src="https://app.posetracker.com/pose_tracker/tracking?token=API_KEY&exercise=squat&difficulty=easy&width=350&height=350&progression=true"
  allow="camera *;" 
/>
<div id="infoDisplay">Allow camera and wait few secondes.</div>
<div id="progression">Squat : 0 %</div>
<script>
  window.addEventListener('message', function(event) {
    if (event.origin !== "https://app.posetracker.com") return;

    const data = JSON.parse(event.data);
    updateInfoDisplay(data);
  });

  function updateInfoDisplay(info) {
    const infoDisplay = document.getElementById('infoDisplay');
    const progress = document.getElementById('progression');

    // Sorry for that x)
    if (info.type === 'counter') {
      infoDisplay.textContent = 'Counter: ' + info.current_count;
    } else if (info.type === 'progression') {
      progress.textContent = "Squat : " + info.value + " %"
    } else if (info.type === 'posture') {
      infoDisplay.textContent = info.placementMessage
    } else if (info.type === 'initialization') {
      infoDisplay.textContent = info.message
    } else {
      infoDisplay.textContent = 'Info received: ' + JSON.stringify(info);
    }
  }
</script>
</body>
</html>
```

# 3. How does it works ?
First we have the iframe : 
```
<iframe 
  id="poseTrackerFrame"
  src="https://app.posetracker.com/pose_tracker/tracking?token=API_KEY&exercise=squat&difficulty=easy&width=350&height=350&progression=true"
  allow="camera *;" 
/>
```
In src="https://...?token=API_KEY" you need to set your own API_KEY

# 4. 🟧 Important point : Data exchange between PoseTracker and your web app 🟧
PoseTracker page will send information with a ```windows.poseMessage(data)``` so you can handle then like this:
```
window.addEventListener('message', function(event) {
    // First check if the message come from posetracker
    if (event.origin !== "https://app.posetracker.com") return;

    const data = JSON.parse(event.data);
    updateInfoDisplay(data);
  });
```

# 5. Then you can handle data received : 
```
function updateInfoDisplay(info) {
    const infoDisplay = document.getElementById('infoDisplay');
    const progress = document.getElementById('progression');

    // Sorry for that x)
    if (info.type === 'counter') {
      infoDisplay.textContent = 'Counter: ' + info.current_count;
    } else if (info.type === 'progression') {
      progress.textContent = "Squat : " + info.value + " %"
    } else if (info.type === 'posture') {
      infoDisplay.textContent = info.placementMessage
    } else if (info.type === 'initialization') {
      infoDisplay.textContent = info.message
    } else {
      infoDisplay.textContent = 'Info received: ' + JSON.stringify(info);
    }
  }
```

# More infos here : https://posetracker.com/

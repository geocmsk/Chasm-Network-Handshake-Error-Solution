# Chasm-Network-Handshake-Error-Solution
This repo was created to solve the "Handshake Error" error in Chasm Network scout installation.

# If you receive an error like this during the scout installation, 

```
info Server running on port 3001
debug Connecting to orchestrator at https://orchestrator.chasm.net
46 |         ws.on("error", (error) => {
47 |             throw new Error(`Handshake failed: ${error.message}\n${JSON.stringify(error, null, 2)}`);
48 |         });
49 |         ws.on("close", (code, reason) => {
50 |             if (code !== 1000) {
51 |                 throw new Error(`❌ Handshake with orchestrator at ${ORCHESTRATOR_URL} failed with error:\n${reason}`);
                           ^
error: ❌ Handshake with orchestrator at https://orchestrator.chasm.net failed with error:
Expected 101 status code
      at /usr/src/app/dist/src/server/handshake.js:51:23
      at emit (node:events:180:48)
      at ws:95:44
```

# follow the steps below.

1) Go to https://ngrok.com/  and sign up

2) Then select linux from dashboard
![alt text](https://i.ibb.co/wzGGv6p/Screenshot-3.png)

3) You will see a screen like this
![alt text](https://i.ibb.co/j4JSJMW/Screenshot-1.png)

4) Run the commands here one by one on your VPS.  Follow the tutorial to add you auth token and activating app

```
   curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc \
	| sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null \
	&& echo "deb https://ngrok-agent.s3.amazonaws.com buster main" \
	| sudo tee /etc/apt/sources.list.d/ngrok.list \
	&& sudo apt update \
	&& sudo apt install ngrok
```
```
   ngrok config add-authtoken Authkey_in_your_Ngrok_account

```
```
   ngrok http http://localhost:8080

```
5) Create a screen named ngrok
```
   screen -S ngrok
```
   
6) Then use this command to view ngrok screen
  ```
    screen ngrok http 3001
   ```

# A screen like this will open in the terminal
![alt text](https://i.ibb.co/QnxYFZ7/Screenshot-2.png)


7) Copy the URL I marked. And do not close it with CTRL+C, otherwise it will stop working. Close this screen with CTRL+A+D


8) Then paste it into the "WEBHOOK_URL" section in your ".env" file. Save and exit with CTRL+X+Y

9) And restart your scout with
    ```
    docker stop scout
    docker rm scout
    docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout johnsonchasm/chasm-scout
    
    ```

# Congratulations! you made it! Your scout will be online within 15-30 minutes. You can view your scout's status at https://scout.chasm.net/dashboard

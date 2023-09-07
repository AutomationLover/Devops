Here are the steps to expose your local app to the internet via ngrok on an Ubuntu VM.

Step 1: Download ngrok
First, you need to download ngrok for Linux. You can do this by using the following command in your terminal:

```bash
wget -q https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-arm64.tgz -O - | tar xz
```
The url may be out of date. You can visit https://ngrok.com/download, and find the right url to download the image.

Step 2: Unzip the downloaded file
The file you downloaded is a zip file. You need to unzip it. The above command will do this for you automatically.

Step 3: Move ngrok to your PATH
You might want to add ngrok to your PATH so that you can call it from anywhere. Use the following command:

```bash
sudo mv ngrok /usr/local/bin
```

Step 4: Authenticate ngrok
You will need to create an account at `https://ngrok.com/` to get your token. After creating an account, you can find your authtoken at `https://dashboard.ngrok.com/auth/your-authtoken`.

Once you have your token, you can authenticate ngrok using the following command:

```bash
ngrok authtoken YOURTOKEN
```

Replace `YOURTOKEN` with your actual ngrok token.

Step 5: Expose your local development server
Now you can use ngrok to expose your local server to the internet. 

If your app is running on port 8080 for example, you can use the following command to expose it:

```bash
ngrok http 8080
```

Replace `8080` with the port your app is actually running on.

Once you run this command, ngrok will provide a unique URL that you can use to access your local server from the internet. Note that this URL will only remain active as long as the ngrok process is running in your terminal.

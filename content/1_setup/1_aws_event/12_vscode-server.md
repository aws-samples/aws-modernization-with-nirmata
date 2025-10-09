---
title: "Accessing VS Code Server"
chapter: false
weight: 22
---

#### Step 1: Locate Your Credentials

1. Navigate to the **Event Outputs** section in the workshop portal
2. Look for two important pieces of information:
   - **URL**: Your unique VS Code server URL
   - **Password**: Your secure access credentials

![Event Outputs Location](/static/images/nirmata-outputs.png)

#### Step 2: Launch VS Code Server

1. Click the **URL** link in Event Outputs
2. A new browser tab will open with the VS Code login screen
3. Copy the password from Event Outputs
4. Paste it into the password field
5. Click **SUBMIT**

![VS Code Login Screen](/static/images/Vscode-server-login.png)

#### Step 3: Verify Your Connection

Once logged in, you should see the VS Code interface:
- A file explorer on the left
- An editor panel in the center
- A terminal at the bottom (if not visible, press `` Ctrl + ` `` to open it)

![VS Code Interface](/static/images/vs-code.png)

#### Steo 4: Update Kubeconfig with pre-provisioned EKS cluster

```bash
aws eks update-kubeconfig --name demo-aws-cluster --region us-west-2
```

### Important Notes

- The VS Code server is secured through CloudFront for enhanced security
- Your session will remain active throughout the workshop
- All necessary extensions are pre-installed
- Work is automatically saved but will be lost when the workshop ends

## Troubleshooting

If you experience issues:
1. Clear your browser cache
2. Try using an incognito/private window
3. Ensure your corporate firewall isn't blocking the connection
4. Contact the workshop support team if problems persist

## Next Steps

✅ Verify you can access the VS Code server  
✅ Test the terminal functionality  
✅ Continue to the next section of the workshop

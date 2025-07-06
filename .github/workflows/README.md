# GitHub Actions Workflow: Initialization Check

This workflow automatically checks the initialization status of the cloud-mail application.

## Features

- **Upstream Sync**: Pulls the latest changes from the upstream repository
- **Delayed Execution**: Waits 5 minutes before checking initialization
- **HTTP Status Check**: Validates the initialization endpoint response
- **Chinese Text Detection**: Checks for "初始化成功" (initialization successful)
- **Failure Handling**: Outputs detailed error messages when initialization fails

## Configuration

### Environment Variables

The workflow requires the following environment variable to be set as a GitHub secret:

- `INIT_URL`: The full URL to the initialization endpoint
  - Example: `https://your-domain.com/init/your-secret-key`

### Setting up the Secret

1. Go to your repository's **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret**
3. Name: `INIT_URL`
4. Value: Your initialization endpoint URL (including the secret parameter)

## Workflow Triggers

The workflow can be triggered in two ways:

1. **Manual**: Click "Run workflow" in the Actions tab
2. **Scheduled**: Runs daily at 02:00 UTC (optional, can be disabled)

## Expected Responses

### Success Response
- HTTP Status: 200
- Response Body: `初始化成功`
- Workflow Status: ✅ Success

### Failure Responses
- HTTP Status: Non-200
- Response Body: `secret不匹配` (secret mismatch) or other error
- Workflow Status: ❌ Failure

## Workflow Steps

1. **Checkout**: Clones the repository
2. **Pull from upstream**: Syncs with upstream repository
3. **Wait**: Pauses for 5 minutes
4. **Check initialization**: 
   - Makes HTTP request to `INIT_URL`
   - Validates response contains "初始化成功"
   - Outputs success or failure message

## Security Considerations

- The `INIT_URL` is stored as a GitHub secret to keep it secure
- The workflow runs on Ubuntu latest with standard GitHub Actions security
- No sensitive data is logged in the workflow output
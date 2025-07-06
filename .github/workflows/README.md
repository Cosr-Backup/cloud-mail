# PR Closed Initialization Check Workflow

This GitHub Actions workflow automatically triggers when a Pull Request is closed and performs system initialization checks.

## Features

- ✅ **Automated Trigger**: Runs automatically when any PR is closed
- ⏰ **Wait Period**: Waits 5 minutes before starting the check
- 🔗 **Configurable URL**: Uses `INIT_URL` environment variable for flexibility
- 🔒 **Privacy Protection**: Masks sensitive information (URLs, IPs, emails) in logs
- 📊 **Content-Based Validation**: Checks response content, not HTTP status codes
- 💬 **Chinese Support**: Supports Chinese characters in responses

## Configuration

### Required Environment Variables

Set the following variable in your repository settings:

- `INIT_URL`: The URL to check for initialization status (e.g., `https://yourapp.com/api/init/your-secret`)

### How to Set Environment Variables

1. Go to your repository settings
2. Navigate to "Secrets and variables" > "Actions"
3. Click "Variables" tab
4. Add a new repository variable:
   - Name: `INIT_URL`
   - Value: Your initialization URL

## Expected Behavior

### Success Case
- Response content: `初始化成功`
- Workflow status: ✅ Success
- Output: Success message with timestamp

### Failure Cases
- Response content: Any other text (e.g., `secret不匹配`, `初始化失败`)
- Workflow status: ❌ Failure
- Output: Error message with masked sensitive information

## Privacy Protection

The workflow automatically masks the following sensitive information in logs:
- URLs: `https://example.com` → `[URL已屏蔽]`
- IP addresses: `192.168.1.1` → `[IP已屏蔽]`
- Email addresses: `admin@example.com` → `[邮箱已屏蔽]`

## Troubleshooting

### Common Issues

1. **"INIT_URL 环境变量未设置"**
   - Solution: Set the `INIT_URL` variable in repository settings

2. **"无法访问初始化URL"**
   - Check if the URL is accessible
   - Verify network connectivity
   - Ensure the service is running

3. **"初始化失败"**
   - Check the actual response content in logs
   - Verify the initialization endpoint is working correctly
   - Ensure proper authentication/secrets are configured

## Workflow Timeline

1. PR is closed
2. Wait 5 minutes
3. Make HTTP request to `INIT_URL`
4. Check response content
5. Report success or failure

## Integration with Cloud Mail

This workflow is designed to work with the Cloud Mail initialization endpoint:
- Endpoint: `/api/init/:secret`
- Success response: `初始化成功`
- Failure response: `secret不匹配` or other error messages
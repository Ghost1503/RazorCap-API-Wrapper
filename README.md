# RazorCap API Wrapper

Multi-language API wrapper for RazorCap's hCaptcha solving service. Available in Python, TypeScript, JavaScript, and Go.

---

## Features

- Support for both Basic and Enterprise hCaptcha types
- Automatic response polling with configurable timeouts
- Type-safe implementations
- Comprehensive error handling
- Modern async/await support
- Well-documented API methods
- Direct and module-based methods for flexibility

---

## Installation

Clone the repository:
```BASH
git clone https://github.com/Nuu-maan/RazorCap-API-wrapper.git
cd RazorCap-API-wrapper
```
---

### Python
Copy the `python/razorcap.py` file to your project.

### TypeScript/JavaScript
Copy either the `typescript/razorcap.ts` or `javascript/razorcap.js` file to your project.

### Go
Copy the `go/razorcap` directory to your project.

---

## Usage

### Folder Structure
Each language folder contains two files:
1. `index`: Direct method implementation.
2. `main`: Module-based method implementation.

---

### Python
```python
from razorcap import RazorCapAPI, HCaptchaType, HCaptchaTaskData

# Initialize the client
api = RazorCapAPI("your_api_key")

# Create task data
task_data = HCaptchaTaskData(
    sitekey="a9b5fb07-92ff-493f-86fe-352a2803b3df",
    siteurl="discord.com",
    proxy="http://user:pass@ip:port"
)

# Create a task
response = api.create_task(task_data, HCaptchaType.BASIC)

# Get the result
result = await api.wait_for_result(response['task_id'])
print(result['response_key'])
```
---

### TypeScript
```typescript
import { RazorCapAPI, HCaptchaType } from './razorcap';

const api = new RazorCapAPI('your_api_key');

async function solveCaptcha() {
    const taskData = {
        sitekey: 'a9b5fb07-92ff-493f-86fe-352a2803b3df',
        siteurl: 'discord.com',
        proxy: 'http://user:pass@ip:port'
    };

    const response = await api.createTask(taskData, HCaptchaType.BASIC);
    const result = await api.waitForResult(response.task_id);
    
    console.log(result.response_key);
}
```
---

### JavaScript
```javascript
const { RazorCapAPI, HCaptchaType } = require('./razorcap');

const api = new RazorCapAPI('your_api_key');

async function solveCaptcha() {
    const taskData = {
        sitekey: 'a9b5fb07-92ff-493f-86fe-352a2803b3df',
        siteurl: 'discord.com',
        proxy: 'http://user:pass@ip:port'
    };

    const response = await api.createTask(taskData, HCaptchaType.BASIC);
    const result = await api.waitForResult(response.task_id);
    
    console.log(result.response_key);
}
```
---

### Go
```go
package main

import (
    "fmt"
    "./razorcap"
)

func main() {
    client := razorcap.NewClient("your_api_key")
    
    taskData := razorcap.HCaptchaTaskData{
        Sitekey: "a9b5fb07-92ff-493f-86fe-352a2803b3df",
        Siteurl: "discord.com",
        Proxy:   "http://user:pass@ip:port",
    }
    
    response, err := client.CreateTask(taskData, razorcap.HCaptchaBasic)
    if err != nil {
        panic(err)
    }
    
    opts := razorcap.DefaultWaitOptions()
    result, err := client.WaitForResult(response.TaskID, opts)
    if err != nil {
        panic(err)
    }
    
    fmt.Println(result.ResponseKey)
}
```
---

## API Reference

### Create Task
Creates a new hCaptcha solving task.

Parameters:
- `taskData`: Configuration for the hCaptcha task
  - `sitekey`: Website's sitekey
  - `siteurl`: Website URL
  - `proxy`: Proxy in format http://user:pass@ip:port or ip:port
  - `rqdata`: (Optional) Additional request data
- `taskType`: Type of hCaptcha task (BASIC or ENTERPRISE)

### Get Task Result
Retrieves the result of a specific task.

Parameters:
- `taskId`: ID of the task to check

### Wait For Result
Waits for task completion with polling.

Parameters:
- `taskId`: ID of the task to check
- `maxAttempts`: Maximum number of polling attempts (default: 60)
- `delay`: Delay between attempts in milliseconds/seconds (default: 5000ms/5s)

---

## Error Handling

All implementations include comprehensive error handling for:
- Network errors
- Invalid API responses
- Timeout errors
- Task-specific errors

---
### API Endpoints

#### Create a New Task
- **POST** `https://api.razorcap.xyz/create_task`
- **Body**:
```json
{
  "key": "api_key",
  "type": "hcaptcha_basic",
  "data": {
    "sitekey": "a9b5fb07-92ff-493f-86fe-352a2803b3df",
    "siteurl": "discord.com",
    "proxy": "http://user:pass@ip:port",
    "rqdata": "captcha_rqdata"
  }
}
```

**Response**:
```json
{
  "price": "0.003",
  "status": "solving",
  "task_id": "task_id"
}
```

#### Get Result of the Task
- **GET** `https://api.razorcap.xyz/get_result/{task_id}`
  
**Response**:
```json
{
  "response_key": "P1_eyJ0eXAiOiJKV1QiLCJhbGciOiJIU...",
  "status": "solved"
}
```
----

## Disclaimer

This project is officially maintained and supported by RazorCap.


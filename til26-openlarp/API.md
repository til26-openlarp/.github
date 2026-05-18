# API Specifications

Complete API documentation for all TIL-26 challenges.

## Common Endpoints

All services implement these endpoints:

### Health Check

```
GET /health
Response: 200 OK
```

Use to verify service is running.

---

## AE — Agent Environment API

**Port:** 5003  
**Endpoint:** `POST /act`

### Request

```json
{
  "instances": [
    {
      "observation": [
        // Multi-channel viewcone (float array)
        // Shape: depends on config (default 9x9x25)
      ],
      "info": {}
    }
  ]
}
```

**Observation detail:** See `ae/til-26-ae/README.md` for channel meanings

### Response

```json
{
  "predictions": [0]
}
```

**Action values:**
- 0: FORWARD
- 1: BACKWARD
- 2: LEFT
- 3: RIGHT
- 4: STAY
- 5: PLACE_BOMB

### Example

```python
import requests
import numpy as np

# Create observation
obs = np.random.randn(9, 9, 25).tolist()

# Request
response = requests.post(
    "http://localhost:5003/act",
    json={"instances": [{"observation": obs, "info": {}}]}
)

action = response.json()["predictions"][0]
print(f"Action: {action}")  # 0-5
```

---

## CV — Computer Vision API

**Port:** 5002  
**Endpoint:** `POST /cv`

### Request

```json
{
  "instances": [
    {
      "key": 0,
      "b64": "BASE64_ENCODED_JPEG_IMAGE"
    }
  ]
}
```

**Fields:**
- `key` — Unique identifier (optional, for tracking)
- `b64` — Base64-encoded JPEG image

### Response

```json
{
  "predictions": [
    [
      {
        "bbox": [x, y, w, h],
        "category_id": 0
      }
    ]
  ]
}
```

**Format:**
- `bbox` — `[x, y, width, height]` (top-left corner + size)
- `category_id` — Integer class ID (0–N)
- Return empty list `[]` if no objects detected

### Example

```python
import requests
import base64
from PIL import Image
from io import BytesIO

# Load image
image = Image.open("test.jpg")
buffer = BytesIO()
image.save(buffer, format="JPEG")
b64 = base64.b64encode(buffer.getvalue()).decode()

# Request
response = requests.post(
    "http://localhost:5002/cv",
    json={"instances": [{"key": 0, "b64": b64}]}
)

predictions = response.json()["predictions"]
for image_preds in predictions:
    for det in image_preds:
        x, y, w, h = det["bbox"]
        category = det["category_id"]
        print(f"Object at ({x}, {y}), size {w}x{h}, class {category}")
```

---

## NLP — Natural Language Processing API

**Port:** 5004  
**Endpoint:** `POST /nlp`

### Two-Step Process

#### Step 1: Load Documents

```json
{
  "instances": [
    {
      "documents": [
        "Document 1 text here...",
        "Document 2 text here...",
        ...
      ]
    }
  ]
}
```

**Response:**
```json
{
  "predictions": ["loaded"]
}
```

Must wait for "loaded" response before proceeding to Step 2.

#### Step 2: Answer Questions

```json
{
  "instances": [
    {"question": "What is the capital of France?"},
    {"question": "Who wrote Hamlet?"}
  ]
}
```

**Response:**
```json
{
  "predictions": [
    "Paris",
    "William Shakespeare"
  ]
}
```

**Format:**
- Each prediction is an answer string
- Order matches input order
- Length matches number of instances

### Example

```python
import requests

# Step 1: Load documents
docs_response = requests.post(
    "http://localhost:5004/nlp",
    json={
        "instances": [{
            "documents": [
                "Paris is the capital of France.",
                "Hamlet was written by William Shakespeare."
            ]
        }]
    }
)

print(docs_response.json())  # {"predictions": ["loaded"]}

# Step 2: Answer questions
qa_response = requests.post(
    "http://localhost:5004/nlp",
    json={
        "instances": [
            {"question": "What is the capital of France?"},
            {"question": "Who wrote Hamlet?"}
        ]
    }
)

answers = qa_response.json()["predictions"]
print(answers)  # ["Paris", "William Shakespeare"]
```

---

## Error Handling

### Status Codes

- **200 OK** — Success
- **400 Bad Request** — Invalid input format
- **500 Internal Server Error** — Server error

### Error Response

```json
{
  "detail": "Error message explaining what went wrong"
}
```

### Example

```python
response = requests.post(url, json=payload)

if response.status_code == 200:
    result = response.json()
    print(result["predictions"])
elif response.status_code == 400:
    print(f"Invalid request: {response.json()['detail']}")
else:
    print(f"Server error: {response.status_code}")
```

---

## Batch Processing

All endpoints support **batch processing**:

```json
{
  "instances": [
    {...},
    {...},
    {...}
  ]
}
```

**Response:** Predictions array has same length and order as instances

### Limits

- **AE:** Typically 1 instance per request (live game)
- **CV:** Multiple images recommended for efficiency
- **NLP:** Multiple questions in single request

---

## Data Type Reference

### AE Observation

```python
observation = {
    "agent_viewcone": [H, W, 25],        # float32, normalized 0-1
    "base_viewcone": [B, B, 25],         # float32, normalized 0-1
    "direction": int,                     # 0-3
    "location": [2],                      # uint8, grid coords
    "base_location": [2],                 # uint8, grid coords
    "health": float,                      # 0-max_health
    "frozen_ticks": int,                  # 0 = alive
    "base_health": float,                 # 0-max_health
    "team_resources": float,              # bomb budget
    "team_bombs": int,                    # count
    "step": int,                          # current step
    "action_mask": [6]                    # uint8, 0-1
}
```

### CV Detection

```python
detection = {
    "bbox": [x, y, w, h],                # floats
    "category_id": int                   # 0-N
}
```

### NLP Question

```python
question_instance = {
    "question": str                      # question text
}
```

---

## Timeout & Performance

Typical latencies:

| Challenge | Latency | Notes |
|-----------|---------|-------|
| AE | <100ms | Pure inference |
| CV | 50-200ms | Depends on model size |
| NLP | 200-2000ms | Includes retrieval + generation |

---

## Rate Limiting

Competition platform may implement rate limits:
- Check response headers for `X-RateLimit-*` fields
- Implement exponential backoff for retries

---

## Testing Tools

### Test Payloads

```bash
# AE
curl -X POST http://localhost:5003/act \
  -H "Content-Type: application/json" \
  -d @ae_test.json

# CV
curl -X POST http://localhost:5002/cv \
  -H "Content-Type: application/json" \
  -d @cv_test.json

# NLP
curl -X POST http://localhost:5004/nlp \
  -H "Content-Type: application/json" \
  -d @nlp_test.json
```

### Python Client

```python
import requests
import json

class TIL26Client:
    def __init__(self, challenge, port):
        self.url = f"http://localhost:{port}"
        self.challenge = challenge
    
    def health(self):
        return requests.get(f"{self.url}/health")
    
    def request(self, payload):
        endpoint = f"/{self.challenge}"
        return requests.post(f"{self.url}{endpoint}", json=payload)

# Usage
client = TIL26Client("cv", 5002)
result = client.request({
    "instances": [{"key": 0, "b64": "..."}]
})
```

---

## API Contracts (Don't Break These!)

- Response must have `"predictions"` key
- Predictions must be a list
- Length of predictions == length of instances
- Response must be JSON-serializable
- No tensors, numpy arrays, or custom objects in response

---

**See challenge-specific README.md files for detailed specs.**

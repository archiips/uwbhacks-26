# Verification model

Place the quantized MobileCLIP image model at:

```text
models/mobileclip_image_quantized.onnx
```

The server expects this exact filename, relative to the backend working directory. If the model is missing, it logs a warning at startup and `/verify` returns HTTP 503. Restart the server after adding the model.

Model files are excluded from Git and must be supplied separately.

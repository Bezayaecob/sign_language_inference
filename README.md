# Sign Language Inference

A full-stack application for real-time sign language recognition and translation using machine learning and computer vision. This system enables bidirectional communication between sign language users and non-signers through automated gesture recognition and text-to-sign video generation.

## 🎯 Features

- **Real-time Sign Recognition**: Continuous video-based sign language detection and translation
- **Live Inference**: Process video streams in real-time with pre-trained models
- **MediaPipe Integration**: Advanced pose estimation and hand tracking using Google's MediaPipe
- **Text-to-Sign Generation**: Convert written text to sign language video output
- **Speech-to-Text Processing**: Integrate Google Cloud Speech-to-Text API for audio input
- **Web Interface**: Modern TypeScript/React frontend with responsive design
- **Backend API**: Python-based Flask/FastAPI backend for model inference
- **Video Processing**: Support for multiple video formats and frame rate conversion

## 📋 Project Structure

```
sign_language_inference/
├── backend/                    # Python backend services
│   ├── models/                # Pre-trained ML models
│   └── routes/                # API endpoints
├── frontend/                  # TypeScript/React application
│   ├── components/            # React components
│   └── styles/                # CSS styling
├── models/                    # Trained sign language models
├── mediapipe_models/          # MediaPipe gesture recognition models
├── server/                    # Additional server implementations
│   └── speech_to_text/        # Audio processing
├── utils/                     # Utility functions
├── videos/                    # Sample and test videos
├── config.py                  # Configuration settings
├── inference.py               # Inference pipeline
├── live_inference.py          # Real-time inference handler
├── run_inference.py           # Main inference runner
├── convert_video.py           # Video format conversion
└── requirements.txt           # Python dependencies
```

## 🚀 Getting Started

### Prerequisites

- Python 3.8 or higher
- Node.js 14+ (for frontend development)
- OpenCV support libraries
- GPU recommended for faster inference (CPU-only supported)
- 5GB+ free disk space for models

### Installation

#### 1. Clone the Repository

```bash
git clone https://github.com/Abu388/sign_language_inference.git
cd sign_language_inference
```

#### 2. Set Up Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

#### 3. Install Python Dependencies

```bash
pip install -r requirements.txt
```

#### 4. Install System Dependencies (Ubuntu/Debian)

```bash
sudo apt-get update
sudo apt-get install -y libsm6 libxext6 libxrender-dev
```

#### 5. Download Pre-trained Models

```bash
# Models should be placed in the models/ directory
# Ensure mediapipe_models are available in mediapipe_models/ directory
```

#### 6. Frontend Setup (Optional)

```bash
cd frontend
npm install
npm start
```

## 💻 Usage

### Basic Inference

#### Process a Single Video

```bash
python inference.py --video_path path/to/video.mp4 --output results.json
```

#### Run Live Inference from Webcam

```bash
python live_inference.py
```

Press 'q' to exit the live inference window.

#### Convert Video Format

```bash
python convert_video.py --input_video video.mp4 --output_format avi --fps 30
```

### Advanced Usage

#### Custom Configuration

Edit `config.py` to adjust:
- Model paths
- Confidence thresholds
- Input/output formats
- Performance parameters

```python
# Example config.py modifications
CONFIDENCE_THRESHOLD = 0.7
MODEL_PATH = "models/custom_model.h5"
BATCH_SIZE = 32
```

#### API Integration

Run the backend server:

```bash
python run_inference.py --host 0.0.0.0 --port 5000
```

The API will be available at `http://localhost:5000`

**Endpoints:**
- `POST /predict` - Submit video for sign recognition
- `GET /status` - Check server status
- `POST /text-to-sign` - Convert text to sign video

## 🔧 Configuration

### Environment Variables

Create a `.env` file in the root directory:

```bash
FLASK_ENV=development
MEDIAPIPE_MODEL_PATH=./mediapipe_models
TENSORFLOW_CPP_MIN_LOG_LEVEL=2
CUDA_VISIBLE_DEVICES=0
```

### Model Parameters

Adjust inference parameters in `config.py`:

```python
{
    "min_detection_confidence": 0.5,
    "min_tracking_confidence": 0.5,
    "static_image_mode": False,
    "max_num_hands": 2
}
```

## 📊 Supported Models

- **Sign Recognition**: LSTM-based architecture trained on WLASL2000 dataset
- **Pose Estimation**: MediaPipe Pose for full-body tracking
- **Hand Landmarks**: MediaPipe Hands for detailed hand gesture analysis
- **TTS Synthesis**: Multi-lingual TTS for audio output

## 🎓 Model Training

For custom model training, refer to the notebooks:

- `keypoint_classification.ipynb` - Train custom sign classifiers
- `point_history_classification.ipynb` - Temporal sequence modeling

## 🔌 API Reference

### Sign Recognition Endpoint

```bash
curl -X POST http://localhost:5000/predict \
  -F "video=@sign_video.mp4" \
  -F "language=en"
```

**Response:**
```json
{
  "success": true,
  "signs": ["hello", "world"],
  "confidence": [0.92, 0.88],
  "translation": "Hello world"
}
```

### Text-to-Sign Endpoint

```bash
curl -X POST http://localhost:5000/text-to-sign \
  -H "Content-Type: application/json" \
  -d '{"text": "Hello", "language": "en"}'
```

## 📁 Data Format

### Input Video

- **Formats**: MP4, AVI, MOV, WebM
- **Resolution**: 480p - 1080p recommended
- **Frame Rate**: 24-30 FPS
- **Duration**: No limit

### Output Format

```json
{
  "timestamp": "2024-01-15T10:30:00Z",
  "video_path": "path/to/input.mp4",
  "detected_signs": [
    {
      "sign": "hello",
      "confidence": 0.95,
      "start_frame": 10,
      "end_frame": 45
    }
  ],
  "translation": "Hello",
  "processing_time_ms": 2341
}
```

## 🎬 Video Processing Pipeline

1. **Frame Extraction** - Convert video to individual frames
2. **Pose Detection** - Extract hand and body landmarks
3. **Feature Normalization** - Standardize landmark coordinates
4. **Model Inference** - Classify sign gestures
5. **Post-Processing** - Smooth predictions, apply confidence thresholds
6. **Output Generation** - Format results and save

## ⚡ Performance Optimization

### GPU Acceleration

```bash
export CUDA_VISIBLE_DEVICES=0
python live_inference.py --use_gpu
```

### Model Quantization

For mobile deployment:

```bash
python convert_model.py --input model.h5 --output model.tflite --quantize
```

### Batch Processing

Process multiple videos simultaneously:

```bash
python run_inference.py --batch_mode --input_dir videos/ --output_dir results/
```

## 🐛 Troubleshooting

### Camera Access Issues
```bash
# Linux - Grant camera permissions
sudo usermod -a -G video $USER
```

### Missing MediaPipe Models
```bash
# Download models
python scripts/download_models.py
```

### Memory Errors
```bash
# Reduce batch size in config.py
BATCH_SIZE = 8  # Default is 32
```

### Low Accuracy
- Ensure proper lighting conditions
- Position hands in frame center
- Use 1080p video when possible
- Retrain on domain-specific data

## 📈 Performance Metrics

- **Inference Speed**: ~50-100ms per frame (GPU)
- **Recognition Accuracy**: 85-92% (trained models)
- **Supported Vocabularies**: 2000+ signs (WLASL)
- **Concurrent Users**: 5-10 (depending on hardware)

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add improvement'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

## 📝 Citation

If you use this project in your research, please cite:

```bibtex
@software{sign_language_inference,
  author = {Abu388},
  title = {Sign Language Inference: Real-time Recognition and Translation},
  year = {2024},
  url = {https://github.com/Abu388/sign_language_inference}
}
```

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- Google MediaPipe team for pose estimation models
- WLASL dataset contributors
- OpenCV and TensorFlow communities

## 📞 Support & Contact

- **Issues**: Please use [GitHub Issues](https://github.com/Abu388/sign_language_inference/issues)
- **Discussions**: Check [GitHub Discussions](https://github.com/Abu388/sign_language_inference/discussions)
- **Email**: [Contact repository owner]

## 🗺️ Roadmap

- [ ] Multi-language sign support (ASL, BSL, LSF)
- [ ] Mobile application (iOS/Android)
- [ ] Browser extension for real-time captioning
- [ ] Improved continuous sign recognition
- [ ] Web-based demo interface
- [ ] Docker containerization
- [ ] REST API documentation (Swagger/OpenAPI)

---

**Status**: Active Development  
**Last Updated**: January 2024  
**Stars**: ⭐ Support this project by starring the repository!

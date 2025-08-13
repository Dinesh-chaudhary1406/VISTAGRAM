# Vistagram 📸

**A blend of "Visit" + "Instagram" style timeline for sharing Point of Interest (POI) experiences**

Vistagram is a full-stack web application that allows users to capture and upload images of Points of Interest using their device's camera, add captions, and browse a timeline of posts with social engagement features.

## 🌟 Features

### Core Functionality
- **📸 Image Capture & Upload**: Use device camera to capture POI images
- **🖊️ Caption Support**: Add descriptive captions to your posts
- **🕒 Timeline View**: Browse posts in reverse chronological order
- **❤️ Like System**: Like/unlike posts with persistent counters
- **🔗 Share Functionality**: Share posts via links with share count tracking

### Advanced Features
- **🤖 AI-Generated Captions**: Automatically generate contextually relevant captions
- **🏛️ POI Recognition**: Identify landmarks and suggest related experiences
- **📱 Responsive Design**: Optimized for mobile and desktop
- **⚡ Real-time Updates**: Live engagement counters

## 🚀 Tech Stack

### Frontend
- **React 18** with TypeScript
- **Tailwind CSS** for styling
- **Lucide React** for icons
- **Camera API** for image capture
- **PWA** capabilities for mobile experience

### Backend
- **Node.js** with Express
- **MongoDB** with Mongoose
- **Multer** for file uploads
- **Cloudinary** for image storage
- **JWT** for authentication

### AI Integration
- **OpenAI GPT-4 Vision API** for caption generation
- **Google Vision API** for POI recognition
- **Hugging Face** for image classification

## 📋 Prerequisites

Before running this application, make sure you have:

- Node.js (v18 or higher)
- MongoDB (local or Atlas)
- npm or yarn package manager

### Required API Keys
- OpenAI API key (for AI captions)
- Google Vision API key (for POI recognition)
- Cloudinary account (for image storage)

## 🛠️ Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/vistagram.git
   cd vistagram
   ```

2. **Install dependencies**
   ```bash
   # Install backend dependencies
   cd backend
   npm install
   
   # Install frontend dependencies
   cd ../frontend
   npm install
   ```

3. **Environment Setup**
   
   Create `.env` files in both backend and frontend directories:

   **Backend `.env`:**
   ```env
   PORT=5000
   MONGODB_URI=mongodb://localhost:27017/vistagram
   JWT_SECRET=your_jwt_secret_key
   CLOUDINARY_CLOUD_NAME=your_cloudinary_name
   CLOUDINARY_API_KEY=your_cloudinary_key
   CLOUDINARY_API_SECRET=your_cloudinary_secret
   OPENAI_API_KEY=your_openai_api_key
   GOOGLE_VISION_API_KEY=your_google_vision_key
   ```

   **Frontend `.env`:**
   ```env
   REACT_APP_API_URL=http://localhost:5000/api
   REACT_APP_SOCKET_URL=http://localhost:5000
   ```

4. **Seed the Database**
   ```bash
   cd backend
   npm run seed
   ```

5. **Start the Application**
   ```bash
   # Start backend server
   cd backend
   npm run dev
   
   # Start frontend (in new terminal)
   cd frontend
   npm start
   ```


## 🎯 API Endpoints

### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `GET /api/auth/profile` - Get user profile

### Posts
- `GET /api/posts` - Get all posts (paginated)
- `POST /api/posts` - Create new post
- `POST /api/posts/:id/like` - Toggle like on post
- `POST /api/posts/:id/share` - Increment share count
- `GET /api/posts/:id/share-link` - Get shareable link

### AI Features
- `POST /api/ai/generate-caption` - Generate AI caption for image
- `POST /api/ai/identify-poi` - Identify POI in image

## 🤖 AI Integration

### Caption Generation
Uses OpenAI's GPT-4 Vision API to analyze uploaded images and generate contextually relevant captions.

```javascript
const generateCaption = async (imageUrl) => {
  const response = await openai.chat.completions.create({
    model: "gpt-4-vision-preview",
    messages: [{
      role: "user",
      content: [
        { type: "text", text: "Generate a travel-focused caption for this POI image" },
        { type: "image_url", image_url: { url: imageUrl } }
      ]
    }]
  });
  return response.choices[0].message.content;
};
```

### POI Recognition
Integrates Google Vision API for landmark detection and identification.

```javascript
const identifyPOI = async (imageBuffer) => {
  const [result] = await visionClient.landmarkDetection(imageBuffer);
  const landmarks = result.landmarkAnnotations;
  return landmarks.map(landmark => ({
    name: landmark.description,
    confidence: landmark.score
  }));
};
```

## 📊 Database Schema

### User Model
```javascript
{
  _id: ObjectId,
  username: String (unique),
  email: String (unique),
  password: String (hashed),
  profilePicture: String,
  createdAt: Date,
  updatedAt: Date
}
```

### Post Model
```javascript
{
  _id: ObjectId,
  user: ObjectId (ref: 'User'),
  username: String,
  imageUrl: String,
  caption: String,
  location: {
    name: String,
    coordinates: [Number] // [longitude, latitude]
  },
  likes: [ObjectId] (ref: 'User'),
  likeCount: Number,
  shareCount: Number,
  aiGenerated: Boolean,
  poiData: {
    identified: Boolean,
    name: String,
    confidence: Number
  },
  createdAt: Date,
  updatedAt: Date
}
```

## 🧪 Testing

### Running Tests
```bash
# Backend tests
cd backend
npm test

# Frontend tests
cd frontend
npm test

# Run all tests
npm run test:all
```

### Test Coverage
- Unit tests for controllers and services
- Integration tests for API endpoints
- Frontend component tests with React Testing Library
- E2E tests with Cypress

## 📱 Mobile Optimization

### PWA Features
- Offline capability for viewing cached posts
- Add to home screen functionality
- Camera access optimization
- Touch-friendly UI components

### Responsive Design
- Mobile-first approach
- Optimized image upload flow
- Touch gestures for interactions
- Progressive image loading

## 🔒 Security Features

- JWT-based authentication
- Image upload validation and sanitization
- Rate limiting for API endpoints
- CORS configuration
- Input validation and sanitization
- Secure file storage with Cloudinary

## 🚀 Deployment

### Environment Setup
1. **Production Build**
   ```bash
   cd frontend
   npm run build
   ```

2. **Environment Variables**
   Update production environment variables for:
   - Database connection
   - API keys
   - Domain configuration

3. **Deployment Platforms**
   - **Frontend**: Vercel, Netlify
   - **Backend**: Railway, Heroku, DigitalOcean
   - **Database**: MongoDB Atlas

### Docker Deployment
```dockerfile
# Dockerfile included for containerized deployment
docker-compose up -d
```

## 🎨 UI/UX Features

### Design Principles
- **Intuitive Navigation**: Clear visual hierarchy
- **Instagram-like Experience**: Familiar interaction patterns
- **Camera-first Design**: Optimized capture flow
- **Engagement Focus**: Prominent like and share buttons

### Custom Components
- Swipeable post cards
- Infinite scroll timeline
- Camera preview with filters
- Real-time engagement counters

## 🔧 Development Scripts

```json
{
  "dev": "Start development servers",
  "build": "Build production version",
  "test": "Run test suite",
  "seed": "Populate database with sample data",
  "lint": "Run ESLint",
  "format": "Format code with Prettier"
}
```

## 📈 Performance Optimizations

- **Image Optimization**: WebP format with fallbacks
- **Lazy Loading**: Posts loaded on scroll
- **Caching**: Redis for frequently accessed data
- **CDN**: Cloudinary for global image delivery
- **Bundle Splitting**: Code splitting for faster loads

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Code Style
- ESLint and Prettier configured
- Conventional commit messages
- TypeScript for type safety

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- OpenAI for GPT-4 Vision API
- Google Cloud Vision for POI recognition
- Cloudinary for image management
- MongoDB for database solutions

## 📞 Support

For questions or support, please open an issue on GitHub or contact the development team.

---

**Built with ❤️ for exploring and sharing amazing places around the world** 🌍
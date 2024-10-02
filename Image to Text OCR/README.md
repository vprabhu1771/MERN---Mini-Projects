To create an Image to Text conversion app using React and Tesseract.js, follow these steps:

### Prerequisites:
- Basic knowledge of React
- Node.js installed
- Tesseract.js library

### Steps:

1. **Set Up a React Project:**
   Initialize a new React project if you don’t have one:
```bash
npx create vite react-ocr
cd react-ocr
npm install
npm run dev
```

2. **Install Dependencies:**
   Install `tesseract.js` to perform Optical Character Recognition (OCR):
```bash
npm install tesseract.js
```

3. **Create ImageToText Component:**
   Create a new component to handle image uploading and text extraction.

   **`src/components/ImageToText.js`:**
```jsx
import React, { useState } from 'react';

const ImageToText = () => {
  const [image, setImage] = useState(null);
  const [text, setText] = useState('');
  const [progress, setProgress] = useState(0);

  const handleImageUpload = async (event) => {
    const file = event.target.files[0];
    if (file) {
      setImage(URL.createObjectURL(file));
      await extractText(file);
    }
  };

  const extractText = async (file) => {
    const Tesseract = await import('tesseract.js');  // Dynamic import
    Tesseract.recognize(
      file,
      'eng',
      {
        logger: (m) => setProgress(Math.round(m.progress * 100)),
      }
    )
    .then(({ data: { text } }) => {
      setText(text);
    })
    .catch((error) => {
      console.error('OCR error:', error);
    });
  };

  return (
    <div style={{ textAlign: 'center', padding: '20px' }}>
      <h2>Image to Text Converter (OCR)</h2>
      <input type="file" onChange={handleImageUpload} accept="image/*" />
      {image && <img src={image} alt="Uploaded" style={{ maxWidth: '100%', marginTop: '20px' }} />}
      {progress > 0 && <p>Processing: {progress}%</p>}
      {text && (
        <div style={{ marginTop: '20px', textAlign: 'left' }}>
          <h4>Extracted Text:</h4>
          <textarea rows="10" value={text} readOnly style={{ width: '100%', padding: '10px' }} />
        </div>
      )}
    </div>
  );
};

export default ImageToText;
```

4. **Update App Component:**
   Import and use the `ImageToText` component in your `App.js`.

   **`src/App.js`:**
   ```jsx
   import React from 'react';
   import ImageToText from './components/ImageToText';

   function App() {
     return (
       <div className="App">
         <ImageToText />
       </div>
     );
   }

   export default App;
   ```

5. **Run the App:**
   Start the development server:
   ```bash
   npm run dev
   ```

### Explanation:
- The app allows users to upload an image file.
- The `handleImageUpload` function triggers when an image is uploaded and displays the image preview.
- The `extractText` function uses `Tesseract.js` to extract text from the image, updating the progress as OCR proceeds.
- Once the text is extracted, it’s displayed in a textarea for easy copying.

### Optional:
You can add styles and error handling to enhance the app's user experience.
# Multer(File Uploads)

### **Table of Contents**

1. **Introduction to File Uploads in Node.js**
2. **What is Multer?**
3. **Installing Multer**
4. **Basic File Upload Example**
5. **Handling Multiple File Uploads**
6. **Setting File Upload Limits**
7. **Filtering File Types (Only Images, PDFs, etc.)**
8. **Storing Files with Custom File Names**
9. **Uploading Files to Cloud Storage (AWS S3, Cloudinary)**
10. **Conclusion**

***

### **1. Introduction to File Uploads in Node.js**

1. Uploading files (images, PDFs, etc.) is a common feature in web applications.&#x20;
2. In a **Node.js Express application**, handling file uploads requires a middleware that processes `multipart/form-data`, which is not handled by default in Express.

#### **Use Cases for File Uploads**

✔ Profile picture uploads\
✔ Document uploads (PDFs, Word files)\
✔ Product images in e-commerce sites\
✔ Storing reports and receipts

***

### **2. What is Multer?**

Multer is a **Node.js middleware** used for handling `multipart/form-data`, which is required for file uploads. It helps process the uploaded file and store it on the **server or cloud storage**.

#### **Why Use Multer?**

✔ Easy to integrate with Express\
✔ Supports **single & multiple file uploads**\
✔ Allows **file validation (type, size, etc.)**\
✔ Stores files **locally or in cloud storage**

***

### **3. Installing Multer**

First, install **Multer** using npm:

```sh
npm install multer express
```

***

### **4. Basic File Upload Example**

#### **Steps to Upload a File**

1. Create an Express server
2. Set up Multer as middleware
3. Use Multer to handle file uploads
4. Process the uploaded file

📄 **server.js**

```javascript
const express = require("express");
const multer = require("multer");
const path = require("path");

const app = express();

// Configure Multer for file storage
const storage = multer.diskStorage({
    destination: "./uploads/", // Directory where files will be stored
    filename: (req, file, cb) => {
        cb(null, file.fieldname + "-" + Date.now() + path.extname(file.originalname));
    }
});

const upload = multer({ storage: storage });

// Route to handle file upload
app.post("/upload", upload.single("file"), (req, res) => {
    if (!req.file) {
        return res.status(400).send("No file uploaded.");
    }
    res.send(`File uploaded successfully: ${req.file.filename}`);
});

app.listen(3000, () => {
    console.log("Server running on port 3000");
});
```

#### **How It Works?**

* The uploaded file is stored in the `/uploads/` folder.
* The file name is customized using `fieldname + timestamp + extension`.
* `upload.single("file")` allows **uploading a single file** with the form field name `"file"`.

#### **Client-side Form Example (HTML)**

```html
<form action="/upload" method="POST" enctype="multipart/form-data">
    <input type="file" name="file">
    <button type="submit">Upload</button>
</form>
```

***

### **5. Handling Multiple File Uploads**

To allow users to upload **multiple files**, modify the route as follows:

📄 **server.js**

```javascript
app.post("/upload-multiple", upload.array("files", 5), (req, res) => {
    if (!req.files || req.files.length === 0) {
        return res.status(400).send("No files uploaded.");
    }
    res.send(`Files uploaded successfully: ${req.files.map(file => file.filename).join(", ")}`);
});
```

📄 **Client-side Form Example (HTML)**

```html
<form action="/upload-multiple" method="POST" enctype="multipart/form-data">
    <input type="file" name="files" multiple>
    <button type="submit">Upload</button>
</form>
```

***

### **6. Setting File Upload Limits**

To prevent **large file uploads**, we can set file size limits using `limits` in Multer.

📄 **server.js**

```javascript
const uploadWithLimit = multer({
    storage: storage,
    limits: { fileSize: 2 * 1024 * 1024 } // 2MB file size limit
});

app.post("/upload-limited", uploadWithLimit.single("file"), (req, res) => {
    res.send("File uploaded successfully!");
});
```

***

### **7. Filtering File Types (Only Images, PDFs, etc.)**

To allow only specific file types (e.g., images), use **fileFilter**:

📄 **server.js**

```javascript
const fileFilter = (req, file, cb) => {
    const allowedTypes = ["image/jpeg", "image/png", "application/pdf"];
    if (allowedTypes.includes(file.mimetype)) {
        cb(null, true); // Accept file
    } else {
        cb(new Error("Only JPEG, PNG, and PDF files are allowed"), false); // Reject file
    }
};

const uploadFiltered = multer({
    storage: storage,
    fileFilter: fileFilter
});

app.post("/upload-filtered", uploadFiltered.single("file"), (req, res) => {
    res.send("File uploaded successfully!");
});
```

***

### **8. Storing Files with Custom File Names**

By default, Multer assigns random names. You can customize the **file name**:

📄 **server.js**

```javascript
const customStorage = multer.diskStorage({
    destination: "./uploads/",
    filename: (req, file, cb) => {
        cb(null, file.fieldname + "-" + Date.now() + "-" + file.originalname);
    }
});

const uploadCustom = multer({ storage: customStorage });

app.post("/upload-custom", uploadCustom.single("file"), (req, res) => {
    res.send("File uploaded with a custom name!");
});
```

***

### **9. Uploading Files to Cloud Storage (AWS S3, Cloudinary)**

Instead of storing files locally, you can **upload them to AWS S3 or Cloudinary**.

📄 **Example of Uploading to Cloudinary**

```sh
npm install cloudinary multer-storage-cloudinary
```

📄 **server.js**

```javascript
const cloudinary = require("cloudinary").v2;
const { CloudinaryStorage } = require("multer-storage-cloudinary");

cloudinary.config({
    cloud_name: "your_cloud_name",
    api_key: "your_api_key",
    api_secret: "your_api_secret"
});

const storage = new CloudinaryStorage({
    cloudinary: cloudinary,
    params: {
        folder: "uploads",
        allowed_formats: ["jpg", "png", "pdf"]
    }
});

const uploadCloudinary = multer({ storage: storage });

app.post("/upload-cloudinary", uploadCloudinary.single("file"), (req, res) => {
    res.send(`File uploaded to Cloudinary: ${req.file.path}`);
});
```

***

### **10. Conclusion**

✅ **Multer is a powerful tool for handling file uploads in Node.js.**\
✅ Supports **single & multiple file uploads**.\
✅ Allows **file filtering, size limits, and custom file names**.\
✅ Files can be stored **locally or on cloud storage** (AWS S3, Cloudinary).


----

# Folder Structure 

```
readnest/
│
├── app.js
├── package.json
├── .env
│
├── config/
│   └── db.js
│
├── controllers/
│   ├── authController.js
│   ├── bookController.js
│   ├── bookmarkController.js
│   └── reviewController.js
│
├── middlewares/
│   ├── authMiddleware.js
│   ├── guestMiddleware.js
│   ├── errorMiddleware.js
│   ├── uploadMiddleware.js
│   └── validateMiddleware.js
│
├── models/
│   ├── User.model.js
│   ├── Book.model.js
│   └── Review.model.js
│
├── routes/
│   ├── authRoutes.js
│   ├── bookRoutes.js
│   ├── bookmarkRoutes.js
│   └── reviewRoutes.js
│
├── views/
│   │
│   ├── partials/
│   │   ├── header.ejs
│   │   ├── navbar.ejs
│   │   ├── footer.ejs
│   │   └── messages.ejs
│   │
│   ├── auth/
│   │   ├── register.ejs
│   │   └── login.ejs
│   │
│   ├── books/
│   │   ├── index.ejs
│   │   ├── single.ejs
│   │   ├── create.ejs
│   │   ├── edit.ejs
│   │   └── search.ejs
│   │
│   ├── dashboard/
│   │   ├── index.ejs
│   │   ├── bookmarks.ejs
│   │   └── mybooks.ejs
│   │
│   ├── errors/
│   │   ├── 404.ejs
│   │   └── error.ejs
│   │
│   └── home.ejs
│
├── public/
│   │
│   ├── css/
│   │   └── style.css
│   │
│   ├── js/
│   │   └── main.js
│   │
│   ├── images/
│   │
│   ├── uploads/
│   │   ├── covers/
│   │   └── pdfs/
│   │
│   └── icons/
│
├── validators/
│   ├── authValidator.js
│   ├── bookValidator.js
│   └── reviewValidator.js
│
├── utils/
│   ├── pagination.js
│   ├── deleteFile.js
│   └── calculateRating.js
│
└── node_modules/
```



# User.model.
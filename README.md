# 🏠 Sahand Estate

A full-stack real estate listing platform built with **Next.js 15** and **React 19**. Users can sign up, sign in, browse and search property listings, and manage their own listings — including image uploads — from a personal dashboard.

**Live demo:** https://real-estate-hsno.onrender.com/
**Repository:** https://github.com/James1oliveira/real-estate

> ⚠️ The app is hosted on Render's free tier, so the first request after a period of inactivity may take up to a minute to spin up.

---

## ✨ Features

- **Authentication** — email/password sign up and sign in with `next-auth` (Credentials provider), passwords hashed with `bcryptjs`, JWT sessions.
- **Listings CRUD** — create, read, update, and delete property listings (rent or sale).
- **Image uploads** — up to 6 images per listing, uploaded to **Cloudinary** with automatic format/quality optimization and a 2MB per-file limit.
- **Search & filters** — filter by search term, listing type (rent/sale), parking, furnished, and special offers, with sorting by price or date and paginated "Show more" loading.
- **Special offers** — mark a listing with a discounted price; the UI automatically shows savings.
- **Personal dashboard** — logged-in users see stats on their listings (total, for rent, for sale) and can edit or delete them.
- **Listing detail page** — server-rendered listing page with an image gallery, features grid (beds/baths/parking/furnished), and owner-only edit/delete actions.
- **Responsive UI** — styled with Tailwind CSS.

---

## 🛠️ Tech Stack

| Layer          | Technology                                  |
|----------------|----------------------------------------------|
| Framework      | Next.js 15 (App Router), React 19             |
| Styling        | Tailwind CSS                                  |
| Auth           | NextAuth.js (Credentials + JWT)               |
| Database       | MongoDB with Mongoose                         |
| Image storage  | Cloudinary                                    |
| Icons          | react-icons                                   |
| Deployment     | Render                                        |

---

## 📁 Project Structure (key files)

```
app/
├── layout.js                        # Root layout, wraps app in AuthProvider + Header
├── page.js                          # Home page — featured offers, rentals, sales
├── globals.css                      # Tailwind base styles
├── about/page.jsx                   # About page
├── search/page.jsx                  # Search & filter listings
├── sign-in/page.jsx                 # Sign in page
├── sign-up/page.jsx                 # Sign up page
├── dashboard/page.jsx               # User dashboard (manage own listings)
├── create-listing/page.jsx          # Create a new listing
├── update-listing/[id]/page.jsx     # Edit an existing listing
├── listing/[id]/page.jsx            # Public listing detail page
└── api/
    ├── auth/[...nextauth]/route.js  # NextAuth config (Credentials provider)
    ├── auth/register/route.js       # User registration endpoint
    ├── upload/route.js              # Cloudinary image upload endpoint
    └── listing/
        ├── create/route.js          # Create listing
        ├── get/route.js             # Fetch/search/filter listings
        ├── update/route.js          # Update a listing (owner only)
        └── delete/route.js          # Delete a listing (owner only)
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- A MongoDB database (e.g. MongoDB Atlas)
- A Cloudinary account

---

## 🔑 Environment Variables Reference

| Variable                  | Description                                              |
|----------------------------|------------------------------------------------------------|
| `MONGODB_URI`             | MongoDB connection string                                  |
| `NEXTAUTH_SECRET`         | Secret used to sign/encrypt NextAuth JWTs                  |
| `NEXTAUTH_URL`            | Base URL of the app (used by NextAuth)                     |
| `URL`                     | Base URL used for server-side fetches within the app       |
| `CLOUDINARY_CLOUD_NAME`   | Cloudinary cloud name                                      |
| `CLOUDINARY_API_KEY`      | Cloudinary API key                                         |
| `CLOUDINARY_API_SECRET`   | Cloudinary API secret                                      |

---

## 📸 Image Uploads

Images are uploaded via `POST /api/upload` as `multipart/form-data` (field name `files`). The endpoint requires an authenticated session, enforces a maximum of 6 files per request, and a 2MB size limit per file, then stores each image in Cloudinary under the `next-estate` folder with automatic quality/format optimization.

## 🔍 Listing Search API

`POST /api/listing/get` accepts a JSON body to filter and sort listings:

```json
{
  "searchTerm": "downtown",
  "type": "rent",
  "parking": true,
  "furnished": false,
  "offer": false,
  "sort": "regularPrice",
  "order": "asc",
  "startIndex": 0,
  "limit": 9
}
```

It can also be used to fetch a single listing by `listingId`, or all listings owned by a user via `userId`.

---

## 📄 License

This project is available for personal and educational use. Feel free to fork and adapt it.

Sahand Estate — Documentation

Live demo: https://real-estate-hsno.onrender.com/ 
Repository: https://github.com/James1oliveira/real-estate

Contents
Business Use Case
Technical Specification
User Guide
1. Business Use Case

Problem: Independent landlords and small agencies lack an affordable, self-service way to list properties online — existing platforms are costly, slow to update, and hard to manage.

Solution: Sahand Estate lets any registered user list, edit, and remove properties instantly, while visitors search and filter listings without needing an account.

Users: Property owners/agents (list & manage), buyers/renters (browse & search).

Core use cases:

Register / sign in
Browse listings and search/filter by type, price, amenities, and offers
View full listing details (photos, price, description, features)
Create, edit, and delete listings (owner only)
Manage listings from a personal dashboard
Mark a listing as a discounted "Offer"

Business rules:

Only signed-in users can create/edit/delete listings, and only their own.
Listings need 1–6 images (2MB max each).
Discount price must be lower than regular price.
A listing is either "rent" or "sale," not both.

Out of scope (current version): messaging, payments/bookings, listing approval workflow, admin panel, map search.

2. Technical Specification

Stack: Next.js 15 (App Router) + React 19, Tailwind CSS, NextAuth (Credentials/JWT), MongoDB + Mongoose, Cloudinary for images, hosted on Render.

Architecture: Server Components handle data-heavy/SEO pages (home, listing detail) with fetch(..., {cache: 'no-store'}); Client Components handle interactive forms (auth, create/update listing, dashboard, search) via internal API routes.

Data models:

User: firstName, lastName, email, password (bcrypt hash), profilePicture.
Listing: userRef, name, description, address, type (rent/sale), bedrooms, bathrooms, regularPrice, discountPrice, offer, parking, furnished, imageUrls[], timestamps.

API routes:

Route	Auth	Purpose
/api/auth/[...nextauth]	—	NextAuth login handler
/api/auth/register	No	Create user account
/api/upload	Yes	Upload ≤6 images (≤2MB) to Cloudinary
/api/listing/create	Yes	Create listing
/api/listing/get	No	Search/filter/paginate listings, or fetch by id/user
/api/listing/update	Yes (owner)	Update listing
/api/listing/delete	Yes (owner)	Delete listing

Auth: NextAuth Credentials provider, bcrypt password compare, JWT sessions carrying user.id.

Ownership check: update/delete routes compare listing.userRef to session.user.id, rejecting with 403 if mismatched.

Env vars: MONGODB_URI, NEXTAUTH_SECRET, NEXTAUTH_URL, URL, CLOUDINARY_CLOUD_NAME, CLOUDINARY_API_KEY, CLOUDINARY_API_SECRET.

Known constraints: no rate limiting, no Cloudinary cleanup on delete, dashboard fetches up to 100 listings unpaginated, no automated tests.

3. User Guide

Browsing: No account needed. Use the homepage sections (Offers, Rentals, Sales) or the Search page to filter by term, type, amenities, and offer status, and sort by price or date.

Account: Sign up with name, email, and password (6+ characters); sign in with email/password.

Creating a listing (sign-in required):

Fill in name, description, address, type, beds/baths, price (and discount price if marking as an Offer).
Upload 1–6 photos (2MB max each) — the first photo becomes the cover image.
Submit to publish immediately.

Managing listings: Your Dashboard shows stats and all your listings, with edit (pencil) and delete (trash, with confirmation) controls.

Editing: Opens a pre-filled form; update fields/photos and save.

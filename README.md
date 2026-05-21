# Secure Notes API

SE 418 — Lab 2: Secure Record Storage.

## Description
A Notes API with JWT authentication and ownership-based authorization. Users can only view, update, and delete notes they personally created. 

## The Problem
The original API allowed any authenticated user to access any note. This lab fixes that flaw by associating each note with a user and enforcing ownership checks on all CRUD operations.

## Technologies
- Node.js
- Express
- MongoDB / Mongoose
- bcrypt
- jsonwebtoken
- dotenv

## Installation
```bash
git clone  https://github.com/KwadwoDanso/secure-notes-api.git
cd secure-notes-api
npm install
```

## Environment Variables
Create a `.env` file at the project root. See `.env.example`:
- `MONGO_URI` — MongoDB connection string
- `JWT_SECRET` — any long random string
- `PORT` — server port (default 3001)

## Run
```bash
node server.js
```

## Authorization Logic
- Every note has a `user` field storing the creator's ObjectId
- `POST` automatically stamps `req.user._id` onto new notes
- `GET` filters with `Note.find({ user: req.user._id })`
- `PUT` and `DELETE` first find the note, compare `note.user` to `req.user._id`, and return `403 Forbidden` if they don't match
- ObjectId comparison uses `.toString()` on both sides for safe comparison


## Security
- `.env` is git-ignored
- Passwords hashed with bcrypt (10 salt rounds)
- JWTs expire after 2 hours
- Ownership checked before every update/delete operation

## Author
- Kwadwo

## Acknowledgements
- Per Scholas Auth module
- AI 